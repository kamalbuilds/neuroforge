# NeuroForge

**The decentralized marketplace for humanoid robot training data.**

Earn crypto by teleoperating robots to create AI training demonstrations. Each trained skill becomes a tokenized Virtuals agent that other robots can license.

## Demo Video Introducing the team

https://youtu.be/oaDcm877-8U

## Problem

The #1 bottleneck in robotics isn't better models or better hardware—**it's training data**. Every robotics lab needs thousands of human demonstrations to train vision-language-action (VLA) policies, but there's no scalable way to collect, quality-check, and compensate demonstration data at scale.

## Solution

NeuroForge creates a **crypto-incentivized marketplace** for robot training data:

```
TELEOPERATOR FLOW          ROBOTICIST FLOW
(Supply Side)              (Demand Side)

Post bounty ────────────► "Pick up cup"
                          500 $FORGE reward
                                 │
                                 ▼
Teleoperate robot ◄────── Need training data
Record demonstration
Get quality score: 0.92
Earn tokens ────────────► Payment released
                                 │
                                 ▼
                          Collect 100 demos
                          Train VLA policy
                          License other robots
                          Earn ongoing revenue
```

### The Flow

1. **Bounty Posted**: Robotics company posts a skill bounty ("teach robot to fold towels, 500 $FORGE reward")
2. **Teleoperator Claims**: Human teleoperation expert claims the bounty
3. **Remote Control**: Teleoperator controls the robot in real-time via web interface
4. **Demo Recorded**: Joint angles, gripper state, camera feed captured
5. **Quality Scored**: Automated quality metrics assess task completion, trajectory smoothness, efficiency
6. **Payment Released**: If quality ≥ threshold, teleoperator gets paid in $FORGE tokens
7. **Policy Trained**: When N demos collected → VLA policy trained via behavior cloning
8. **Token Launched**: Trained skill becomes a Virtuals agent token
9. **Licensing**: Other robots pay to license the trained skill; revenue flows to token holders

## Why This Works

| Challenge | How We Solve It |
|-----------|-----------------|
| **Autonomy** | Humans teleoperate. Output is training data for future autonomy. |
| **Virtuals Integration** | Each trained skill IS a Virtuals agent token. Can't exist without it. |
| **Proof System** | No proof-of-work needed. Quality measured by standard robotics metrics (task completion, trajectory analysis). |
| **Moat** | Accumulated training dataset + community. Can't fork 10,000 human teleop sessions. |
| **Market** | Real: robotics labs need training data RIGHT NOW. Data scarcity is the #1 bottleneck. |
| **Liability** | Human-controlled teleoperation (standard remote operation liability). |

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                  NEUROFORGE PLATFORM                    │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  TELEOP LAYER              DATA PIPELINE      TRAINING  │
│  ├─ Web UI                 ├─ Recording       ├─ Behavior
│  ├─ Real-time control      ├─ Storage         │  Cloning
│  ├─ Video streaming        ├─ Indexing        └─ VLA Policy
│  └─ Robot connection       └─ Versioning          Training
│                                                        │
│  ┌─────────────────────────────────────────────────┐   │
│  │          QUALITY SCORING LAYER                  │   │
│  │  • Task success detector (binary)                   │
│  │  • Trajectory smoothness (DTW score)                │
│  │  • Efficiency (time vs baseline)                    │
│  │  • Consistency checks                               │
│  └─────────────────────────────────────────────────┘   │
│                                                        │
│  ┌─────────────────────────────────────────────────┐   │
│  │      BLOCKCHAIN LAYER (Base Sepolia)            │   │
│  │  • Bounty contracts                                 │
│  │  • Skill token (Virtuals)                           │
│  │  • Payment settlement                               │
│  │  • Governance / DAO                                 │
│  └─────────────────────────────────────────────────┘   │
│                                                        │
└────────────────────────────────────────────────────────┘
```

## Tokenomics ($FORGE)

### Token Utility

| Action | Token Flow |
|--------|-----------|
| Post skill bounty | Pay $FORGE to fund the bounty |
| Submit demo | Earn $FORGE if quality ≥ threshold |
| Review demo | Earn $FORGE for quality assessment |
| Train policy | Costs $FORGE (compute) from skill treasury |
| License skill | Pay in skill's Virtuals agent token |
| Hold skill token | Earn % of all licensing revenue |
| Stake $FORGE | Priority access to high-value bounties |
| Governance | Vote on quality thresholds, revenue splits |

### Revenue Model

1. **Platform fee**: 5% of all bounty payments
2. **Training fee**: 10% of compute costs
3. **Licensing revenue cut**: 15% of all skill licensing fees
4. **Data marketplace**: Sell anonymized datasets to robotics companies

### Why the Token Has Real Value

The token is backed by **trained VLA policies that robots actually pay to license**. As more skills get trained and licensed, more revenue flows through the system. This is a productive asset, not speculation.

## Skill Lifecycle on Virtuals

```
1. BOUNTY PHASE
   - Post: "Teach robot to fold towels"
   - Community funds bounty by buying skill pre-launch token
   - This is a Virtuals-native token launch for a PHYSICAL skill

2. DATA COLLECTION PHASE
   - Teleoperators earn tokens per accepted demo
   - Quality reviewers earn tokens for validation
   - Data accumulates on-chain (metadata) + off-chain (recordings)

3. TRAINING PHASE
   - When N demos collected → VLA policy trained
   - Training costs paid from skill token treasury
   - Model weights stored (IPFS/Arweave), hash on-chain

4. MONETIZATION PHASE
   - Trained policy = Virtuals Agent with a token
   - Any robot can LICENSE the skill by paying in token
   - Revenue: 60% → token holders, 25% → original teleoperators, 15% → protocol
   - More robots using skill → higher token value

5. IMPROVEMENT PHASE
   - Failed executions → new bounties for better demos
   - Token holders vote on priority skills to train
   - Flywheel: better skills → more licensing → more revenue → more investment
```

## Tech Stack

**Frontend**: Next.js 16 + shadcn/ui (dark mode, Geist fonts)
**Teleop Interface**: WebRTC real-time streaming, REST API for robot control
**Quality Scoring**: Python ML pipeline (DTW, Computer Vision)
**Blockchain**: Base Sepolia (fast, Ethereum-compatible)
**Smart Contracts**: Solidity (bounty contracts, skill tokens, Virtuals integration)
**Storage**: IPFS (Storacha) for model weights and demo videos
**Encryption**: Lit Protocol for access control on sensitive demos

## Getting Started

### For Teleoperators
1. Go to dashboard
2. Browse available bounties
3. Claim a bounty
4. Control the robot via teleoperation interface
5. Submit your demonstration
6. Get quality score → receive $FORGE tokens

### For Roboticists
1. Post a skill bounty (specify task, reward, quality threshold)
2. Fund the bounty with $FORGE tokens
3. Monitor incoming demonstrations
4. When threshold reached → policy trained automatically
5. Launch skill as Virtuals agent token
6. Collect licensing revenue as robots use your skill

## Competitive Landscape

| Competitor | What They Do | Why NeuroForge Wins |
|-----------|-------------|-------------------|
| **Scale AI** | 2D image/text labeling | We do 3D robot demonstration data. Different modality entirely. |
| **Hugging Face** | Model hosting | We **produce the training data**, not host models. Complementary. |
| **RoboFlow** | Computer vision datasets | 2D vision only. No robot manipulation data. |
| **No one** | Tokenized robot skill marketplace | **First mover**. No direct competitor. |

## Moat Layers

1. **Data moat**: Accumulated teleop demonstrations (irreplaceable)
2. **Community moat**: Trained teleoperators with skill ratings (network effect)
3. **Quality moat**: Calibrated quality scoring models (improve with more data)
4. **Token moat**: Virtuals agent tokens for each skill (ecosystem lock-in)

## License

MIT
