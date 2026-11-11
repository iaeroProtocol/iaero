# Aero Stake (veNFT) Management

> **Updated November 2026.** On Aero, the old veAERO NFTs are replaced by **Aero stakes**: NFTs on Aero's VotingEscrow (`0xBF7977E1521e9a3032bf7243Ee4712f311f13915`). This page describes how iAERO manages its Aero stakes today. The PermalockVault V5 approach (re-locking every few months and merging NFTs on Aerodrome) is retired: see the [Legacy](#legacy-venft-management-in-permalockvault-v5) section at the end.

## The Main Stake: #1584

- **What it is:** iAERO's main Aero stake, token ID **1584** on Aero's VotingEscrow.
- **Size:** 4,853,405.56 AERO as of 11 November 2026.
- **Lock:** permanent. A permanent stake keeps its full voting power and never needs re-locking, so there is no rebase or re-lock routine.
- **Where it is held:** in the **AeroStakeVault** (`0xA21cA21973681bB68A42B8387E86cA7c144D2de2`) since 5 November 2026.
- **History:** created on 22 October 2026 by converting iAERO's veAERO from PermalockVault V5 1:1 into a permanent Aero stake. It was held by the protocol Safe until it moved into the vault.

You can check all of this on-chain: `ownerOf(1584)` on the VotingEscrow returns the AeroStakeVault, and `staked(1584)` returns the amount and `isPermanent = true`.

## Stakes Deposited by Users

Users can deposit their own Aero stakes through AeroStakeDeposit v2 (see [Depositing an Aero stake](../getting-started/guides/deposit-venft.md)):
- each deposited stake must hold at least 5,000 AERO;
- it is made permanent if it was not already;
- it is held in the AeroStakeVault next to #1584, permanently locked.

## What Can Happen to a Stake in the Vault

| Action | Possible? | Who |
|---|---|---|
| Vote on Aero pools | Yes | The keeper, within the Safe's on-chain limits |
| Claim rewards | Yes | To the fixed reward address for each chain only |
| Withdraw to a user | No | Deposits are one-way |
| Unlock the AERO | No | The stakes are permanent |
| Move out of the vault | Only to the protocol Safe | The Safe |
| Change the limits or reward addresses | Yes | The Safe only |

## Voting Across Chains

Aero lets one stake split its votes across several chains. The AeroStakeVault lets iAERO's stakes vote on Base, Ethereum, Optimism, Arbitrum, Robinhood and Ink. Votes on other chains are sent from Base as cross-chain messages, and the rewards earned there are claimed on that chain. See [Voting Strategy](voting-strategy.md) and [Rewards Harvesting](../getting-started/convert-or-compound.md).

## Benefits vs Holding a Stake Yourself

- No votes to plan or send.
- No rewards to claim on several chains, or tokens to sell.
- A liquid token (iAERO) instead of a locked NFT.
- Rewards for stakers in USDC.

## Legacy: veNFT Management in PermalockVault V5

Until the Aero migration on 22 October 2026, PermalockVault V5 (`0x180DAB53968e599Dd43CF431E27CB01AA5C37909`) managed iAERO's veAERO on Aerodrome:
- one **primary NFT** held the bulk of the voting power at the maximum 4-year lock;
- a keeper **re-locked** it back to 4 years whenever the remaining lock fell below about 3.75 years;
- deposited veNFTs were **merged** into the primary NFT during maintenance;
- the VotingManager voted the primary NFT every week.

V5 is retired and its deposits are paused. Its audit report is at [Audit PermaLock Vault](../technical/AUDIT-PermaLockVault_V5.md).
