# Truffle Pricing Model

## Overview

Truffle uses a **GitHub-style hybrid pricing model**: well-defined subscription tiers with included quotas, plus optional add-on packs to extend specific limits based on business needs.

This approach provides:
- **Predictability**: SMBs know exactly what they'll pay each month
- **Flexibility**: Grow specific resources without upgrading entire tier
- **Transparency**: No surprise overages or usage-based billing complexity

---

## Subscription Tiers

### Free Tier
**$0/month** — For hobbyists and evaluation

| Resource | Included |
|----------|----------|
| Agents | 1 |
| VMs | 3 |
| Team members | 1 (owner only) |
| Deployments/month | 25 |
| Templates | Community only |
| Support | Community (Discord) |

**Limitations:**
- No SSO/SAML
- No audit logs
- No API access
- Truffle branding on preview URLs

---

### Pro Tier
**$29/month** — For small teams and startups

| Resource | Included |
|----------|----------|
| Agents | 3 |
| VMs | 15 |
| Team members | 5 |
| Deployments/month | 200 |
| Templates | Community + Private |
| API access | Yes |
| Support | Email (48h response) |

**Features:**
- Full API access
- Private templates
- Basic webhooks (5 endpoints)
- Slack/Discord notifications
- 7-day audit log retention

---

### Team Tier
**$79/month** — For growing teams

| Resource | Included |
|----------|----------|
| Agents | 10 |
| VMs | 50 |
| Team members | 15 |
| Deployments/month | 1,000 |
| Templates | Community + Private + Shared |
| Support | Priority email (24h response) |

**Features:**
- Everything in Pro
- Role-based access control (RBAC)
- Unlimited webhooks
- All notification channels
- 30-day audit log retention
- Cost tracking & budgets
- Drift detection

---

### Enterprise Tier
**Custom pricing** — For large organizations

| Resource | Included |
|----------|----------|
| Agents | Unlimited |
| VMs | Unlimited |
| Team members | Unlimited |
| Deployments/month | Unlimited |
| Support | Dedicated CSM, 4h response SLA |

**Features:**
- Everything in Team
- SSO/SAML integration
- OIDC authentication
- 1-year audit log retention
- Custom data retention policies
- On-premises control plane option
- Custom SLAs
- Invoice billing
- Security questionnaire support

---

## Add-On Packs

Add-ons allow customers to extend specific resource limits without upgrading their entire tier. Available on Pro and Team tiers.

### Agent Packs

| Pack | Price | Description |
|------|-------|-------------|
| +3 Agents | $15/month | Add 3 additional agents to your quota |
| +10 Agents | $40/month | Add 10 additional agents (save $10) |

### VM Packs

| Pack | Price | Description |
|------|-------|-------------|
| +10 VMs | $20/month | Add 10 additional VMs to your quota |
| +25 VMs | $45/month | Add 25 additional VMs (save $5) |
| +50 VMs | $80/month | Add 50 additional VMs (save $20) |

### Team Member Packs

| Pack | Price | Description |
|------|-------|-------------|
| +5 Members | $10/month | Add 5 additional team members |
| +15 Members | $25/month | Add 15 additional team members (save $5) |

### Deployment Packs

| Pack | Price | Description |
|------|-------|-------------|
| +500 Deployments | $15/month | Add 500 deployments/month |
| Unlimited Deployments | $35/month | Remove deployment limits entirely |

### Feature Add-Ons

| Add-On | Price | Description |
|--------|-------|-------------|
| Priority Support | $25/month | Upgrade to 24h response time |
| Extended Audit Logs | $20/month | 90-day audit log retention |
| Advanced Analytics | $30/month | Cost forecasting, trend analysis |

---

## Billing Details

### Billing Cycle
- All plans are billed monthly
- Annual billing available with 2 months free (20% discount)
- Add-ons are prorated when added mid-cycle

### Usage Tracking
Resources are tracked in real-time:
- **Agents**: Count of registered, active agents
- **VMs**: Count of VMs managed through Truffle (any state)
- **Team members**: Count of users with organization access
- **Deployments**: Count of deployment operations initiated

### Soft Limits vs Hard Limits
- **Soft limits** (VMs, Deployments): Warning at 80%, notification at 100%
- **Hard limits** (Agents, Team members): Cannot exceed without add-on

When approaching limits:
1. Dashboard shows usage warnings
2. Email notification at 80% usage
3. At 100%: prompt to add pack or upgrade tier

### Downgrades
- Downgrades take effect at next billing cycle
- If current usage exceeds new tier limits, must reduce resources first
- Add-ons can be removed anytime (prorated credit)

---

## Implementation Notes

### Database Schema

```sql
-- Subscription tiers
CREATE TABLE subscription_tiers (
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  price_monthly INTEGER NOT NULL, -- cents
  price_annual INTEGER NOT NULL,  -- cents
  limits JSONB NOT NULL,          -- {agents: 3, vms: 15, ...}
  features JSONB NOT NULL,        -- {api: true, sso: false, ...}
  created_at TIMESTAMP DEFAULT NOW()
);

-- Add-on definitions
CREATE TABLE addon_types (
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  resource_type TEXT NOT NULL,    -- 'agents', 'vms', 'members', 'deployments'
  quantity INTEGER NOT NULL,      -- amount added
  price_monthly INTEGER NOT NULL, -- cents
  tier_requirements TEXT[],       -- ['pro', 'team']
  created_at TIMESTAMP DEFAULT NOW()
);

-- Organization subscriptions
CREATE TABLE organization_subscriptions (
  id TEXT PRIMARY KEY,
  organization_id TEXT REFERENCES organizations(id),
  tier_id TEXT REFERENCES subscription_tiers(id),
  status TEXT NOT NULL,           -- 'active', 'canceled', 'past_due'
  billing_cycle TEXT NOT NULL,    -- 'monthly', 'annual'
  current_period_start TIMESTAMP,
  current_period_end TIMESTAMP,
  stripe_subscription_id TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Active add-ons
CREATE TABLE organization_addons (
  id TEXT PRIMARY KEY,
  organization_id TEXT REFERENCES organizations(id),
  addon_type_id TEXT REFERENCES addon_types(id),
  quantity INTEGER DEFAULT 1,     -- can buy multiple of same pack
  status TEXT NOT NULL,
  stripe_subscription_item_id TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Usage tracking
CREATE TABLE usage_snapshots (
  id TEXT PRIMARY KEY,
  organization_id TEXT REFERENCES organizations(id),
  period_start TIMESTAMP NOT NULL,
  period_end TIMESTAMP NOT NULL,
  agents_count INTEGER DEFAULT 0,
  vms_count INTEGER DEFAULT 0,
  members_count INTEGER DEFAULT 0,
  deployments_count INTEGER DEFAULT 0,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### API Endpoints

```
GET  /api/billing/subscription          -- Current subscription details
POST /api/billing/subscription          -- Create/upgrade subscription
PUT  /api/billing/subscription          -- Modify subscription
DELETE /api/billing/subscription        -- Cancel subscription

GET  /api/billing/addons                -- List active add-ons
POST /api/billing/addons                -- Add an add-on pack
DELETE /api/billing/addons/:id          -- Remove an add-on

GET  /api/billing/usage                 -- Current usage vs limits
GET  /api/billing/usage/history         -- Historical usage data

GET  /api/billing/invoices              -- List invoices
GET  /api/billing/invoices/:id          -- Get invoice details

POST /api/billing/portal                -- Get Stripe customer portal URL
```

### UI Components

```
src/components/settings/billing/
├── SubscriptionSettings.tsx     -- Current plan, upgrade options
├── AddonsSettings.tsx           -- Manage add-on packs
├── UsageDisplay.tsx             -- Usage meters with limits
├── InvoiceHistory.tsx           -- Past invoices
└── BillingPortalButton.tsx      -- Link to Stripe portal
```

---

## Competitive Positioning

| Competitor | Model | Truffle Advantage |
|------------|-------|-------------------|
| Railway | Usage-based | Predictable bills for SMBs |
| Vercel | Tier + overages | No surprise overage charges |
| Rancher | Per-CPU/vCPU | Simpler, no CPU counting |
| Portainer | Per-node flat | More granular resource control |
| VMware Aria | Enterprise negotiated | Transparent, self-serve pricing |

---

## Future Considerations

### Template Marketplace Revenue Share
When template marketplace launches:
- Free templates: No charge
- Premium templates: Creator sets price, Truffle takes 20%
- Template subscriptions: Monthly fee for template updates

### Usage-Based Option (Future)
For customers preferring pay-as-you-go:
- Optional "Flex" billing mode
- Pay per VM-hour, deployment, API call
- Requires explicit opt-in (not default)
- Higher per-unit cost than committed tiers
