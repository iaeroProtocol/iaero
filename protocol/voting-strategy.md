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

```python
# Simplified allocation logic
total_score = sum(all pool scores)
for each pool:
    weight = (pool_score / total_score) × 100%
    
    # Apply constraints
    if weight < 0.05%:
        weight = 0  # Too small, skip
    if weight > 70%:
        weight = 70%  # Cap single pool exposure
