# NEUROFORGE Architecture Explanation

## What is NEUROFORGE?

NEUROFORGE is a **decentralized training data marketplace** designed to solve a critical problem in robotics AI: collecting high-quality demonstrations at scale while ensuring data collectors own the value they create. Instead of centralized companies collecting data and keeping all profits, NEUROFORGE creates an open marketplace where:

- **Robotics Labs** (demand side) post bounties for robot demonstrations they need
- **Teleoperators** (supply side) collect those demonstrations and get paid in tokens
- **Smart contracts** automate quality scoring, payments, and revenue distribution
- **Trained models** become tokenized assets that generate ongoing revenue for everyone involved

---

## Core Architecture Breakdown

### 1. Two-Sided Marketplace Design

The system has two core stakeholders:

**DEMAND SIDE (Robotics Labs/Companies)**
- Need training data for specific robot manipulation tasks
- Post bounties with detailed specifications (e.g., "Pick up a cup - 500 $FORGE for 100 demonstrations")
- Lock funds in smart contracts as escrow
- Receive trained policies once enough quality data is collected

**SUPPLY SIDE (Teleoperators)**
- Humans who want to earn crypto by teaching robots
- Claim bounties and teleoperate robots via VR/gamepad
- Submit recordings of their demonstrations
- Get paid immediately when quality thresholds are met

**PROTOCOL LAYER (Base L2 Blockchain)**
- Smart contracts manage bounties, payments, and tokenization
- Automated quality scoring eliminates human disputes
- Revenue distribution logic ensures fair splits
- Record of all transactions and policy hashes for transparency

---

### 2. Bounty Marketplace

The bounty marketplace is where labs post jobs and operators claim work:

```
Lab posts: "I need 100 pick-place demonstrations, willing to pay 500 $FORGE"
  ↓
Marketplace displays the task with:
- Task description & success criteria
- Bounty amount in $FORGE
- Progress: (current demonstrations / target)
- Quality threshold (e.g., must score 0.85+)
  ↓
Teleoperator claims bounty
  ↓
Operator teleopeates (controls robot live)
  ↓
Demonstration is submitted & scored automatically
  ↓
If score ≥ 0.85: $FORGE paid to operator, progress increments
If score < 0.85: No payment, operator can retry
```

Key insight: The bounty is for **N demonstrations of the same task**, not individual payments. This aggregates data for training.

---

### 3. Teleoperation Layer (Live Robot Control)

When an operator claims a bounty, they control a real Unitree G1 robot via:

**Input (Operator's Browser)**
- VR headset OR gamepad controller
- Camera feeds from robot (2 RGB cameras + depth)
- Force feedback on gripper (what the robot feels)

**Real-time Communication**
- WebRTC protocol (<200ms latency for responsive control)
- Enables immersive teleoperation experience

**What Gets Recorded**
- Joint angles of all 23 degrees of freedom at 30Hz (robot's motion)
- Gripper force and position (hand strength/location)
- RGB video feeds (2 camera angles)
- Depth maps (3D understanding of scene)
- IMU data (accelerometer + gyroscope)
- Timestamps and task metadata

This rich multimodal data (joint angles + video + depth + force) is what trains the VLA (Vision Language Action) model.

---

### 4. Automated Quality Scoring Engine

Instead of humans arguing about whether a demonstration is "good enough", the system uses **deterministic, automated scoring**:

**Three Scoring Components:**

1. **Task Success Detector** (Binary)
   - Did the robot actually pick up the cup? (Yes/No)
   - Uses computer vision + object detection
   - Prevents fraudulent submissions where nothing happened

2. **Trajectory Smoothness** (DTW Score 0-1)
   - Uses Dynamic Time Warping to compare against reference trajectories
   - Penalizes jerky, inefficient motions
   - Higher score = more professional execution

3. **Efficiency Score** (0-1)
   - Measures time taken vs. baseline
   - Faster execution = higher score
   - Prevents slow, wandering demonstrations

**Final Score Calculation:**
```
Composite Score = f(task_success, smoothness, efficiency)
Threshold = 0.85 (configurable per bounty)

Score ≥ 0.85 → PASS (operator gets paid)
Score < 0.85 → FAIL (no payment, retry allowed)
```

**Why this matters:**
- No human judges = no bias or disputes
- Operators know exactly what will pass
- Quality is guaranteed before training
- Prevents Sybil attacks (low-quality spam)

---

### 5. VLA Training Pipeline

Once a bounty collects N demonstrations (e.g., 100 high-quality pick-place examples):

```
Raw 100 Demonstrations
  ↓ (Clean outliers, remove duplicates)
100 Clean Demonstrations
  ↓ (Behavior cloning: teach model to imitate teleoperators)
Behavior Cloning Policy
  ↓ (Fine-tune into Vision Language Action model)
Trained VLA Policy
  ↓ (Policy weights stored on IPFS, hash on-chain)
Ready for Deployment
```

**Key details:**
- Uses open-source frameworks: LeRobot, OpenVLA, or Action Chunking Transformer
- Training is funded from the skill token treasury (no additional cost to operators)
- Compute is provided by the protocol
- Output is a policy file (neural network weights) that can run on any Unitree G1

---

### 6. Virtuals Agent Tokenization

Each trained policy becomes a **Virtuals Agent Token** on the blockchain:

```
Pick-Place Policy → $SKILL_PICKPLACE token
Drawer-Open Policy → $SKILL_DRAWER token
Object-Handoff Policy → $SKILL_HANDOFF token
Towel-Fold Policy → $SKILL_FOLD token
```

**What ownership of a $SKILL token means:**
- Ownership rights to the training data (the 100 demonstrations)
- Ownership rights to the trained model weights
- Claim on future licensing revenue
- Ability to trade the token on decentralized exchanges

---

### 7. Licensing Marketplace (Ongoing Revenue)

After tokenization, robots can license the trained skill:

```
Robot Operator (Demand)
  ↓ Pays $SKILL_PICKPLACE tokens
  ↓
Downloads trained policy weights
  ↓
Runs it on their own Unitree G1 (autonomously!)
```

**Revenue Distribution (per license transaction):**
- 60% → Token Holders (investors, early supporters, governance participants)
- 25% → Teleoperators (who created the training data)
- 15% → NEUROFORGE Protocol Treasury (for ongoing development)

**Flywheel Effect:**
```
More Operators Collect Data
  ↓
Better Trained Policies
  ↓
More Robots License (because policies work better)
  ↓
More Revenue Generated
  ↓
Tokens become more valuable
  ↓
More Bounties Posted (because $FORGE is valuable)
  ↓
(cycle repeats)
```

---

### 8. Smart Contract Architecture (Base L2)

Four main smart contract modules orchestrate everything:

**1. BountyFactory.sol**
- `createBounty()` - Labs post new tasks
- `claimBounty()` - Operators claim work
- `submitDemo()` - Submit recorded demonstrations
- `releasePayout()` - Pay operators when quality passes
- `cancelBounty()` - Cancel if not enough interest

**2. QualityOracle.sol**
- `submitScore()` - AI scores the demonstration
- `getScore()` - Query any demonstration's score
- `appealScore()` - Operators can challenge unfair scores (via staking)
- `updateThreshold()` - Adjust quality threshold per bounty

**3. SkillRegistry.sol**
- `registerSkill()` - Register a newly trained policy
- `mintSkillToken()` - Create the $SKILL token for the policy
- `licenseSkill()` - Robot operators license the policy
- `distributeRev()` - Auto-split revenue 60/25/15
- `getPolicy()` - Download the policy file from IPFS

**4. VirtualsBridge.sol**
- `launchAgentToken()` - Bridge to Virtuals Protocol
- `syncRevenue()` - Keep revenue tracking in sync
- `mapSkillToAgent()` - Link trained policy to Virtuals agent

All use **escrow patterns** to prevent theft, and admin functions have **timelocks** to prevent rug pulls.

---

## Data Flow (Complete Lifecycle)

### Step-by-Step Example: Pick-Place Bounty

**Step 1: Lab Posts Bounty**
```
Lab → BountyFactory.sol
Input: {
  task: "Pick up the blue cup",
  count: 100,
  reward: 500 $FORGE,
  threshold: 0.85
}
Output: Bounty created, 500 $FORGE locked in escrow
```

**Step 2: Operator Claims & Teleopeates**
```
Operator → Clicks "Claim Bounty" on dashboard
Robot connects via WebRTC
Operator controls with VR/gamepad for ~30 seconds
Demonstration recorded locally (joint angles, video, depth, IMU, force)
```

**Step 3: Submit & Automatic Scoring**
```
Operator → Uploads data to IPFS
QualityOracle processes in background:
  - Detects: Did robot hold the cup? (yes/no)
  - Scores: Smoothness via DTW (0.95/1.0)
  - Scores: Efficiency vs baseline (0.88/1.0)
  - Computes: Composite = 0.92 ✅
Database records score on-chain
```

**Step 4: Instant Payout**
```
Score 0.92 ≥ 0.85 → PASS
BountyFactory.sol transfers 5 $FORGE to operator (500 / 100 demos)
Operator's wallet shows +5 $FORGE balance
Bounty progress updates: 47/100 → 48/100
```

**Step 5: Collect Until Full (100/100)**
```
Steps 2-4 repeat 52 more times until bounty collects 100 quality demos
After 100th demo passes scoring, bounty moves to "Training" phase
```

**Step 6: Train VLA Policy (1-2 weeks)**
```
All 100 demos → VLA Training Pipeline
Output: Neural network policy weights (~200MB file)
Policy stored on IPFS
Hash: QmXxxx... recorded on-chain in SkillRegistry.sol
```

**Step 7: Tokenize on Virtuals**
```
SkillRegistry.sol → VirtualsBridge.sol
VirtualsBridge → Virtuals Protocol API
Result: $SKILL_PICKPLACE token minted
Token appears on Virtuals DEX, tradeable instantly
```

**Step 8: First License**
```
Robot Company → Pays 100 $SKILL_PICKPLACE tokens
SkillRegistry → Validates payment received
Download link generated: https://ipfs.io/ipfs/QmXxxx...
Robot company downloads policy
Policy deployed to their Unitree G1
G1 executes pick-place autonomously (no teleop needed!)
```

**Step 9: Revenue Distribution**
```
100 $SKILL_PICKPLACE license fee enters revenue pool
Automatically split:
  ├─ 60 tokens → All token holders (proportional)
  ├─ 25 tokens → All 100 teleoperators (proportional to demos submitted)
  └─ 15 tokens → Protocol Treasury
Next license repeats this
```

---

## Technology Stack (Why Each Choice?)

| Layer | Technology | Why |
|-------|-----------|-----|
| **Blockchain** | Base (Ethereum L2) | Fast txns (~2 sec), low fees (~$0.01), Ethereum security |
| **Smart Contracts** | Solidity + Foundry | Industry standard, auditable, OpenZeppelin libraries |
| **Tokens** | ERC-20 ($FORGE) + Virtuals | Familiar to investors, liquid ecosystem |
| **Teleoperation** | WebRTC + ROS2 | <200ms latency (sub-human reaction time), open-source |
| **Robot** | Unitree G1 | State-of-the-art humanoid, 23 degrees of freedom, affordable |
| **Data Recording** | ROS2 bags + HDF5 | Industry standard robotics format, rich multimodal support |
| **Quality Scoring** | Python (DTW, sklearn) | Fast inference, well-tested algorithms, reproducible |
| **VLA Training** | LeRobot / OpenVLA / ACT | Open-source, proven on real robots, academic backing |
| **Model Storage** | IPFS / Arweave | Decentralized (no single point of failure), permanent archival |
| **Dashboard** | Next.js + shadcn/ui | Modern UX, responsive design, type-safe |
| **Backend** | Node.js API Routes | Lightweight orchestration, event streaming |
| **Indexer** | The Graph (subgraph) | Real-time query of on-chain events, no own backend needed |

---

## Security Model (What Could Go Wrong & Mitigations)

| Threat | Attack Example | Mitigation |
|--------|----------------|-----------|
| **Fake Demonstrations** | Operator submits random video, claims task succeeded | Automated quality scoring (DTW analysis) + task completion detection |
| **Sybil Operators** | One person creates 100 wallets to claim all bounties | Require $FORGE stake to claim, reputation score per wallet, rate limiting |
| **Spam/Low-Quality Flood** | Operator submits 100 barely-passing demos to ruin dataset | Quality threshold (0.85+), no payment below threshold, escalating stake for repeat failures |
| **Model Theft** | Hacker steals policy weights from IPFS | Policy hash stored on-chain (detect tampering), encrypted storage, license key required for access |
| **Smart Contract Exploits** | Hacker drains escrow funds | Audited Solidity (OpenZeppelin base), timelock on admin functions, escrow pattern (funds only release on success) |
| **Oracle Manipulation** | Attacker tricks quality scorer | Deterministic algorithms (DTW, object detection), appeal mechanism with staking, transparent code review |

---

## Demo Day Flow (3-Minute Live Demo)

The architecture supports an impressive live demo:

```
1. [Audience Watches Dashboard]
   Lab posts bounty: "Pick up the green cube" (500 $FORGE, need 100 demos)

2. [Volunteer Joins]
   Audience member claims bounty from their phone/laptop

3. [Live Teleoperation]
   Volunteer controls Unitree G1 via gamepad while standing on stage
   Camera feed projects on screen (green cube moves as volunteer moves the controller)
   Takes ~30 seconds

4. [Instant Quality Score]
   Score appears: 0.91/1.00 ✅
   Audience sees real-time validation

5. [Blockchain Payment]
   Dashboard updates: +5 $FORGE to volunteer's wallet
   Audience opens Etherscan (Base explorer), sees transaction confirmed
   Shows it's real money, real blockchain

6. [Pre-Recorded Montage]
   Slide: "This week, 500 teleoperators collected 2,500 demonstrations"
   Video: Timeline of 500 humans controlling G1 from home

7. [Autonomous Execution]
   "Now the robot runs the learned policy..."
   G1 walks to table, sees green cube, picks it up, walks back
   (No teleop, fully autonomous from trained policy)

8. [Tokenization Reveal]
   Dashboard shows: $SKILL_CUBEHANDOFF token launches on Virtuals
   Price ticker: Just sold for 5,000 $FORGE
   Shows who earned: teleoperators, holders, treasury

9. [Licensing Demo]
   Another robot company on video call
   Pays 100 $SKILL_CUBEHANDOFF tokens
   Downloads policy, runs on their own Unitree G1
   That robot also picks up a cube autonomously (different location, different human)
   Proves the policy generalizes
```

---

## Key Metrics & Targets

### Current Status (Pre-Demo Day)
- 47 simulated demonstrations collected
- 3 manipulation tasks tested
- 23 test transactions on Base Sepolia (testnet)
- Teleoperation latency: <200ms (validated)
- Quality scoring accuracy: ~94% (on simulation)
- Operator interest: 12 on waitlist
- Lab interest: 3 potential customers

### Demo Day Targets
- 500+ real-world demonstrations collected
- 5 fully trained VLA policies deployed
- 5 Virtuals agent tokens minted and trading
- Live teleoperation demo with audience
- Autonomous execution demo (trained G1)
- First paid skill license transaction on-chain

---

## Why This Architecture Matters

### For Teleoperators
- Earn crypto for work that has real value
- Get paid instantly (no middleman)
- Share ongoing revenue when policies are licensed
- Work from home, flexible hours

### For Robotics Labs
- Access diverse, high-quality demonstrations
- Don't hire expensive teleoperation teams
- Trained policies ready to deploy
- Clear pricing and timeline

### For Crypto/Blockchain Community
- New use case beyond DeFi
- Token value backed by physical robotic labor
- Decentralization at scale (distributed workforce)
- Escape platform risk (data stays open on IPFS)

### For Society
- Decentralizes AI development (not just big labs)
- Creates economic opportunity globally
- Transparent AI training (auditable data collection)
- Humanoid robots become more capable faster
