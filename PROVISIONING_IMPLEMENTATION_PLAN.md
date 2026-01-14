# Provisioning Implementation Plan

## Overview

This document outlines the full implementation plan for wiring up real VM provisioning in Truffle. Currently, the UI creates database records but doesn't actually provision VMs on hypervisors.

## Current State

### What Works
- Agent has a working `provision` command handler (`truffle-agent/src/agent.ts:598`)
- Agent can clone Proxmox templates and configure VMs
- Control plane has `POST /api/agents/:id/provision` endpoint
- Control plane has `POST /api/commands` for sending commands to agents

### What's Mock/Broken
- `ProvisionNodeModal` uses `setTimeout` to fake provisioning stages
- Visual Template Builder creates DB records but no real VMs
- No hypervisor selection in provisioning flow
- No mapping between UI template UUIDs and Proxmox template IDs
- Node records created without `vmId`, `hypervisorId`, `ipAddress`

## Architecture

### Data Flow
```
User clicks "Provision Node"
    ↓
ProvisionNodeModal (select hypervisor + template config)
    ↓
POST /api/commands { agentId, type: 'provision', params }
    ↓
Control Plane sends command via WebSocket to Agent
    ↓
Agent executes provision:
  1. Clone Proxmox template
  2. Configure VM (memory, cores, network)
  3. Start VM
  4. Wait for network/IP
  5. Run post-provision commands
  6. Report result back
    ↓
Control Plane receives result
    ↓
Update Node record with vmId, hypervisorId, ipAddress, status='running'
    ↓
UI polls command status and shows real progress
```

### Agent Provision Parameters
```typescript
// What agent expects (from truffle-agent/src/types.ts)
interface ProvisionParams {
  name: string;
  template: {
    type: string;
    template_id?: number;      // Proxmox template VMID (e.g., 9000)
    template_node?: string;    // Proxmox node where template lives
    memory: number;            // MB
    cores: number;
    description?: string;
    network_bridge?: string;   // e.g., 'vmbr0'
    pre_provision_commands?: string[];
    post_provision_commands?: string[];
  };
  target_node?: string;        // Proxmox node to create VM on
  min_vm_id?: number;
  skip_post_provision?: boolean;
}

// What agent returns
interface ProvisionResult {
  vm_id: number;
  node: string;
  ip_address: string;
  name: string;
  status: 'completed' | 'partial';
  stages_completed: string[];
}
```

## Implementation Tasks

### Phase 1: Hypervisor Selection

**File: `truffle-ui/src/components/modals/ProvisionNodeModal.tsx`**

1. Add hypervisor selection step before template selection
2. Fetch available hypervisors via `useHypervisors()` hook
3. Only show connected hypervisors
4. Store selected hypervisor in modal state

```typescript
// New state
const [selectedHypervisor, setSelectedHypervisor] = useState<HypervisorResponse | null>(null);

// New step in wizard
type Step = 'hypervisor' | 'template' | 'configure' | 'review' | 'provisioning';
```

### Phase 2: Template Mapping

**Database Change: Templates need `proxmoxTemplateId`**

The `templates` table already has `proxmox_template_id` column. Need to:
1. Populate this for system templates
2. Allow users to set it for custom templates
3. Fetch Proxmox templates from agent to help mapping

**File: `truffle-control-plane/src/api/routes.ts`**

Add endpoint to list Proxmox templates from an agent:
```typescript
app.get('/api/agents/:id/templates', async (request, reply) => {
  // Send 'list_templates' command to agent
  // Return available Proxmox templates
});
```

**File: `truffle-agent/src/agent.ts`**

Add handler for listing Proxmox templates:
```typescript
case 'list_templates':
  result = await this.handleListTemplates();
  break;

private async handleListTemplates(): Promise<Record<string, unknown>> {
  const provider = this.getHypervisor();
  // Call Proxmox API to list templates
  // Return { templates: [...] }
}
```

### Phase 3: Real Provision Command

**File: `truffle-ui/src/components/modals/ProvisionNodeModal.tsx`**

Replace mock provisioning with real API call:

```typescript
import { commandsApi } from '@/lib/api';

const handleStartProvisioning = async () => {
  setStep('provisioning');

  try {
    // Send provision command
    const { commandId } = await commandsApi.send({
      agentId: selectedHypervisor!.agentId,
      type: 'provision',
      params: {
        name: nodeName,
        template: {
          type: 'clone',
          template_id: selectedTemplate!.proxmoxTemplateId,
          memory: memory,
          cores: cores,
          post_provision_commands: selectedTemplate!.provisionCommands,
        },
      },
    });

    setCommandId(commandId);
    // Start polling for status
    pollCommandStatus(commandId);
  } catch (error) {
    setProvisionError(error.message);
  }
};

const pollCommandStatus = async (commandId: string) => {
  const pollInterval = setInterval(async () => {
    const command = await commandsApi.get(commandId);

    // Update stages based on command.progress
    if (command.progress) {
      updateStagesFromProgress(command.progress);
    }

    if (command.status === 'completed') {
      clearInterval(pollInterval);
      handleProvisionComplete(command.result);
    } else if (command.status === 'failed') {
      clearInterval(pollInterval);
      setProvisionError(command.error?.message);
    }
  }, 1000);
};
```

### Phase 4: Progress Reporting

**File: `truffle-agent/src/agent.ts`**

The agent already sends progress updates. Ensure stages are reported:
```typescript
// In handleProvision method
await this.sendProgress(commandId, {
  stage: 'clone_template',
  percent: 20,
  message: 'Cloning template...',
});
```

**File: `truffle-ui/src/components/modals/ProvisionNodeModal.tsx`**

Map progress stages to UI:
```typescript
const stageMapping: Record<string, string> = {
  'clone_template': 'Creating VM from template',
  'configure_vm': 'Configuring VM settings',
  'start_vm': 'Starting VM',
  'wait_network': 'Waiting for network',
  'post_provision': 'Running initialization scripts',
  'register': 'Registering with control plane',
};

const updateStagesFromProgress = (progress: { stage: string; percent: number }) => {
  setStages(prev => prev.map(s => ({
    ...s,
    status: s.id === progress.stage ? 'running'
          : stageOrder.indexOf(s.id) < stageOrder.indexOf(progress.stage) ? 'completed'
          : 'pending'
  })));
};
```

### Phase 5: Node Record Updates

**File: `truffle-control-plane/src/api/routes.ts`**

When provision completes, update node record:
```typescript
// In command completion handler or as separate endpoint
app.post('/api/nodes/:id/provision-complete', async (request, reply) => {
  const { vmId, hypervisorId, ipAddress } = request.body;

  await db.nodes.update(request.params.id, {
    vmId,
    hypervisorId,
    ipAddress,
    status: 'running',
  });
});
```

**Alternative: Have UI update node after provision completes**
```typescript
// In ProvisionNodeModal after successful provision
const handleProvisionComplete = async (result: ProvisionResult) => {
  // Create node record if new, or update existing
  if (existingNodeId) {
    await nodesApi.update(existingNodeId, {
      vmId: result.vm_id.toString(),
      hypervisorId: selectedHypervisor!.agentId,
      ipAddress: result.ip_address,
      status: 'running',
    });
  } else {
    await nodesApi.create({
      applicationId,
      name: result.name,
      templateId: selectedTemplate!.id,
      vmId: result.vm_id.toString(),
      hypervisorId: selectedHypervisor!.agentId,
      ipAddress: result.ip_address,
      status: 'running',
    });
  }
};
```

### Phase 6: Provision Existing Nodes

For nodes created by Visual Template Builder that exist in DB but not as VMs:

**File: `truffle-ui/src/pages/ApplicationDetail.tsx`**

Add "Provision" action to nodes without `vmId`:
```typescript
// In NodesTable or node row actions
{!node.vmId && (
  <Button onClick={() => openProvisionModal(node)}>
    Provision
  </Button>
)}
```

**File: `truffle-ui/src/components/modals/ProvisionNodeModal.tsx`**

Accept optional `existingNode` prop:
```typescript
interface ProvisionNodeModalProps {
  // ... existing props
  existingNode?: Node; // If provided, provision this existing node
}

// Pre-fill form from existing node
useEffect(() => {
  if (existingNode) {
    setNodeName(existingNode.name);
    // Look up template by ID
    const template = templates.find(t => t.id === existingNode.templateId);
    if (template) {
      setSelectedTemplate(template);
      setMemory(existingNode.memory || template.defaultMemory);
      setCores(existingNode.cores || template.defaultCores);
    }
  }
}, [existingNode]);
```

## Database Schema Updates

### Nodes Table
Ensure columns exist (should already be there):
```sql
ALTER TABLE nodes ADD COLUMN IF NOT EXISTS vm_id TEXT;
ALTER TABLE nodes ADD COLUMN IF NOT EXISTS hypervisor_id TEXT;
ALTER TABLE nodes ADD COLUMN IF NOT EXISTS ip_address TEXT;
ALTER TABLE nodes ADD COLUMN IF NOT EXISTS memory INTEGER DEFAULT 2048;
ALTER TABLE nodes ADD COLUMN IF NOT EXISTS cores INTEGER DEFAULT 2;
```

### Templates Table
Already has `proxmox_template_id` - ensure it's populated:
```sql
-- Seed with common template IDs
UPDATE templates SET proxmox_template_id = 9000 WHERE id = 'web-api-worker';
UPDATE templates SET proxmox_template_id = 9001 WHERE id = 'web-api-horizon';
-- etc.
```

## API Endpoints Summary

### New Endpoints Needed
| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/agents/:id/templates` | List Proxmox templates from agent |
| POST | `/api/nodes/:id/provision-complete` | Update node after provision (optional) |

### Existing Endpoints to Use
| Method | Path | Description |
|--------|------|-------------|
| POST | `/api/commands` | Send provision command to agent |
| GET | `/api/commands/:id` | Poll command status |
| PUT | `/api/nodes/:id` | Update node record |

## UI Component Changes

### ProvisionNodeModal
- Add hypervisor selection step
- Replace mock setTimeout with real API calls
- Poll command status for progress
- Handle success/error states
- Update node record on completion

### ApplicationDetail / NodesTable
- Show "Provision" button for nodes without vmId
- Show hypervisor name for provisioned nodes
- Disable actions on unprovisioned nodes

### Visual Template Builder
- Consider: Don't create node records, only create application + templates
- Or: Mark nodes as "pending provision" vs "provisioned"

## Error Handling

1. **Hypervisor not connected**: Show error, suggest checking agent
2. **Template not found on Proxmox**: Help user map template IDs
3. **Provision failed**: Show error from agent, allow retry
4. **Network timeout**: Allow user to check VM manually

## Testing Plan

1. Create application via Visual Template Builder
2. Click "Provision Node" on unprovisioned node
3. Select hypervisor
4. Verify command sent to agent
5. Watch real progress updates
6. Verify VM created in Proxmox
7. Verify node record updated with vmId, ipAddress
8. Verify node shows as "running" in UI

## Migration Notes

Existing "phantom" nodes (created by import but never provisioned):
- Keep in DB with status "stopped" and no vmId
- UI should indicate they need provisioning
- User can provision or delete them
