# PIVOT: Sentient Protocol → NEUROFORGE

## Why Sentient Protocol Failed (Judge Consensus)

| Problem | Who Caught It | Severity |
|---------|--------------|----------|
| VLA autonomy impossible in 3.5 weeks | Rocky (2/10) | Fatal |
| Virtuals integration is superficial/swappable | Jansen (4/10) | Fatal |
| Proof-of-Physical-Work has replay attacks | All 4 judges | Fatal |
| No competitive moat, forkable in 90 days | Balaji (3/10 moat) | Critical |
| No GTM, zero traction, no partner commitments | Xen (3/10 GTM) | Critical |
| Liability gap for autonomous robot harm | Balaji, Xen | Critical |

Root cause: Sentient Protocol tried to solve autonomy (the hardest problem in robotics) AND build a protocol layer AND design tokenomics, all in 3.5 weeks.

---

## The Pivot: NEUROFORGE

### One-liner

The decentralized training data marketplace for humanoid robots, where humans earn crypto by teleoperating robots to create VLA training demonstrations, and each trained skill becomes a tokenized Virtuals agent.

### Why This Fixes Everything

| Sentient Problem | NEUROFORGE Solution | Why It Works |
|-----------------|---------------------|-------------|
| VLA autonomy too hard | We don't need autonomy. Humans teleoperate. The OUTPUT is training data for future autonomy. | Teleoperation is proven, shipping tech. No sim-to-real gap. |
| Virtuals integration superficial | Each trained policy IS a Virtuals agent token. Token = ownership of the training data + model weights. | You literally cannot remove Virtuals without destroying the product. |
| Replay attacks on proof | No proof-of-work needed. Value = training demonstrations, verified by quality metrics (task completion, smoothness, efficiency). | Quality scoring is a solved ML problem (DTW, success rate, trajectory smoothness). |
| No moat | The accumulated demonstration dataset IS the moat. Can't fork 10,000 human teleop sessions. | Data moats are the strongest moats in AI. |
| No GTM | Every robotics lab and company needs training data RIGHT NOW. This is the #1 bottleneck in the field. | Data scarcity is universally acknowledged as the robotics bottleneck. |
| Liability gap | Human teleoperators control the robots. No autonomous harm. | Standard remote operation liability framework applies. |

---

## How NEUROFORGE Works

```
DEMAND SIDE                          SUPPLY SIDE
(Robotics companies need data)       (Teleoperators earn crypto)

┌─────────────────────┐              ┌─────────────────────-┐
│ Skill Bounty        │              │ Human Teleoperator   │
│ "Pick up cup"       │              │ Controls G1 remotely │
│ Reward: 500 $FORGE  │──────────────│ Records demo session │
│ Quality: min 0.85   │              │ Gets rated on quality│
│ Demos needed: 100   │              │ Earns $FORGE tokens  │
└─────────────────────┘              └────────────────────-─┘
         │                                    │
         ▼                                    ▼
┌─────────────────────────────────────────────────-┐
│              NEUROFORGE PROTOCOL                 │
│                                                  │
│  1. Bounty posted on-chain (Base)                │
│  2. Teleoperator claims bounty                   │
│  3. Teleoperator performs task via robot         │
│  4. Demo recorded (joint angles, gripper, video) │
│  5. Quality score computed (automated)           │
│  6. If quality ≥ threshold → payment released    │
│  7. When N demos collected → train VLA policy    │
│  8. Trained policy = NEW VIRTUALS AGENT TOKEN    │
│                                                  │
│  Token holders of skill token earn royalties     │
│  every time another robot licenses that skill    │
└───────────────────────────────────────────────-──┘
```

### The Virtuals Integration (Deep, Not Superficial)

This is why Jansen should care: every trained policy is a Virtuals agent.

```
SKILL LIFECYCLE ON VIRTUALS:

1. BOUNTY PHASE
   - Someone posts a skill bounty: "Teach robot to fold towels"
   - Community funds the bounty by buying into the skill's token pre-launch
   - This is a Virtuals-native token launch for a PHYSICAL skill

2. DATA COLLECTION PHASE
   - Teleoperators earn tokens per accepted demonstration
   - Quality reviewers earn tokens for validating demos
   - Data accumulates on-chain (metadata) + off-chain (recordings)

3. TRAINING PHASE
   - When enough demos collected, VLA policy is trained
   - Training costs paid from the skill token's treasury
   - Model weights stored (IPFS/Arweave), hash on-chain

4. MONETIZATION PHASE
   - Trained policy = Virtuals Agent with a token
   - Any robot operator can LICENSE the skill by paying in the skill token
   - Revenue flows to: token holders (60%), original teleoperators (25%), protocol (15%)
   - The more robots use the skill, the more valuable the token

5. IMPROVEMENT PHASE
   - Failed executions create new bounties for better demos
   - Token holders vote on which skills to train next
   - Flywheel: better skills → more licensing → more revenue → more investment
```

Key insight for Jansen: You CANNOT remove Virtuals from this. The tokenization IS the coordination mechanism. Without it, you have no way to:
- Fund bounties collectively
- Share ownership of trained policies
- Distribute licensing revenue
- Govern which skills to prioritize

---

## Judge-by-Judge Scoring Prediction

### Jansen Teng (Virtuals Co-Founder)

| Criterion | Old Score | New Score | Why |
|-----------|-----------|-----------|-----|
| Virtuals Integration Depth | 4/10 | 9/10 | Each skill IS a Virtuals agent. Token = policy ownership. Cannot exist without Virtuals. |
| Tokenomics Soundness | 5/10 | 8/10 | Real value backing: trained VLA policies that robots pay to license. Not speculative. |
| Revenue Model Viability | 3/10 | 7/10 | Licensing revenue is proven (SaaS model). Data marketplace has real demand. |
| Technical Feasibility | 3/10 | 8/10 | Teleoperation + behavior cloning = proven stack. No moonshot VLA needed. |
| Novelty | 6/10 | 8/10 | First tokenized robot skill marketplace. Unique. |
| Overall | 4/10 | 8/10 | |

Jansen's killer questions answered:
- *"How is task completion verified?"* → Automated quality metrics: task success (binary), trajectory smoothness (DTW score), efficiency (time vs. baseline). No human oracle needed.
- *"What breaks if you remove Virtuals?"* → EVERYTHING. No token = no bounty funding, no skill ownership, no licensing revenue, no governance.
- *"Dispute resolution?"* → Quality score is deterministic. If demo scores below threshold, payment withheld automatically. Appeals go to token holder vote (on-chain governance).

---

### Balaji (Network School Founder)

| Criterion | Old Score | New Score | Why |
|-----------|-----------|-----------|-----|
| Network Effects | 6/10 | 9/10 | More teleoperators → more data → better policies → more licensing → more teleoperators. Classic two-sided marketplace. |
| Civilizational Significance | 8/10 | 9/10 | "Solving the data bottleneck is solving robotics. Period." This is the Mechanical Turk for the physical world. |
| First Principles Thinking | 5/10 | 8/10 | Autonomy is a DATA problem, not a MODEL problem. We produce the data. |
| Execution Realism | 4/10 | 8/10 | Teleoperation works TODAY. Behavior cloning works TODAY. We're combining them with crypto incentives. |
| Competitive Moat | 3/10 | 8/10 | 10,000 teleop demonstrations can't be forked. The dataset IS the moat. Figure AI can't replicate our community. |
| Overall | 5.5/10 | 8.4/10 | |

Balaji's killer questions answered:
- *"Legal liability?"* → Human teleoperator controls the robot. Standard remote operation liability. No autonomous agent causing harm.
- *"Replay data as fake proof?"* → Not relevant. We're not proving physical work. We're collecting training demonstrations. Quality is measured by task metrics, not proof schemes.
- *"Name ONE task?"* → We don't need autonomous tasks. Humans do the tasks via teleoperation. That's the whole point. The OUTPUT is training data, not autonomous execution.

---

### Xen (Base Head of Growth)

| Criterion | Old Score | New Score | Why |
|-----------|-----------|-----------|-----|
| Base Ecosystem Value | 6/10 | 8/10 | Every bounty post, every demo submission, every quality review, every licensing payment = Base transaction. |
| Go-to-Market Realism | 3/10 | 7/10 | Target: robotics researchers who need data (known market). Teleoperators: gig economy workers, gaming community. |
| Community/Virality | 8/10 | 9/10 | Leaderboards, skill ratings, "I trained a robot to fold towels and earned $200." Gamified teleoperation is inherently viral. |
| Transaction Volume | 5/10 | 8/10 | Each demo = transaction. Each quality review = transaction. Each license = transaction. High frequency, small value. |
| Competitive Landscape | 4/10 | 7/10 | Scale AI does data labeling for 2D. Nobody does tokenized data labeling for 3D robot demonstrations. |
| Overall | 5/10 | 7.8/10 | |

Xen's killer questions answered:
- *"Name one partner committed in writing?"* → We don't need partner commitments. Teleoperators self-select from the community. Bounties are permissionless. DAY 1: post bounties from the hackathon lab using the 30 G1 robots.
- *"Liability/insurance structure?"* → Human-controlled teleoperation. Same liability as any remote-controlled machine. Standard operator insurance.
- *"What stops Unitree from shipping this?"* → Unitree makes hardware. They don't build data marketplaces. This is like asking "what stops NVIDIA from building Hugging Face?" Different layer.

---

### Rocky Yang (NTU Professor)

| Criterion | Old Score | New Score | Why |
|-----------|-----------|-----------|-----|
| VLA Integration Realism | 2/10 | 8/10 | We're not deploying VLA at inference time. We're CREATING the training data for VLA. Behavior cloning from teleop demos is standard. |
| Sim-to-Real Transfer | 2/10 | 9/10 | NO sim-to-real needed. Demos collected on REAL robots via teleoperation. Data is already real-world. |
| Proof System | 3/10 | 7/10 | No proof-of-work scheme. Quality measured by standard robotics metrics: task completion, trajectory analysis, consistency. |
| Autonomous Execution | 3/10 | N/A | Not claiming autonomy. Humans teleoperate. The PRODUCT of our platform enables future autonomy (via training data). |
| Architecture Quality | 4/10 | 7/10 | Clean separation: teleoperation layer, data storage layer, quality assessment layer, training pipeline, tokenization layer. |
| Overall | 3/10 | 7.8/10 | |

Rocky's killer questions answered:
- *"VLA inference latency vs G1 control frequency?"* → Not relevant. Humans control the robot in real-time via standard teleoperation interfaces (30Hz+). No VLA inference during operation.
- *"Replay attack?"* → Each demo has unique joint trajectories, timestamps, and camera feeds. Statistical analysis detects duplicates trivially (identical trajectories = flagged). But more importantly, the incentive is to CREATE good demos, not fake them.
- *"What specific task, what success rate?"* → Demo day: "pick up object and place in bin." 30 teleop demos collected over 2 hours. Behavior cloning trained overnight. Next day: show the TRAINED policy executing autonomously. This is a 2-day experiment, not a research frontier.

---

## Build Timeline (3.5 Weeks, Realistic)

| Week | Focus | Deliverables | Risk Level |
|------|-------|-------------|------------|
| Week 1 | Teleoperation + Smart Contracts | Working teleop interface for G1 (use provided lab tooling). Bounty contract deployed on Base testnet. Demo submission flow working. | LOW (teleoperation software provided by lab) |
| Week 2 | Data Pipeline + Quality Scoring | Demo recording (joint angles, video). Automated quality scoring (task completion detector, trajectory smoothness). Token payment flow end-to-end. | MEDIUM (quality scoring needs tuning) |
| Week 3 | Training Pipeline + Tokenization | Behavior cloning from collected demos. Virtuals agent token launch for first trained skill. Licensing mechanism. Dashboard showing the full lifecycle. | MEDIUM (training may need iteration) |
| Week 3.5 | Demo Day Prep | Live demo: bounty → teleop → quality score → payment → train → autonomous execution. Polish dashboard. Rehearse pitch. | LOW (polish phase) |

### The Demo Day Moment

```
LIVE ON STAGE:

1. Open dashboard: "Here's a bounty: teach robot to pick up a cup. 500 $FORGE reward."
2. Volunteer from audience teleoperates the G1 (takes 30 seconds)
3. Quality score appears: 0.92/1.00 ✅
4. $FORGE tokens transfer to volunteer's wallet (visible on Base explorer)
5. "We've collected 50 demos this week. Here's what happened when we trained a VLA on them..."
6. Robot executes the task AUTONOMOUSLY using the trained policy
7. "This skill is now a Virtuals agent. Anyone can license it."
8. Show the token, show the licensing flow, show revenue distribution.

Total demo time: 3 minutes. Judges see the FULL lifecycle.
```

---

## Tokenomics ($FORGE)

### Token Utility (Real, Not Speculative)

| Action | Token Flow |
|--------|-----------|
| Post a skill bounty | Pay $FORGE to fund the bounty |
| Submit a demo | Earn $FORGE if quality threshold met |
| Review a demo | Earn $FORGE for quality assessment |
| Train a policy | Costs $FORGE (compute) from skill treasury |
| License a trained skill | Pay in skill's Virtuals agent token |
| Hold skill token | Earn share of all licensing revenue |
| Stake $FORGE | Priority access to high-value bounties |
| Governance | Vote on platform parameters (quality thresholds, revenue splits) |

### Revenue Model

1. Platform fee: 5% of all bounty payments
2. Training fee: 10% of compute costs
3. Licensing cut: 15% of all skill licensing revenue
4. Data marketplace: Sell aggregated (anonymized) datasets to robotics companies

### Why the Token Has Real Value

The token is backed by trained VLA policies that robots pay to use. As more skills get trained and licensed, more revenue flows through the system. This is a productive asset, not speculation.

---

## Competitive Analysis

| Competitor | What They Do | Why NEUROFORGE Wins |
|-----------|-------------|-------------------|
| Scale AI | 2D image/text labeling | We do 3D robot demonstration data. Different modality entirely. |
| Hugging Face | Model hosting | We produce the TRAINING DATA, not host models. Complementary. |
| RoboFlow | Computer vision datasets | 2D vision only. No robot manipulation data. |
| No one | Tokenized robot skill marketplace | First mover. No direct competitor. |

### Moat Layers

1. Data moat: Accumulated teleop demonstrations (can't fork)
2. Community moat: Trained teleoperators with skill ratings (network effect)
3. Quality moat: Calibrated quality scoring models (improve with more data)
4. Token moat: Virtuals agent tokens for each skill (ecosystem lock-in)

---

## Aggregate Predicted Score

| Judge | Old Score | New Score | Delta |
|-------|-----------|-----------|-------|
| Jansen | 4/10 | 8/10 | +4 |
| Balaji | 5.5/10 | 8.4/10 | +2.9 |
| Xen | 5/10 | 7.8/10 | +2.8 |
| Rocky | 3/10 | 7.8/10 | +4.8 |
| Average | 4.4/10 | 8.0/10 | +3.6 |

---

## What Changed (Summary)

| Aspect | Sentient Protocol | NEUROFORGE |
|--------|------------------|------------|
| Core bet | Autonomous robot execution | Human teleoperation → training data |
| Technical risk | Extreme (VLA, sim-to-real) | Low (teleoperation is proven) |
| Virtuals integration | Bolted on | Native (skills = agent tokens) |
| Revenue source | Robot task payments (speculative) | Skill licensing (proven SaaS model) |
| Moat | None (forkable) | Data + community (unforkable) |
| Liability | Autonomous robot harm (unsolved) | Human-controlled (standard) |
| 3.5 week feasibility | Unrealistic | Achievable |
| Demo day wow factor | "Robot does task alone" (risky) | "Audience member trains robot live, robot learns" (reliable + interactive) |

---

## Updated Application Answers

### "Describe what you are building"

We're building NEUROFORGE, the first decentralized marketplace where humans earn crypto by teleoperating humanoid robots to create training demonstrations, and each trained skill becomes a tokenized Virtuals agent that other robots can license.

Problem: The #1 bottleneck in robotics isn't better models or better hardware. It's training data. Every robotics lab needs thousands of human demonstrations to train VLA policies, but there's no scalable way to collect, quality-check, and compensate demonstration data at scale.

Solution: NEUROFORGE creates a crypto-incentivized marketplace: robotics companies post skill bounties ("teach a robot to fold towels, 500 $FORGE reward"), teleoperators around the world claim bounties and perform tasks via remote robot control, quality is scored automatically, and when enough demos are collected, a VLA policy is trained and tokenized as a Virtuals agent. Anyone can license the trained skill, with revenue flowing to the original teleoperators, token holders, and the protocol.

Why Virtuals is essential: Each trained robot skill IS a Virtuals agent token. Token holders own the training data and model weights. When robots license a skill, revenue flows through Virtuals' tokenomics. You cannot remove Virtuals from NEUROFORGE without destroying the product.

### "Why interested in this robotics hackathon?"

Three reasons: (1) 30 Unitree G1 robots = the perfect testbed for our teleoperation data collection pipeline.  We can collect hundreds of demonstrations in 3.5 weeks and prove the full lifecycle live. 
(2) Virtuals Protocol tokenization is not an add-on for us. Each trained skill IS a Virtuals agent. Direct access to Jansen's team means we ship the most deeply integrated Virtuals product at Demo Day. 
(3) The data bottleneck in robotics is THE problem. This hackathon gives us the hardware to prove we can solve it.

### "How far along are you?"

Prototype. We have smart contract architecture for bounties and skill tokens designed, teleoperation interface spec'd for Unitree G1, and quality scoring metrics defined. The IRL build phase is exactly what we need: real robots to collect real demonstrations and prove the full bounty → teleop → train → license lifecycle.

### "Traction"

Pre-product, but the market validation is clear: every robotics lab we've spoken to confirms data collection is their #1 bottleneck. The 3.5 weeks at the lab IS our traction plan. By Demo Day: working bounty marketplace on Base, 100+ teleop demonstrations collected, at least 2 trained VLA policies, and first Virtuals agent token launched representing a real trained skill.
