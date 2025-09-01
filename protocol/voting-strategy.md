# Voting Strategy: Maximizing Protocol Returns

## Overview

The iAERO voting strategy optimizes veAERO voting power allocation across Aerodrome gauges to maximize returns for iAERO stakers. Unlike manual voting, our system uses data-driven decisions based on bribes, fees, and strategic value.

## Core Principles

### 1. ROI Maximization
Every vote aims to generate maximum value per unit of voting power

### 2. Automated Execution
No manual intervention - votes execute programmatically each epoch

### 3. Transparent Allocation
All voting decisions are on-chain and verifiable

## The Optimization Algorithm

### Step 1: Data Collection
Each epoch, the system collects:
- **Bribe Values**: USD value of all bribes per pool
- **Historical Fees**: Past fee generation by pool
- **Base Revenue**: Strategic value assignments
- **Pool Health**: TVL, volume, and gauge status
- **Current Votes**: Essential for calculating ROV (Return on Vote)

### Step 2: Score Calculation

**Components:**
- **Bribes**: Current epoch bribes converted to USD via oracles
- **Discount Factor**: Usually 100%, can be adjusted for risk
- **Base Revenue**: Keeper-assigned strategic value
- **Historical Performance**: 30-day average fees generated
- **Current Votes**: Votes given to pool so far during epoch

### Step 3: Weight Allocation

### Step 4: Rebalancing
Remaining weight after constraints is redistributed using the "largest remainder" method to ensure exactly 100% allocation.
Constraints and Limits
Minimum Vote Weight

Threshold: 0.05% (5 basis points)
Reason: Gas efficiency and impact threshold
Pools below minimum receive no votes

Maximum Pool Allocation

Cap: 70% of voting power
Reason: Risk diversification
Prevents single pool domination

Eligible Pools

Must have active gauge
Must be whitelisted by keeper
Must have sufficient liquidity

Oracle Integration
Price Feeds
All bribes valued using Chainlink oracles:

Real-time price discovery
Manipulation resistance
Multi-token support

Supported Bribe Tokens

AERO
USDC/USDT
ETH/WETH
Major protocol tokens

Staleness Protection

Maximum oracle age: 3600 seconds
Fallback: Skip pools with stale prices

### Strategic Considerations
Base Revenue Assignments
Keepers can assign base revenue to pools for strategic reasons:
High Priority Pools:

iAERO/AERO - Maintain protocol liquidity
AERO/USDC - Core ecosystem pair
Strategic partner pools

### Example Assignments:
iAERO/AERO: $10,000 base revenue (ensures consistent votes)
AERO/USDC: $5,000 base revenue
Partner pools: $1,000-3,000 based on agreements
Multi-Epoch Planning
Bribes can be deposited for multiple epochs:

Provides vote certainty for bribers
Smooths out weekly volatility
Enables long-term partnerships

### Execution Timeline
Weekly Cycle
Wednesday 23:00 UTC: Epoch ends
Thursday 00:00 UTC: New epoch begins
Thursday 00:00-23:59 UTC: Voting window

00:00-12:00: Data collection and calculation
12:00-18:00: Keeper review period
18:00-23:59: Vote execution

Friday: Rewards distribution
Performance Metrics
Key Indicators

Vote Efficiency: $ earned per veAERO voting power
Bribe Capture Rate: % of available bribes earned
Execution Rate: % of epochs successfully voted

Historical Performance
Typical returns by source:

Bribes: 60-70% of total
Trading fees: 20-30% of total
AERO emissions: 10-20% of total

Manual Override
While fully automated, the system includes safety overrides:
Keeper Intervention
Authorized keepers can:

Adjust base revenue values
Add/remove eligible pools
Execute votes with custom weights

Emergency Procedures
If automation fails:

Keeper alerts triggered
Manual vote execution
Post-mortem analysis

Competitive Advantages
vs Manual Voting

Never misses epochs (100% participation)
Optimizes across all pools (not just familiar ones)
Responds to real-time bribes (data-driven)

vs Other Managers

Transparent algorithm (fully on-chain)
No hidden deals (all bribes visible)
Decentralized execution (multiple keepers)

### Future Improvements
Machine Learning Integration

Predict future bribe patterns
Optimize for multi-epoch outcomes
Adapt to market conditions

Cross-Protocol Voting

Coordinate with other protocols
Joint voting strategies
Shared liquidity initiatives

### Governance Integration

LIQ holder input on strategy
Community pool priorities
Revenue sharing adjustments

### Example Allocation
Here's a typical epoch allocation:
PoolBribesBaseScoreWeightAERO/USDC$50,000$5,000$55,00027.5%iAERO/AERO$20,000$10,000$30,00015.0%ETH/USDC$40,000$0$40,00020.0%WBTC/ETH$35,000$0$35,00017.5%Others (10 pools)$40,000$0$40,00020.0%Total$185,000$15,000$200,000100%
This allocation maximizes returns while maintaining strategic positions in key pools.
Verification
All votes can be verified on-chain:

Check VotingManager contract for execution
View vote transaction on Basescan
Verify weights on Aerodrome UI
Calculate expected vs actual returns

The strategy is fully transparent and auditable, ensuring alignment with iAERO staker interests.

