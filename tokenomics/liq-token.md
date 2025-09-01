# LIQ Token Economics

## Overview

LIQ is the governance and incentive token of the iAERO ecosystem, designed with a deflationary emission schedule and multiple value accrual mechanisms.

## Token Specifications

- **Token Name**: Liquid
- **Symbol**: LIQ
- **Max Supply**: 100,000,000 LIQ
- **Decimals**: 18
- **Token Type**: ERC20 with Permit

## Supply Distribution

Total Supply: 100,000,000 LIQ
├── Community Emissions: 30,000,000 (40%)
├── Treasury Vesting: 10,000,000 (10%)
├── Team Vesting: 10,000,000 (10%)
├── Investor Vesting: 30,000,000 (20%)
└── Remaining: 20,000,000 (20%) - Protocol reserves

## Emission Schedule

### Community Emissions (60M LIQ)

LIQ follows a halving schedule for community emissions:

- **Initial Rate**: 1 LIQ per 1 iAERO minted
- **Halving Interval**: Every 5,000,000 LIQ minted
- **Treasury Take**: 20% of all user emissions

#### Halving Timeline

| Milestone | Total Minted | Emission Rate | Per iAERO |
|-----------|--------------|---------------|-----------|
| Start | 0 | 1.0x | 1.0 LIQ |
| Halving 1 | 5,000,000 | 0.5x | 0.5 LIQ |
| Halving 2 | 10,000,000 | 0.25x | 0.25 LIQ |
| Halving 3 | 15,000,000 | 0.125x | 0.125 LIQ |
| Halving 4 | 20,000,000 | 0.0625x | 0.0625 LIQ |
| ... | ... | ... | ... |

### Vesting Schedule (20M LIQ)

20% of supply is linearly vested over 3 years:

- **Treasury**: 10,000,000 LIQ (3 years linear)
- **Team**: 10,000,000 LIQ (3 years linear)
- **Daily Release**: ~9,132 LIQ per day per stream
- **Vesting Contract**: LIQLinearVester.sol

## Value Accrual Mechanisms

### 1. Staking Rewards (80% of Protocol Revenue)

LIQ stakers receive the majority of protocol rewards:

Protocol Revenue Flow:
├── 80% → LIQ Stakers (via TreasuryDistributor)
└── 20% → Treasury Operations

### 2. Governance Rights

LIQ holders control:
- Voting on protocol parameters
- Treasury allocation
- Strategic partnerships
- Protocol upgrades

### 3. Deflationary Pressure

- **Capped Supply**: Hard cap at 100M
- **Halving Emissions**: Decreasing inflation over time
- **Burn Mechanism**: Users can burn LIQ tokens

## Staking LIQ

Stake LIQ to earn protocol revenues:

- **Contract**: StakingDistributor (for LIQ staking)
- **Rewards**: 80% of treasury's 10% protocol fee share
- **Tokens**: AERO, USDC, ETH, and other bribes
- **Compounding**: Auto-compound by restaking rewards

## Treasury Management

The protocol treasury receives:
- 20% of all LIQ emissions
- 10% of all protocol fees
- Vested allocation (10M over 3 years)

Treasury funds are used for:
- Protocol development
- Liquidity provision
- Strategic partnerships
- Security audits

