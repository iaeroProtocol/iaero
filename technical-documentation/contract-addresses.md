# Contract Addresses — Base Mainnet

All addresses are on Base unless a section says otherwise. Updated 11 November 2026.

## Core Tokens
- **iAERO**  
  `0x81034Fb34009115F215f5d5F564AAc9FfA46a1Dc`

- **LIQ**  
  `0x7ee8964160126081cebC443a42482E95e393e6A8`

- **xiAERO** ("Staked iAERO (USDC)"; see [What is xiAERO?](../getting-started/what-is-xiaero.md))  
  `0x44D8172b95a208082327f58c5b42269d48a34847`

- **aXiAERO** ("Compounding Staked iAERO"; see [What is aXiAERO?](../getting-started/what-is-axiaero.md))  
  `0xa00FeB9AA79612f380D523894321F52AD34EB416`


## Treasury
- **Protocol Safe / Treasury MultiSig** (3-of-4; owns or administers the contracts below)  
  `0x1039CB48254a3150fC604d4B9ea08F66f4739D37`


## Staking (since November 2026)
- **StakedIAERO (xiAERO)**: stake iAERO 1:1, earn streamed USDC  
  `0x44D8172b95a208082327f58c5b42269d48a34847`

- **CompoundingVault (aXiAERO)**: auto-compounding shares of xiAERO  
  `0xa00FeB9AA79612f380D523894321F52AD34EB416`

- **RewardConverter**: receives the stakers' share of each harvest, sells it for USDC and funds xiAERO  
  `0xA397Ad9f43967b9AE7Ab54E2dDf61CF8c5585bd8`

- **SwapPolicy**: the swap routes the converter and the compounding vault may use  
  `0x2F75ae9a734ddDD073E28fDe72E54991E37d28f0`

- **PairPriceOracle**: iAERO/USDC reference price used for compounding  
  `0x9b801f50AD1fC458146ec69E0010BcDB926E5705`

- **AeroIAEROChainlinkFreshTwapFeed**: iAERO/USD price (Aero iAERO/AERO 30-minute TWAP × Chainlink AERO/USD)  
  `0x65B65Def72080EBc82b999D91437f6e083504e23`


## Aero Stake and Deposits
- **AeroStakeVault**: holds iAERO's permanent Aero stake #1584 and stakes deposited by users; votes them within limits set by the Safe  
  `0xA21cA21973681bB68A42B8387E86cA7c144D2de2`

- **AeroStakeDeposit v2**: deposit AERO or an Aero stake, receive iAERO and LIQ  
  `0xc9B2D05873a3c49a4142a04a2DCBda81aCC2570D`

- **Aero stake #1584**: token ID 1584 on the Aero VotingEscrow (`0xBF7977E1521e9a3032bf7243Ee4712f311f13915`)


## Rewards
- **RewardsHarvester**: weekly harvest and split (80% stakers / 10% TreasuryDistributor / 10% peg reserve)  
  `0x77f90d2dDB15Ffe28fa322aDA351d11da3B8bFe5`

- **AeroRewardForwarder v2**: claims #1584's Base rewards and forwards them to the RewardsHarvester  
  `0x5ceEF9c4D123c0D0b52311403A30cE115d973884`

- **AeroLeafRewardVault**: receives #1584's rewards on each other chain; they are sold and sent back to the RewardsHarvester on Base. Same address on Ethereum, Optimism, Arbitrum, Robinhood and Ink  
  `0xC6bcDf0d212341B4d6fA6Dd94cEed6431E08985B`

- **TreasuryDistributor**: splits its share 80% to LIQ stakers and 20% to the treasury  
  `0xD36b84EeFd1F481a737595C8212c43A9cD76C8e0`

- **LIQ Staking Distributor**  
  `0xb81efc6be6622Bf4086566210a6aD134cd0CDdA4`

- **LIQ Linear Vester**  
  `0xF1d25F4ee64988Afad0f1612cc3d540725F319Db`

- **RewardsSwapper**  
  `0x25f11f947309df89bf4d36da5d9a9fb5f1e186c1`

## Aero Integrations (since 22 Oct 2026)
See [Aero Migration (22 Oct 2026)](../getting-started/aero-migration.md). Aero's full contract list is at [app.aero.xyz/docs/security](https://app.aero.xyz/docs/security).

- **AERO (Aero)**  
  `0x3f5bF3EdAc59d650D8fDf9657a71F2f904A3a4Ee`

- **Aero Voter**  
  `0x49d7d4f17Ec56Ef8fA04E464AF5D1A4942CC31e6`

- **Aero VotingEscrow** (iAERO's permanent stake is #1584)  
  `0xBF7977E1521e9a3032bf7243Ee4712f311f13915`

- **Aero Metarouter**  
  `0x0fEf9e88A7b76BCDBF451780C6774FE32965Ee98`

- **Aero v2 Pool Factory (volatile)**  
  `0x8E6a9FCd1d638CC760Ffba397605Fe3355dE76D2`

- **Aero v2 Pool Factory (stable)**  
  `0x78589E876edcEbdce72ee3436ab3c9AD3a7cEF2D`

- **Aero CL Pool Factory**  
  `0x9CB220581b91b0C83223012FE69688b92aD07a76`

## Pools (Aero)
- **AERO/iAERO v2 (volatile)**  
  `0x72Decb30DFE4F6Cf7Db3ba1D58411dDda5ed903b`

- **AERO/iAERO CL (tick spacing 100)**  
  `0x361D728c97E591Ef47A68CAe2DC5C8ef475608EB`

- **LIQ/USDC**  
  `0xE4553d8d7284192ab84346A9C9FFdb69dCE30C71`

- **AERO/USDC (pricing)**  
  `0xF634058dC94e6BF8e828E27621320C423bC8B18E`

The old Aerodrome pools have been drained.

## Legacy / Retired

### Previous staking (no new rewards after the 5 November 2026 harvest)
Holders can still withdraw and claim rewards already funded. See [Moving from stiAERO or the Auto-USDC Vault](../getting-started/guides/moving-from-stiaero.md).

- **stiAERO** (receipt for the previous staking)  
  `0x72C135B8eEBC57A3823f0920233e1A90FF4D683D`

- **EpochStakingDistributor** (the previous staking)  
  `0x781A80fA817b5a146C440F03EF8643f4aca6588A`

- **Auto-USDC Vault** (final reward week due to be processed after the 12 November 2026 harvest)  
  `0xFE5c929677D97723dc822C86c93c7e2D1B59c774`

### Retired since the Aero migration
- **AeroRewardForwarder v1** (Base only; #1584's Base forwarder until #1584 moved into the AeroStakeVault on 5 November 2026. On the other chains this address is the AeroLeafRewardVault)  
  `0xC6bcDf0d212341B4d6fA6Dd94cEed6431E08985B`

### Aerodrome era
- **PermalockVault V5** (retired: deposits paused; the protocol's veAERO position was moved out on 22 Oct 2026)  
  `0x180DAB53968e599Dd43CF431E27CB01AA5C37909`

- **VotingManager** (voted PermalockVault V5's veNFT on Aerodrome)  
  `0xA0EBbdeD0E201a9C37F4Bffbb831CB8Db9FEd0B8`

- **AERO (Aerodrome, old)**  
  `0x940181a94a35a4569e4529a3cdfb74e38fd98631`

- **Aerodrome VOTER** (legacy)  
  `0x16613524e02ad97eDfeF371bC883F2F5d6C480A5`

- **veAERO (Aerodrome VotingEscrow, legacy)**  
  `0xeBf418Fe2512e7E6bd9b87a8F0f294aCDC67e6B4`

- **Aerodrome Router** (legacy)  
  `0xcF77a3Ba9A5CA399B7c97c74d54e5b1Beb874E43`

- **Aerodrome RewardsSugar** (legacy)  
  `0xD4aD2EeeB3314d54212A92f4cBBE684195dEfe3E`
