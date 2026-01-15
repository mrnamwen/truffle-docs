# Laracon 2026 Preparation and Networking Guide

*A practical playbook for representing Truffle at Laracon. Review this on the flight there.*

---

## Table of Contents
1. [Pre-Conference Preparation](#1-pre-conference-preparation)
2. [Elevator Pitches](#2-elevator-pitches)
3. [Conversation Starters and Qualifying Questions](#3-conversation-starters-and-qualifying-questions)
4. [Demo Script](#4-demo-script)
5. [Handling Objections](#5-handling-objections)
6. [Follow-Up Strategy](#6-follow-up-strategy)
7. [Networking Tactics](#7-networking-tactics)

---

## 1. Pre-Conference Preparation

### What to Have Ready

**Physical Materials**
- [ ] Business cards (200+) - include QR code to demo signup
- [ ] One-pager (50 copies) - folded, pocket-sized
- [ ] Stickers with Truffle logo (people love stickers)

**Digital Assets**
- [ ] Demo environment running and tested (see Laptop Setup below)
- [ ] Landing page with Laracon-specific offer ready
- [ ] Short video demo (2 min) for when live demo isn't possible

**One-Pager Content:**
```
TRUFFLE - VM Orchestration for Laravel Teams

The Problem:
- Forge is great, but you don't own your infrastructure
- Kubernetes is overkill for most Laravel apps
- Manual deployments are slow and error-prone

The Solution:
- Full VM lifecycle: provision, deploy, monitor
- Works with YOUR Proxmox infrastructure
- 3-minute deployments (vs 45-min K8s builds)

Pricing: Flat monthly rate. No per-server fees.

[QR Code] → truffle.dev/laracon
```

### Laptop Setup for Impromptu Demos

**The night before the conference:**

1. **Verify demo environment**
   - Control plane running and accessible
   - At least one agent connected and healthy
   - Test VM template ready for quick provisioning
   - Sample Laravel app ready to deploy

2. **Offline fallback**
   - Screen recording of full demo flow (in case of wifi issues)
   - Screenshots of key UI moments

3. **Browser setup**
   - Truffle UI in one tab, pre-logged-in
   - Proxmox dashboard in another (to show the orchestration happening)
   - Clear browser history, close personal tabs
   - Increase font size for showing others

4. **Terminal setup**
   - Clean terminal profile (no embarrassing aliases)
   - SSH keys configured for demo server
   - Increase terminal font size

**Pro tip:** Create a "Demo Mode" user account on your laptop with nothing but demo-related tools.

### Social Media Before the Event

**2 weeks before:**
- Tweet/post: "Heading to Laracon 2026! Anyone else frustrated with their current deployment setup? Let's chat."
- Engage with #Laracon2026 hashtag
- Comment on speaker announcements

**1 week before:**
- "Working on something that might interest Laravel devs who run their own infrastructure. Find me at Laracon if you want a sneak peek."
- DM people you specifically want to meet

**Day before/of:**
- "Just landed! Always happy to talk deployments, Proxmox, or why I stopped using Kubernetes for Laravel apps. DM me."

---

## 2. Elevator Pitches

### 10-Second: "What is Truffle?"

> "Truffle is like Forge, but you own the infrastructure. We orchestrate VMs on your Proxmox servers - you get the simplicity without the lock-in."

*Use this when someone asks "So what do you do?" in passing.*

### 30-Second: The Value Prop

> "We built Truffle for Laravel teams who want Forge-like simplicity but need to run their own infrastructure. Maybe it's compliance, cost, or just wanting more control.
>
> You install our agent on your Proxmox cluster, and suddenly you've got one-click provisioning, zero-downtime deployments, and real-time monitoring - all through a clean web UI. We cut our own deployment times from 45 minutes to 3 minutes."

*Use this when you have someone's attention in a hallway conversation.*

### 2-Minute: The Full Story

> "So, we were a Laravel shop running everything on Kubernetes. Every deployment took 45 minutes. Container builds, rolling updates, the whole dance. For a simple Laravel CRUD app. It was absurd.
>
> We looked at Forge, and honestly, it's great. But we had compliance requirements that meant we needed to own our infrastructure. Plus, paying per-server adds up fast when you're managing 50+ VMs.
>
> So we built Truffle. It's a SaaS control plane plus an agent you install on your Proxmox cluster. The agent does the heavy lifting - talking to the Proxmox API, running deployments over SSH, monitoring services. The control plane gives you a beautiful UI and coordinates everything.
>
> The magic moment is watching a new VM spin up in under a minute, your Laravel app deploy automatically, and everything just work. No YAML files. No container orchestration. Just VMs doing what VMs do, but automated.
>
> We're targeting small-to-medium Laravel teams. People who find Kubernetes overkill but need more than FTP-and-pray. And unlike Forge, it's a flat monthly rate. Whether you have 5 servers or 500."

*Use this when someone says "Tell me more" and you're sitting down together.*

---

## 3. Conversation Starters and Qualifying Questions

### Natural Entry Points

**At talks about deployment/DevOps:**
- "What's your current deployment setup?"
- "How long do your deployments typically take?"
- "Are you happy with Forge, or just tolerating it?"

**At meals/social events:**
- "What kind of infrastructure do you run on?"
- "Have you looked at Proxmox? We've been getting great results with it."
- "What's the most painful part of your DevOps workflow?"

**After someone mentions server management:**
- "Do you manage your own servers or use something like Forge?"
- "What made you choose [their solution] over alternatives?"

### Qualifying Questions (Find Good Prospects)

Ask these to determine if someone is worth a longer conversation:

1. **"Do you run your own servers, or are you fully in the cloud?"**
   - Own servers / Proxmox / on-prem = Good prospect
   - All AWS/GCP with managed services = Probably not

2. **"How do you handle deployments today?"**
   - Manual / painful / "we wrote our own scripts" = Great prospect
   - "We love Forge" = Maybe, probe deeper on pain points
   - "We have a dedicated DevOps team" = Probably too big

3. **"How many servers/VMs are you managing?"**
   - 5-100 = Sweet spot
   - 1-4 = Might not need us
   - 100+ = Enterprise sales cycle, different conversation

4. **"What do you wish was easier about your current setup?"**
   - Let them tell you the pain point, then connect it to Truffle

### Red Flags (Politely Move On)

- "We're 100% serverless" - Not our market
- "Our DevOps team handles everything" - They're not the buyer
- "We just use Heroku/Render/Railway" - They've outsourced the problem
- "I'm just learning Laravel" - Not ready for infrastructure tooling
- "We're a one-person shop" - Probably can't justify the cost

**Exit gracefully:** "That's cool! We're focused on teams managing their own infrastructure, so probably not a fit right now. But hey, if you ever go down that path, look us up."

---

## 4. Demo Script

### The 3-5 Minute Hallway Demo

**Setup line:** "Let me show you what I mean. This will take 3 minutes."

**The Flow:**

1. **Dashboard overview (30 sec)**
   - "Here's the control plane. You can see all our agents - these are running on our Proxmox cluster."
   - Point out connected agents and their health status
   - "Each agent manages one Proxmox cluster. We have [X] VMs across [Y] nodes."

2. **Provision a new VM (60 sec)**
   - "Watch this. I'm going to spin up a new VM right now."
   - Click through the provisioning UI
   - "I pick my template - this is a base Ubuntu with our standard packages - select the node, and go."
   - While it's provisioning: "The agent is talking to Proxmox's API. No SSH, no manual clicks in the Proxmox UI."
   - **Key moment:** Show the VM appearing in Proxmox dashboard

3. **Deploy an application (60 sec)**
   - "Now let's deploy a Laravel app to it."
   - Show the deployment configuration (git repo, environment)
   - Trigger deployment
   - "Under the hood, this is SSH-based. Clone, composer install, migrate, cache clear. But you don't have to script any of it."
   - **Key moment:** Show the timestamp - "See? 2 minutes 47 seconds. Used to take us 45 minutes with our K8s setup."

4. **Monitoring (30 sec)**
   - "And now it's monitored. CPU, memory, disk, plus your custom health checks."
   - Show real-time metrics
   - "If anything goes red, you know immediately."

5. **The close (30 sec)**
   - "That's it. No YAML, no Kubernetes, no paying per-server. You own everything - we just make it easy to manage."
   - "Want me to send you a link to try it yourself?"

### Key Moments That Impress

- **Speed:** The VM provisioning completing in real-time
- **Simplicity:** The clean UI vs. the Proxmox interface complexity
- **The Proxmox tab:** Showing that it's actually happening on their infrastructure
- **The deployment time:** Make sure to mention the before/after numbers

### If Demo Fails

- Have the screen recording ready: "Wifi's rough here, let me show you a recording instead"
- Pivot to conversation: "It's demo gods punishing me. But let me tell you what you would have seen..."

---

## 5. Handling Objections

### "Why not just use Forge?"

> "Forge is excellent. We actually recommend it if you don't need to own your infrastructure.
>
> The difference is ownership. With Forge, they provision and manage the servers - which is convenient, but you're locked into their ecosystem and paying per-server.
>
> With Truffle, you bring your own Proxmox cluster. You own everything. We just make it manageable. For teams with compliance requirements, or anyone who wants more control, that matters a lot.
>
> Plus, our pricing is flat-rate. Whether you're running 10 servers or 100, same price."

### "We already use Kubernetes"

> "How's that working for you? [Let them answer]
>
> We actually came from K8s ourselves. It's powerful, but for Laravel apps, we found it was massive overkill. 45-minute build times, debugging pod networking issues, the whole YAML circus.
>
> Truffle is intentionally simpler. If you genuinely need container orchestration and horizontal auto-scaling, K8s is the right tool. But if you're running Laravel apps that just need to deploy reliably and fast, VMs are honestly better.
>
> We're not anti-container. We just think most Laravel teams don't need that complexity."

### "Is this secure?"

> "Great question. A few things:
>
> First, the agent runs inside your network, connecting out to our control plane. We never have direct access to your infrastructure - the agent does the work.
>
> Second, all agent-to-control-plane communication is encrypted over WebSocket with TLS. Commands are validated on both ends.
>
> Third, credentials (Proxmox API, SSH keys) are stored encrypted on the agent using AES-256-GCM. They never touch our servers.
>
> For enterprise customers with additional requirements, we can discuss on-prem deployment of the control plane too."

### "What if you go out of business?"

> "Fair concern. Here's the thing: your VMs don't depend on us to keep running.
>
> If Truffle disappeared tomorrow, your Proxmox cluster still works. Your VMs still run. Your apps are still deployed. You'd just lose the orchestration layer.
>
> That's different from platforms where they own your servers. With Truffle, you always own the underlying infrastructure. We're a management layer, not a dependency.
>
> That said, we're well-funded and growing. But I understand wanting to know the exit path."

### "This seems early-stage / risky"

> "We're definitely still growing, but we've been running this in production for [X months/years] ourselves.
>
> We're targeting teams who are comfortable being early adopters in exchange for being able to shape the product. If you need something battle-tested with a huge support team, that's not us yet.
>
> But if you're frustrated with your current setup and want to try something new, I'd love to have you as a design partner. Early customers get a lot of say in our roadmap."

---

## 6. Follow-Up Strategy

### What to Collect from Interested People

**Minimum:**
- Name
- Email
- Company

**Better:**
- What problem they're trying to solve
- Current setup (Forge, manual, K8s, etc.)
- Rough server count
- Timeline ("exploring" vs "need something now")

**Best:**
- All the above
- Photo together (helps you remember the face)
- Specific follow-up action agreed on ("I'll send you beta access")

**Method:** Use your phone's notes app. Start a new note for each person. Voice-to-text if you need to capture quickly.

### Post-Conference Email Template

**Send within 48 hours.** Personalize the [bracketed] parts.

---

**Subject:** Great meeting you at Laracon - Truffle follow-up

Hey [Name],

It was great chatting at Laracon [specific reference - "after Taylor's keynote" / "at the after-party" / "during lunch on day 2"].

You mentioned [their specific pain point - "frustration with deployment times" / "wanting to get off Forge" / "considering Proxmox"]. I've been thinking about that, and I genuinely think Truffle could help.

Here's what I promised to send:
- [Link to demo / beta signup / video / docs - whatever you discussed]
- [Any other specific thing you promised]

Would you be up for a 20-minute call next week to dig into your setup? I'd love to see if we're a fit. Here's my calendar: [Calendly link]

Either way, hope Laracon was valuable for you. Looking forward to staying in touch.

Best,
[Your name]

---

### How to Nurture Without Being Spammy

**Week 1 post-conference:**
- Send the personal follow-up email above
- Connect on LinkedIn with a personalized note

**Week 2-3:**
- If no response, one gentle bump: "Hey, just floating this back up - let me know if you'd like to chat."
- No response to bump = move to long-term nurture

**Monthly:**
- Add to your newsletter/updates list (with permission)
- Share genuinely useful content (not just product updates)

**Quarterly:**
- Personal check-in: "Hey, how's [thing they were working on]?"
- Share relevant case study if you have one

**The rule:** Be useful, not persistent. If they go quiet, assume timing is wrong. Stay visible but don't chase.

---

## 7. Networking Tactics

### Where the Real Conversations Happen

**Not in sessions.** Sessions are for learning, not networking.

**Best opportunities:**

1. **Hallway track** - Between talks, near the coffee
2. **Lunch** - Sit with strangers, not your colleagues
3. **After-parties** - More relaxed, people are drinking, guards are down
4. **Speaker dinner** (if you can get in) - High-value contacts
5. **Hotel lobby late night** - Serious developers who want to talk shop
6. **The airport/rideshare** - Captive audience, often great conversations

**Worst opportunities:**
- During talks (don't be that person)
- When someone is clearly heading somewhere
- When someone is on their phone/laptop and focused

### How to Talk to Speakers

**Before their talk:**
- Don't. They're nervous and preparing.

**Right after their talk:**
- Brief is best: "Great talk. [One specific thing you liked]. Quick question: [something genuine]."
- Don't pitch them. They're surrounded by people.

**At social events:**
- Treat them like a normal person (they are)
- Talk about THEIR interests, not your product
- Only mention Truffle if it comes up naturally
- Goal: Build relationship, not close a sale

**Follow up:**
- "Hey, really enjoyed your talk on [topic]. Would love to buy you coffee sometime and hear more about [specific thing they mentioned]."

### Building Relationships vs. Pitching

**The ratio:** 80% relationship-building, 20% pitching (at most)

**Relationship-building looks like:**
- Asking about their work, challenges, interests
- Sharing useful knowledge freely
- Introducing them to others they should meet
- Being genuinely interested in them as a person

**Pitching looks like:**
- Talking about your product features
- Asking if they want a demo
- Qualifying them as a prospect
- Pushing for a meeting

**The balance:** Only pitch if they express genuine interest or pain that Truffle solves. Otherwise, focus on being memorable and helpful.

**The test:** At the end of the conversation, do they want to talk to you again? That matters more than whether they'll buy today.

---

## Quick Reference Card

*Print this. Put it in your badge holder.*

**10-second pitch:** "Truffle is like Forge, but you own the infrastructure."

**Qualifying questions:**
1. Do you run your own servers?
2. How do you handle deployments?
3. How many VMs?

**Demo highlights:**
1. Dashboard + agents
2. Provision VM (the speed)
3. Deploy app (the simplicity)
4. Monitoring

**Objection quick responses:**
- "Why not Forge?" → Ownership + flat pricing
- "We use K8s" → Simpler, faster for Laravel
- "Is it secure?" → Agent in your network, encrypted, you own everything
- "What if you shut down?" → Your VMs keep running, we're just the management layer

**Follow-up:**
- Collect: Name, email, company, pain point, server count
- Email within 48 hours
- Include specific reference + what you promised

---

*Good luck. Be helpful. Be human. The product sells itself if you find the right people.*
