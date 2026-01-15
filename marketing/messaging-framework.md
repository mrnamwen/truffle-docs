# Truffle Messaging Framework

A practical guide for talking about Truffle - whether you have 10 seconds or 10 minutes.

---

## 1. Core Messaging Hierarchy

### One-Liner (10 words or less)
**"Deploy apps to your Proxmox cluster in one click."**

### Tagline Options
1. "Heroku for your own hardware"
2. "Your infrastructure, without the infrastructure headache"
3. "Skip Kubernetes. Ship faster."
4. "VM orchestration that doesn't require a DevOps team"
5. "The deploy button for Proxmox"

### Value Proposition Statement
Truffle lets small teams deploy and manage applications on their own Proxmox infrastructure with the simplicity of a managed platform - no Kubernetes expertise required, no credentials leaving your network, and deployments that take minutes instead of hours.

### Key Differentiators
- **Your infrastructure, your control**: Agent runs on-prem, credentials never leave your network. You get SaaS convenience without giving up data sovereignty.
- **No Kubernetes required**: VMs are simpler, more predictable, and easier to debug. We embrace that instead of fighting it.
- **Built for small teams**: Two-step install, OAuth login, intuitive wizards. No DevOps hire required.

---

## 2. Elevator Pitches by Length

### 10-Second (Cocktail Party)
"We make it easy to deploy apps to your own servers. Think Heroku, but running on hardware you own."

### 30-Second (Elevator)
"Truffle is VM orchestration for Proxmox. If you've got a server or cluster and you're tired of manual deployments or scared of Kubernetes, we give you one-click deployments, application templates, and a simple dashboard. Your credentials stay on your network, but you get the experience of a managed platform."

### 60-Second (Interested Prospect)
"We built Truffle because we were spending 45 minutes on every deployment - cloning VMs, running scripts, configuring services. Now it takes 3 minutes.

Truffle is a SaaS platform with an agent that runs on your Proxmox infrastructure. The agent handles all the actual work - provisioning VMs, running deployments, managing your apps. The SaaS gives you a dashboard and coordinates everything.

The key thing: your Proxmox credentials never leave your network. The agent connects out to us, not the other way around. So you get the convenience of a managed platform without handing over the keys to your infrastructure.

We're targeting small teams - 10 to 50 people - who have their own hardware but don't want to hire a DevOps person or learn Kubernetes just to deploy a Laravel app."

### 2-Minute (Conference Hallway)
"So we had this problem at our company - we were running everything on Proxmox, which is great, but deployments were painful. Clone a VM, SSH in, run a bunch of scripts, configure nginx, set up SSL, update DNS. Every deployment was 45 minutes of tedious work, and we'd inevitably mess something up.

We looked at Kubernetes, but honestly, for a team our size it felt like trading one set of problems for a worse set of problems. We just wanted to deploy apps, not become infrastructure experts.

So we built Truffle. It's an agent that runs on your Proxmox cluster and connects to our SaaS platform. You define your apps with templates, click deploy, and it handles everything - VM provisioning, configuration, the whole thing.

The architecture is important: the agent runs on your network and connects outbound to us. Your Proxmox credentials, your SSH keys, your secrets - they never leave your infrastructure. We just coordinate the work; the agent does the actual execution.

We've been using it internally for over six months now. That 45-minute deployment? Three minutes. And anyone on the team can do it, not just the one person who knows how the servers work.

We're targeting SMBs who have their own hardware - either because they need to for compliance, or cost, or just preference - but don't have the DevOps resources to manage it properly. Think a 20-person startup with a couple of servers, or a Laravel developer who outgrew Forge but doesn't want to become a full-time sysadmin."

---

## 3. Pitches by Persona

### For the Laravel Developer
"You know how Forge is great until you need something it doesn't do? Truffle is like that, but for Proxmox. You get the same one-click deploys and simple dashboard, but you're working with VMs you fully control. Need a weird PHP extension? Custom nginx config? Go for it. And if you're running multiple projects, you can spin up isolated environments in seconds instead of configuring everything on one server."

### For the Overwhelmed CTO
"Your team shouldn't need to understand Kubernetes to ship features. Truffle gives you the deployment automation and infrastructure visibility you need without the complexity tax. Two-step install, works with your existing Proxmox setup, and your developers get a 'deploy' button instead of a runbook. We've seen teams go from 45-minute deployments to 3 minutes."

### For the Sysadmin Managing Too Much
"How much of your week is spent on deployment tasks that should be automated? Truffle handles the repetitive stuff - VM provisioning, app deployment, configuration - so you can focus on actual infrastructure work. It works across multiple Proxmox clusters, gives you a single dashboard, and the agent architecture means you stay in control. No SaaS vendor gets credentials to your systems."

### For the Cost-Conscious Founder
"You've got hardware. Maybe it's a Hetzner dedicated server, maybe it's a rack in a colo. You don't want to pay cloud prices, but you also don't want to spend engineering time on deployment automation. Truffle is a managed platform experience for infrastructure you own. Keep your low hosting costs, lose the deployment headaches."

---

## 4. Pitches by Context

### Cold Intro at Conference
"Hey, I'm [name] from Truffle. We do VM orchestration for Proxmox - basically one-click deployments for your own hardware. Are you folks self-hosting anything, or all cloud?"

*[Wait for response, adjust accordingly]*

### Response to "What do you do?"
"I work on Truffle - we make it easy to deploy apps to Proxmox servers. Like Heroku, but for your own hardware."

*[If they seem interested]:* "The idea is you shouldn't need a DevOps team or Kubernetes expertise just to deploy a web app to servers you own."

### Forum/Reddit Mention
"We built Truffle for exactly this use case - [specific thing they mentioned]. It's an agent that runs on your Proxmox cluster and gives you one-click deployments through a dashboard. Your credentials stay on your network since the agent handles all the actual infrastructure work. Happy to answer questions if you want to know more."

### Email Outreach (If Ever Used)
Subject: Simpler Proxmox deployments

Hi [name],

Saw you're running [app/service] on Proxmox. We built Truffle to make that easier - one-click deployments, application templates, multi-cluster management.

The agent runs on your infrastructure, so your credentials never leave your network. We just provide the dashboard and coordination.

Would a 3-minute deployment instead of a 45-minute one be useful for your team?

[name]

### Twitter Bio / Profile
Options:
- "One-click deployments for Proxmox. Your hardware, our automation."
- "Heroku for your own infrastructure. No K8s required."
- "VM orchestration for teams who'd rather ship than configure."

---

## 5. Story Frameworks

### The Origin Story (45 min to 3 min)
"We were a small team running our apps on Proxmox. Every deployment was the same drill: clone a VM template, SSH in, pull the code, install dependencies, configure nginx, set up SSL, update the load balancer. Forty-five minutes, every single time. And if someone was out sick, nobody else knew how to do it.

We looked at Kubernetes. Spent a month learning it. Realized we'd traded 45 minutes of deployment work for a full-time job managing Kubernetes.

So we built something simpler. An agent that runs on the Proxmox cluster and does all that deployment work automatically. Define your app once, click deploy, done in 3 minutes.

That tool became Truffle, and now we're making it available to other small teams who are stuck in the same spot we were."

### The "Why We Built This" Narrative
"There's this gap in the market. On one side, you've got managed platforms - Heroku, Render, Railway - that are great until you need control or can't justify the cost. On the other side, you've got Kubernetes, which is incredibly powerful but requires serious expertise to run well.

In the middle are all these small teams with their own hardware - a Proxmox cluster, some dedicated servers, whatever - who just want to deploy their apps without it being a whole project.

That's who we built Truffle for. People who want managed-platform simplicity on infrastructure they own and control."

### Customer Success Story Template
**The Setup**: "[Company/person] was running [what] on Proxmox. They had [number] of [servers/VMs] and deployments were taking [time]."

**The Problem**: "Every deployment meant [specific painful steps]. When [person who knew how] was unavailable, [bad thing that happened]. They looked at [alternative] but [why it didn't work]."

**The Solution**: "With Truffle, they [specific improvement]. Deployments went from [old time] to [new time]. Now [benefit - anyone can deploy / they have visibility / etc.]."

**The Quote**: "[Something specific about the improvement, in their words]"

---

## 6. Words and Phrases

### Words to Use (and Why)

| Word/Phrase | Why |
|-------------|-----|
| "Your infrastructure" | Emphasizes ownership and control |
| "One-click" | Implies simplicity without being vague |
| "On-prem agent" | Technical accuracy that builds trust |
| "Credentials stay on your network" | Addresses the obvious SaaS concern |
| "Templates" | Familiar concept, implies reusability |
| "Small teams" | Specific, relatable, not trying to be everything |
| "Ship" / "Deploy" | Action-oriented, what they actually want to do |
| "Minutes, not hours" | Concrete improvement |
| "No DevOps hire required" | Addresses real cost/resource concern |

### Words to Avoid (and Why)

| Word/Phrase | Why | Use Instead |
|-------------|-----|-------------|
| "Revolutionary" / "Disruptive" | Corporate buzzword, means nothing | Be specific about the improvement |
| "Enterprise-grade" | We're targeting SMBs, this sounds expensive | "Production-ready" or just describe the feature |
| "Seamless" | Overused, vague | Describe the actual experience |
| "Leverage" | Corporate speak | "Use" |
| "Ecosystem" | Vague, often meaningless | Be specific about what integrates |
| "Best-in-class" | Unsubstantiated superlative | Show, don't tell |
| "Cloud-native" | We're explicitly not that | "Runs on your infrastructure" |
| "DevOps" (as something we do) | We're the alternative to needing DevOps | "Deployment automation" |
| "AI-powered" | We're not, and it's overused | Just don't |

### Jargon Translation Guide

| Technical Term | Plain English |
|----------------|---------------|
| VM orchestration | Managing your virtual machines automatically |
| Provisioning | Creating and setting up new VMs |
| Agent architecture | Software that runs on your servers and does the work |
| SaaS control plane | Our dashboard that coordinates everything |
| Templates | Pre-configured setups for common apps |
| Multi-cluster | Works across multiple Proxmox servers |
| On-premises | Running on your own hardware, not ours |
| Credentials | Passwords and access keys for your infrastructure |

---

## 7. Proof Points to Mention

### Internal Usage
- "We've been using Truffle in production internally for over 6 months"
- "Built because we needed it - we're customers zero"

### Specific Metrics
- "45-minute deployments reduced to 3 minutes" (15x improvement)
- "Two-step install process"
- "Agent installs in under 5 minutes"

### Architecture Trust Signals
- "Agent connects outbound - we never need inbound access to your network"
- "Credentials stored with AES-256-GCM encryption, on your infrastructure"
- "Open source agent - you can audit exactly what runs on your systems" *(if applicable)*

### Technology Credibility
- "Built on Proxmox VE API"
- "TypeScript/Node.js stack"
- "Supports [specific features that matter to the audience]"

### Social Proof (As Available)
- Number of deployments processed
- Number of VMs managed
- Community size (GitHub stars, Discord members, etc.)
- Notable users (if they've agreed to be mentioned)

---

## Quick Reference Card

**When someone asks "What's Truffle?"**
> "One-click deployments for Proxmox. Like Heroku, but for your own hardware."

**When someone asks "Why not just use Kubernetes?"**
> "You could. But if you just want to deploy apps and don't want to become a Kubernetes expert, we're simpler. VMs are predictable and easy to debug."

**When someone asks "Why not just use [Heroku/Render/etc]?"**
> "Those are great. But if you've got your own hardware - for cost, compliance, or control reasons - and want that same experience, that's us."

**When someone asks "Is it secure?"**
> "Agent runs on your network, credentials never leave. Agent connects out to us, we never need inbound access. You can audit exactly what it does."

**When someone asks "Who's it for?"**
> "Small teams with their own Proxmox infrastructure who don't have dedicated DevOps. Think 10-50 person companies, solo developers with servers, MSPs managing client infrastructure."
