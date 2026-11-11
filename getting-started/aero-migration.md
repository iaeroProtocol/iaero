# Aero Migration (22 Oct 2026)

On 22 October 2026, Aero replaced Aerodrome on Base. This page sets out what changed for iAERO and what did not.

> **Updated 11 November 2026.** Several things have changed since 22 October: the stake #1584 now sits in the AeroStakeVault and is voted automatically, deposits have reopened, and staking has moved to xiAERO and aXiAERO. See [What changed after 22 October](#what-changed-after-22-october) below.

## What happened

- iAERO's veAERO position (4.85M AERO) was moved out of PermalockVault V5 and converted 1:1 into a **permanent Aero stake, #1584** on the Aero VotingEscrow (`0xBF7977E1521e9a3032bf7243Ee4712f311f13915`).
- A new vault for #1584 came next. It went live on 5 November 2026 (see below).

## What was paused

- **Deposits into PermalockVault V5 were paused**, for both AERO and veNFTs, and V5 stays retired. Deposits reopened on 5 November 2026 through a new contract, AeroStakeDeposit v2: see [Depositing AERO](guides/deposit-aero.md) and [Depositing an Aero stake](guides/deposit-venft.md).

## What changed after 22 October

- **5 November 2026: #1584 moved into the AeroStakeVault.** The protocol Safe transferred #1584 into the [AeroStakeVault](../protocol/vault-system.md) (`0xA21cA21973681bB68A42B8387E86cA7c144D2de2`). Since then an automated keeper votes it, inside limits the Safe sets on-chain. See [Voting Strategy](../protocol/voting-strategy.md).
- **5 November 2026: deposits reopened.** AeroStakeDeposit v2 (`0xc9B2D05873a3c49a4142a04a2DCBda81aCC2570D`) takes new AERO, or an existing Aero stake, and mints iAERO and LIQ by the same rules as V5.
- **November 2026: new staking.** Staking iAERO now gives you [xiAERO](what-is-xiaero.md), which earns USDC, or [aXiAERO](what-is-axiaero.md), which compounds. The 5 November 2026 harvest was the last one paid to the old staking (stiAERO). The Auto-USDC Vault, which pays one week late, is due to process that last week after the 12 November 2026 harvest. On 10 November 2026 the stakers' share of each harvest was pointed at the new RewardConverter. The first funding of the new staking is planned for the harvest on 12 November 2026. If you staked before, see [Moving from stiAERO or the Auto-USDC Vault](guides/moving-from-stiaero.md).
- **Rewards from more chains.** #1584 can now be voted on Base, Ethereum, Optimism, Arbitrum, Robinhood and Ink. Rewards earned on other chains are sold and sent back to Base, where they join the weekly harvest. See [Rewards Harvesting](convert-or-compound.md).

## Tokens

Both the old and the new token use the symbol **AERO**. Always check the address.

| Token | Address |
|-------|---------|
| AERO (Aero, new) | `0x3f5bF3EdAc59d650D8fDf9657a71F2f904A3a4Ee` |
| AERO (Aerodrome, old) | `0x940181a94A35A4569E4529A3CDfB74e38FD98631` |
| iAERO (unchanged) | `0x81034Fb34009115F215f5d5F564AAc9FfA46a1Dc` |
| LIQ (unchanged) | `0x7ee8964160126081cebC443a42482E95e393e6A8` |

## New pools on Aero

| Pool | Address |
|------|---------|
| AERO/iAERO v2 (volatile) | `0x72Decb30DFE4F6Cf7Db3ba1D58411dDda5ed903b` |
| AERO/iAERO CL, tick spacing 100 | `0x361D728c97E591Ef47A68CAe2DC5C8ef475608EB` |
| LIQ/USDC | `0xE4553d8d7284192ab84346A9C9FFdb69dCE30C71` |
| AERO/USDC (used for pricing) | `0xF634058dC94e6BF8e828E27621320C423bC8B18E` |

The old Aerodrome pools have been drained. Do not trade or add liquidity there.

Swap on Aero at [app.aero.xyz/swap](https://app.aero.xyz/swap). Aero's full list of deployed contracts is at [app.aero.xyz/docs/security](https://app.aero.xyz/docs/security).

## Aero contracts

| Contract | Address |
|----------|---------|
| Voter | `0x49d7d4f17Ec56Ef8fA04E464AF5D1A4942CC31e6` |
| VotingEscrow | `0xBF7977E1521e9a3032bf7243Ee4712f311f13915` |
| Metarouter | `0x0fEf9e88A7b76BCDBF451780C6774FE32965Ee98` |
| v2 pool factory (volatile) | `0x8E6a9FCd1d638CC760Ffba397605Fe3355dE76D2` |
| v2 pool factory (stable) | `0x78589E876edcEbdce72ee3436ab3c9AD3a7cEF2D` |
| CL pool factory | `0x9CB220581b91b0C83223012FE69688b92aD07a76` |

## Unchanged iAERO contracts

| Contract | Address |
|----------|---------|
| iAERO | `0x81034Fb34009115F215f5d5F564AAc9FfA46a1Dc` |
| LIQ | `0x7ee8964160126081cebC443a42482E95e393e6A8` |
| RewardsSwapper | `0x25f11f947309df89bf4d36da5d9a9fb5f1e186c1` |
| Treasury MultiSig | `0x1039CB48254a3150fC604d4B9ea08F66f4739D37` |

The previous staking contracts, StakingDistributor (Epoch Distributor) `0x781A80fA817b5a146C440F03EF8643f4aca6588A`, stiAERO `0x72C135B8eEBC57A3823f0920233e1A90FF4D683D` and the Auto-USDC Vault `0xFE5c929677D97723dc822C86c93c7e2D1B59c774`, are now legacy. The new contracts are on [Contract Addresses](../technical-documentation/contract-addresses.md).

## Retired

- **PermalockVault V5** (`0x180DAB53968e599Dd43CF431E27CB01AA5C37909`): deposits paused; the protocol's veAERO position was moved out of it on 22 Oct 2026.
- **Aerodrome contracts** (old AERO, Voter, veAERO, Router, RewardsSugar): replaced by Aero. See [Contract Addresses](../technical-documentation/contract-addresses.md) for the legacy list.
