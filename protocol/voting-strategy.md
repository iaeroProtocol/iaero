# Voting Strategy: Maximizing Protocol Returns

> **Updated November 2026.** Since 5 November 2026, iAERO's main stake #1584 is held by the AeroStakeVault and voted by an automated keeper on Aero, across several chains. The previous system, the VotingManager voting PermalockVault V5's veNFT on Aerodrome, is retired: see [Legacy](#legacy-the-votingmanager-on-aerodrome) at the end.

## Overview

iAERO's votes decide which Aero pools its stake supports. Voters earn the trading fees and incentives paid to the pools they vote for, so better votes mean more rewards for iAERO stakers. iAERO's votes are planned with data and sent automatically, inside limits that are enforced on-chain.

## What Is Voted

- **#1584**, iAERO's permanent Aero stake: 4,853,405.56 AERO as of 11 November 2026.
- It is held by the **AeroStakeVault** (`0xA21cA21973681bB68A42B8387E86cA7c144D2de2`) on Base.

## Where It Can Vote

Aero lets one stake split its votes across several chains. The vault lets the keeper vote on the chains where iAERO's reward payout is set up. As of 11 November 2026 these are:

| Chain | Chain ID | Rewards are claimed to |
|---|---|---|
| Base | 8453 | AeroRewardForwarder v2 `0x5ceEF9c4D123c0D0b52311403A30cE115d973884` |
| Ethereum | 1 | AeroLeafRewardVault `0xC6bcDf0d212341B4d6fA6Dd94cEed6431E08985B` |
| Optimism | 10 | AeroLeafRewardVault (same address) |
| Arbitrum | 42161 | AeroLeafRewardVault (same address) |
| Robinhood | 4663 | AeroLeafRewardVault (same address) |
| Ink | 57073 | AeroLeafRewardVault (same address) |

Optimism and Ink were added on 11 November 2026; the other four have been allowed since #1584 moved into the vault on 5 November 2026. The protocol Safe decides which chains are allowed.

## How the Votes Are Chosen

1. **Forecast.** For every pool on every allowed chain, the keeper estimates the fees and incentives a vote is likely to earn, taking account of the votes already there.
2. **Optimise.** It works out the split of #1584 across chains and pools that is expected to earn the most.
3. **Check.** It simulates the exact transaction before sending it. If anything is unclear, it does nothing: the existing votes stay in place and keep earning.
4. **Send.** It sends the votes through the AeroStakeVault, which checks them against the on-chain limits.

### How often

Aero lets a stake change its votes on a chain once per cooldown. On Base the cooldown is currently 169,200 seconds (47 hours). The keeper re-plans when each cooldown ends.

## On-Chain Limits

The AeroStakeVault enforces limits that the protocol Safe sets and can change. The keeper cannot raise them or move the stake. On Base, rewards can only be claimed to the AeroRewardForwarder v2, which passes them to the RewardsHarvester. On Ethereum, Optimism, Arbitrum, Robinhood and Ink, rewards collect in iAERO's leaf reward vault, which is managed by the iAERO team, before they are sold and bridged to Base. The vault also has a delayed recovery path for a lost Safe: see [Recovery path](vault-system.md#recovery-path).

| Limit | Setting on 11 November 2026 |
|---|---|
| Allowed chains | The six chains above |
| Share of the stake on each chain | 0% to 100% |
| Cap per pool | None (100%) |
| Base pools | Must be live (activated gauge, non-zero emission cap) |
| Reward addresses | Fixed per chain, as in the table above |

## Where the Rewards Go

The rewards are collected every week and split: 80% to stakers in USDC, 10% to the TreasuryDistributor (for LIQ stakers and the treasury), 10% to the peg reserve. See [Rewards Harvesting](../getting-started/convert-or-compound.md).

## Verification

Anyone can check iAERO's votes on-chain:
- **Owner of #1584:** `ownerOf(1584)` on the Aero VotingEscrow (`0xBF7977E1521e9a3032bf7243Ee4712f311f13915`) returns the AeroStakeVault.
- **Chains #1584 votes on:** `allocationChainIds(1584)` on the Aero Voter (`0x49d7d4f17Ec56Ef8fA04E464AF5D1A4942CC31e6`).
- **Allowed chains and limits:** `chainLimits(chainId)` and `rewardRecipient(chainId)` on the AeroStakeVault.
- **Vote transactions:** the AeroStakeVault's transactions on [Basescan](https://basescan.org/address/0xA21cA21973681bB68A42B8387E86cA7c144D2de2).

## Legacy: the VotingManager on Aerodrome

Until the Aero migration on 22 October 2026, the **VotingManager** (`0xA0EBbdeD0E201a9C37F4Bffbb831CB8Db9FEd0B8`) voted PermalockVault V5's veNFT on Aerodrome once per weekly epoch. It scored each pool by its incentives (valued with Chainlink prices) and fees, applied a minimum vote weight and a per-pool cap, and allocated the votes proportionally. Between 22 October and 5 November 2026, #1584 was voted by the protocol Safe directly. The VotingManager is retired; its audit report is at [Audit Voting Manager Optimised](../technical/AUDIT-VotingManagerOptimised.md).
