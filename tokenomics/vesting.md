# LIQ Vesting Schedule

## Overview

40,000,000 LIQ (40% of total supply) is allocated to long-term stakeholders through linear vesting contracts.

## Vesting Streams

### Stream 0: Treasury
- **Amount**: 10,000,000 LIQ
- **Duration**: 3 years (1,095 days)
- **Start**: Token Generation Event (TGE)
- **Daily Release**: ~9,132 LIQ
- **Purpose**: Protocol development and operations

### Stream 1: Team
- **Amount**: 10,000,000 LIQ
- **Duration**: 3 years (1,095 days)
- **Start**: Token Generation Event (TGE)
- **Daily Release**: ~9,132 LIQ
- **Purpose**: Team incentive alignment

- ### Stream 2: Investors
- **Amount**: 20,000,000 LIQ
- **Duration**: 3 years (1,095 days)
- **Start**: Token Generation Event (TGE)
- **Daily Release**: ~9,132 LIQ
- **Purpose**: Investor incentive alignment

## Vesting Mechanics

### Linear Release
Tokens vest linearly over time:

Vested(t) = Total × (t - start) / duration

### Claiming
- **Pull Model**: Anyone can trigger claims
- **Recipient**: Funds go directly to beneficiary
- **No Admin Control**: Immutable once deployed

## Vesting Chart

<div id="vesting-chart"></div>

## Contract Details

**LIQLinearVester.sol**
- Holds 20M LIQ at deployment
- Immutable beneficiaries
- No pause or modification functions
- Rescue function for non-LIQ tokens only
