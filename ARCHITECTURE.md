# NEUROFORGE Architecture

> The Decentralized Training Data Marketplace for Humanoid Robots

---

## System Overview

```
                        ┌─────────────────────────────────────────┐
                        │            NEUROFORGE PROTOCOL          │
                        │     "Scale AI for 3D Robot Data, but    │
                        │      data collectors own the value"     │
                        └─────────────────────────────────────────┘
                                          │
                    ┌─────────────────────┼─────────────────────┐
                    ▼                     ▼                     ▼
             DEMAND SIDE           PROTOCOL LAYER          SUPPLY SIDE
          (Robotics Labs)         (Base + Virtuals)      (Teleoperators)
```

---

## Full Architecture Diagram

```
┌──────────────────────────────────────────────────────────────────────────┐
│                                                                          │
│   DEMAND SIDE                        SUPPLY SIDE                         │
│   ───────────                        ───────────                         │
│                                                                          │
│   ┌──────────────────┐               ┌──────────────────┐                │
│   │  Robotics Lab /  │               │  Teleoperator    │                │
│   │  Company         │               │  (Human)         │                │
│   │                  │               │                  │                │
│   │  "I need 100     │               │  "I'll teach     │                │
│   │   pick-place     │               │   robots for     │                │
│   │   demonstrations"│               │   crypto"        │                │
│   └────────┬─────────┘               └────────┬─────────┘                │
│            │                                  │                          │
│            │ Posts bounty                     │ Claims bounty            │
│            │ (500 $FORGE)                     │                          │
│            ▼                                  ▼                          │
│   ┌─────────────────────────────────────────────────────────────┐        │
│   │                   BOUNTY MARKETPLACE                        │        │
│   │                   (Smart Contract on Base)                  │        │
│   │                                                             │        │
│   │   ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐        │        │
│   │   │Pick-up  │  │Drawer   │  │Object   │  │Fold     │        │        │
│   │   │Cup      │  │Open     │  │Handoff  │  │Towel    │        │        │
│   │   │500 FORGE│  │300 FORGE│  │400 FORGE│  │800 FORGE│        │        │
│   │   │92/100   │  │45/100   │  │78/100   │  │12/100   │        │        │
│   │   │demos    │  │demos    │  │demos    │  │demos    │        │        │
│   │   └─────────┘  └─────────┘  └─────────┘  └─────────┘        │        │
│   └───────────────────────┬─────────────────────────────────────┘        │
│                           │                                              │
│                           ▼                                              │
│   ┌──────────────────────────────────────────────────────-───────┐       │
│   │                TELEOPERATION LAYER                           │       │
│   │                                                              │       │
│   │   Operator's Browser                    Unitree G1 Robot     │       │
│   │   ┌──────────────┐       WebRTC         ┌──────────────┐     │       │
│   │   │ VR/Gamepad   │◄────────────────────►│ Joint Motors │     │       │
│   │   │ Controller   │     <200ms latency   │ Gripper      │     │       │
│   │   │ Video Feed   │                      │ IMU Sensors  │     │       │
│   │   │ Force FB     │                      │ RGB-D Camera │     │       │
│   │   └──────────────┘                      └──────────────┘     │       │
│   │                                                              │       │
│   │   RECORDED DATA PER DEMONSTRATION:                           │       │
│   │   ├── Joint angles (23 DoF @ 30Hz)                           │       │
│   │   ├── Gripper force/position                                 │       │
│   │   ├── RGB video (2 cameras)                                  │       │
│   │   ├── Depth map                                              │       │
│   │   ├── IMU (accelerometer + gyroscope)                        │       │
│   │   └── Timestamps + task metadata                             │       │
│   └───────────────────────┬──────────────────────────-───────────┘       │
│                           │                                              │
│                           ▼                                              │
│   ┌─────────────────────────────────────────────────────────────-┐       │
│   │                QUALITY SCORING ENGINE                        │       │
│   │                (Automated, Deterministic)                    │       │
│   │                                                              │       │
│   │   ┌───────────────┐ ┌───────────────┐ ┌────────────────┐     │       │
│   │   │ Task Success  │ │ Trajectory    │ │ Efficiency     │     │       │
│   │   │ Detector      │ │ Smoothness    │ │ Score          │     │       │
│   │   │ (binary)      │ │ (DTW score)   │ │ (time/baseline)│     │       │
│   │   └───────┬───────┘ └───────┬───────┘ └───────┬────────┘     │       │
│   │           │                 │                 │              │       │
│   │           └─────────────────┼─────────────────┘              │       │
│   │                             ▼                                │       │
│   │                    Composite Score: 0.00 - 1.00              │       │
│   │                    Threshold: 0.85 (configurable per bounty) │       │
│   │                                                              │       │
│   │          ┌─────────┐                    ┌─────────┐          │       │
│   │          │ PASS ✅ │                     │ FAIL ❌ │          │       │
│   │          │ ≥ 0.85  │                    │ < 0.85  │          │       │
│   │          └────┬────┘                    └────┬────┘          │       │
│   │               │                              │               │       │
│   │          $FORGE paid                    No payment,          │       │
│   │          to operator                    retry allowed        │       │
│   └───────────┬─────────────────────────────────────────────────-┘       │
│               │                                                          │
│               │ When bounty filled (N demos collected)                   │
│               ▼                                                          │
│   ┌─────────────────────────────────────────────────────────────-┐       │
│   │                VLA TRAINING PIPELINE                         │       │
│   │                                                              │       │
│   │   Raw Demos ──► Data Cleaning ──► Behavior Cloning ──► VLA   │       │
│   │   (N demos)     (outlier         (policy learning)    Policy │       │
│   │                  removal)                                    │       │
│   │                                                              │       │
│   │   Tools: LeRobot / OpenVLA / ACT (Action Chunking)           │       │
│   │   Compute: Funded from skill token treasury                  │       │
│   │   Output: Trained policy weights (stored on IPFS/Arweave)    │       │
│   │           Policy hash recorded on-chain                      │       │
│   └───────────────────────┬─────────────────────────────────-────┘       │
│                           │                                              │
│                           ▼                                              │
│   ┌─────────────────────────────────────────────────────────────┐       │
│   │            VIRTUALS AGENT TOKENIZATION                      │       │
│   │            (Each trained skill = Virtuals Agent Token)      │       │
│   │                                                             │       │
│   │   ┌─────────────────────────────────────────────────┐       │       │
│   │   │  $SKILL_PICKPLACE    ← "Pick and Place" policy  │       │       │
│   │   │  $SKILL_DRAWER       ← "Drawer Open" policy     │       │       │
│   │   │  $SKILL_HANDOFF      ← "Object Handoff" policy  │       │       │
│   │   │  $SKILL_FOLD         ← "Towel Fold" policy      │       │       │
│   │   │  ...                                            │       │       │
│   │   └─────────────────────────────────────────────────┘       │       │
│   │                                                             │       │
│   │   Token = Ownership of:                                     │       │
│   │   ├── Training data (the demonstrations)                    │       │
│   │   ├── Model weights (the trained VLA policy)                │       │
│   │   └── Licensing revenue stream                              │       │
│   └───────────────────────┬─────────────────────────────────────┘       │
│                           │                                              │
│                           ▼                                              │
│   ┌─────────────────────────────────────────────────────────────-┐       │
│   │                LICENSING MARKETPLACE                         │       │
│   │                                                              │       │
│   │   Any robot operator can license a trained skill:            │       │
│   │                                                              │       │
│   │   Robot Operator ──► Pays in $SKILL token ──► Downloads      │       │
│   │                                                 VLA Policy   │       │
│   │                                                              │       │
│   │   Revenue Distribution:                                      │       │
│   │   ├── 60%  Token Holders (investors, early supporters)       │       │
│   │   ├── 25%  Teleoperators (who created the training data)     │       │
│   │   └── 15%  Protocol Treasury (NEUROFORGE)                    │       │
│   └──────────────────────────────────────────────────────────-───┘       │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

---

## Smart Contract Architecture (Base L2)

```
                    ┌──────────────────────────┐
                    │     $FORGE Token          │
                    │     (ERC-20 on Base)      │
                    │                           │
                    │  Utility:                 │
                    │  - Pay bounties           │
                    │  - Stake for priority     │
                    │  - Governance votes       │
                    │  - Platform fees          │
                    └────────────┬──────────────┘
                                │
              ┌─────────────────┼─────────────────┐
              ▼                 ▼                  ▼
┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐
│ BountyFactory.sol│ │ QualityOracle.sol│ │ SkillRegistry.sol│
│                  │ │                  │ │                  │
│ createBounty()   │ │ submitScore()    │ │ registerSkill()  │
│ claimBounty()    │ │ getScore()       │ │ mintSkillToken() │
│ submitDemo()     │ │ appealScore()    │ │ licenseSkill()   │
│ releasePayout()  │ │ updateThreshold()│ │ distributeRev()  │
│ cancelBounty()   │ │                  │ │ getPolicy()      │
└──────────────────┘ └──────────────────┘ └──────────────────┘
        │                     │                     │
        └─────────────────────┼─────────────────────┘
                              ▼
                 ┌──────────────────────┐
                 │ VirtualsBridge.sol   │
                 │                      │
                 │ launchAgentToken()   │
                 │ syncRevenue()        │
                 │ mapSkillToAgent()    │
                 └──────────────────────┘
                              │
                              ▼
                 ┌──────────────────────┐
                 │ Virtuals Protocol    │
                 │ (Agent Token Launch) │
                 └──────────────────────┘
```

---

## Data Flow (Single Demonstration Lifecycle)

```
Step 1: POST BOUNTY
   Lab ──► BountyFactory.sol ──► Bounty created on-chain
           (500 $FORGE locked in escrow)

Step 2: CLAIM + TELEOPERATE
   Operator ──► Claims bounty ──► WebRTC connects to G1
   Operator ──► Performs task ──► Data recorded locally

Step 3: SUBMIT + SCORE
   Demo upload ──► IPFS (raw data) ──► QualityOracle scores
   Score = 0.92 ✅ ──► QualityOracle.sol.submitScore()

Step 4: PAYOUT
   BountyFactory.sol ──► Transfers $FORGE to operator wallet
   On-chain event: DemoAccepted(bountyId, operatorAddr, score)

Step 5: TRAIN (when bounty fills 100/100 demos)
   100 demos ──► VLA Training Pipeline ──► Trained policy
   Policy weights ──► IPFS (hash recorded on-chain)

Step 6: TOKENIZE
   SkillRegistry.sol ──► VirtualsBridge.sol ──► Agent token minted
   $SKILL_PICKPLACE now tradeable on Virtuals

Step 7: LICENSE
   Robot Operator ──► Pays $SKILL_PICKPLACE ──► Downloads policy
   Revenue auto-distributed: 60% holders / 25% operators / 15% protocol
```

---

## Technology Stack

```
LAYER               TECHNOLOGY                PURPOSE
─────────────────────────────────────────────────────────────────
Blockchain          Base (Ethereum L2)         Low-cost, fast txns
Smart Contracts     Solidity + Foundry         Bounties, tokens, licensing
Token Standard      ERC-20 ($FORGE)            Platform utility token
Agent Tokens        Virtuals Protocol          Per-skill agent tokens
Teleoperation       WebRTC + ROS2              Low-latency robot control
Robot Hardware      Unitree G1 (23 DoF)        Humanoid manipulation
Data Recording      ROS2 bag + HDF5            Joint angles, video, IMU
Quality Scoring     Python (DTW, sklearn)      Automated demo assessment
VLA Training        LeRobot / OpenVLA / ACT    Behavior cloning
Model Storage       IPFS / Arweave             Decentralized persistence
Frontend            Next.js + shadcn/ui        Dashboard + bounty UI
Backend             Node.js API Routes         Orchestration layer
Indexer             The Graph (subgraph)        On-chain event indexing
```

---

## Flywheel Effect

```
                    More Teleoperators
                         │
                         ▼
                  More Demonstrations
                         │
                         ▼
              Better Trained Policies
                         │
                         ▼
               More Robots License
                         │
                         ▼
              More Revenue to Holders
                         │
                         ▼
           Higher Skill Token Value ───────┐
                         │                 │
                         ▼                 │
              More Bounties Posted         │
                         │                 │
                         └─────────────────┘
                      (FLYWHEEL)
```

---

## Security Model

```
THREAT                          MITIGATION
─────────────────────────────────────────────────────────────────
Fake demonstrations             Automated quality scoring (DTW,
                                task completion, trajectory analysis)
                                Statistical duplicate detection

Sybil operators                 Stake $FORGE to claim bounties
                                Reputation score per operator
                                Rate limiting per wallet

Low-quality flooding            Quality threshold per bounty (0.85+)
                                No payment below threshold
                                Escalating stake for repeat failures

Model weight theft              Policy hash on-chain (integrity)
                                Encrypted storage on IPFS
                                License key required for download

Smart contract exploits         Audited Solidity (OpenZeppelin base)
                                Timelock on admin functions
                                Escrow pattern for bounty payouts
```

---

## Demo Day Architecture (Live Flow)

```
STAGE DEMO (3 minutes):

1. Dashboard     ──► Show bounty: "Pick up cup" (500 $FORGE)
        │
2. Volunteer     ──► Claims bounty from phone/laptop
        │
3. Teleoperate   ──► Volunteer controls G1 via gamepad (30 seconds)
        │
4. Quality Score ──► 0.92/1.00 appears on dashboard ✅
        │
5. Payment       ──► $FORGE transfers to volunteer wallet (Base explorer)
        │
6. Pre-recorded  ──► "We collected 500 demos this week. Here's the result..."
        │
7. Autonomous    ──► G1 executes pick-up AUTONOMOUSLY (trained policy)
        │
8. Token Launch  ──► Show $SKILL_PICKPLACE on Virtuals
        │
9. License Flow  ──► Another robot pays to download the skill
```

---

## Key Metrics (Current)

| Metric                     | Value              |
|----------------------------|--------------------|
| Sim demonstrations         | 47                 |
| Manipulation tasks tested  | 3                  |
| Base Sepolia test txns     | 23                 |
| Teleop latency             | <200ms             |
| Quality scoring accuracy   | ~94% (sim-tested)  |
| Operator waitlist          | 12                 |
| Labs interested            | 3                  |

## Demo Day Targets

| Metric                     | Target             |
|----------------------------|--------------------|
| Real-world demonstrations  | 500+               |
| Trained VLA policies       | 5                  |
| Virtuals agent tokens      | 5                  |
| Live teleoperation demo    | Yes (audience)     |
| Autonomous execution demo  | Yes (trained G1)   |
| First paid skill license   | Yes (on-chain)     |
