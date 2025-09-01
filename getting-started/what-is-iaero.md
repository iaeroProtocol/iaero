# What is iAERO?

iAERO is a liquid staking derivative protocol built on top of Aerodrome Finance's veAERO system.

## The Problem

Traditional veAERO locking requires users to choose between:
- **Liquidity** - Keeping AERO liquid but earning no voting rewards
- **Rewards** - Locking AERO for up to 4 years but losing access to capital

## The Solution

iAERO solves this dilemma by:

1. **Pooling deposits** into a protocol-owned vault
2. **Permanently locking** veAERO positions for maximum voting power
3. **Issuing iAERO tokens** as liquid receipts for deposits
4. **Distributing rewards** to iAERO stakers

## How It Works

### Key Components

- **PermalockVault**: Manages veAERO NFTs and deposits
- **iAERO Token**: 1:1 receipt token for AERO deposits (minus fees)
- **LIQ Token**: Bonus emission token with halving schedule
- **StakingDistributor**: Handles reward distribution to stakers
- **RewardsHarvester**: Claims and processes Aerodrome rewards

