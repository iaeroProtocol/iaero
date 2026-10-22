# Aero Migration (22 Oct 2026)

On 22 October 2026, Aero replaced Aerodrome on Base. This page sets out what changed for iAERO and what did not.

## What happened

- iAERO's veAERO position (4.85M AERO) was moved out of PermalockVault V5 and converted 1:1 into a **permanent Aero stake, #1584** on the Aero VotingEscrow (`0xBF7977E1521e9a3032bf7243Ee4712f311f13915`).
- A new sAERO vault comes next. These docs will be updated when it is live.

## What is paused

- **Deposits into PermalockVault V5 are paused**, for both AERO and veNFTs. The [Depositing AERO](guides/deposit-aero.md) and [Depositing veNFTs](guides/deposit-venft.md) flows do not work while the pause is in place.

## What works as normal

- Staking iAERO
- Unstaking iAERO
- Claiming rewards

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
| StakingDistributor (Epoch Distributor) | `0x781A80fA817b5a146C440F03EF8643f4aca6588A` |
| stiAERO | `0x72C135B8eEBC57A3823f0920233e1A90FF4D683D` |
| Auto-USDC Vault | `0xFE5c929677D97723dc822C86c93c7e2D1B59c774` |
| RewardsSwapper | `0x25f11f947309df89bf4d36da5d9a9fb5f1e186c1` |
| Treasury MultiSig | `0x1039CB48254a3150fC604d4B9ea08F66f4739D37` |

## Retired

- **PermalockVault V5** (`0x180DAB53968e599Dd43CF431E27CB01AA5C37909`): deposits paused; the protocol's veAERO position was moved out of it on 22 Oct 2026.
- **Aerodrome contracts** (old AERO, Voter, veAERO, Router, RewardsSugar): replaced by Aero. See [Contract Addresses](../technical-documentation/contract-addresses.md) for the legacy list.
