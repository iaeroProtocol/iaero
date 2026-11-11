# Smart Contracts Overview

All contracts are on Base unless stated. Addresses are on [Contract Addresses](../technical-documentation/contract-addresses.md). Updated November 2026.

## Tokens

### iAERO
**Purpose**: liquid token minted for AERO deposited into the protocol's permanent Aero stakes

**Features**:
- ERC-20 with Permit, 18 decimals
- Minted by the deposit contract (AeroStakeDeposit v2 today; PermalockVault V5 before the Aero migration)
- 95% of each deposit to the depositor, 5% to the treasury
- Transferable and tradeable; not redeemable for AERO
- Roles: the protocol Safe holds the admin role, which decides who holds `MINTER_ROLE` and `BURNER_ROLE` (`burnFrom` needs only `BURNER_ROLE`)

### LIQ
**Purpose**: bonus and governance token

**Features**:
- 100,000,000 maximum supply
- Minted with each deposit on a halving schedule
- Burnable by holders
- ERC-20 with Permit

## Aero Stake and Deposits

### AeroStakeVault
**Purpose**: holds iAERO's Aero stakes, including the main stake #1584, and lets them be voted only within limits set by the Safe

**Key functions**:
- `allocate`, `allocateChains`, `allocateGauges`: vote a held stake (keeper only, checked against `chainLimits` and `gaugeCapBps`)
- `claimRewards`: claim a stake's rewards, only to `rewardRecipient(chainId)` (keeper only)
- `setChainLimits`, `setGlobalLimits`, `setRewardRecipient`, `setKeeper`, `setDepositor`: Safe only
- `rescueNFT`: send any stake, #1584 included, to the Safe at once (Safe only; the only function that moves a stake out of the vault). Once held by the Safe, a stake can be transferred or downgraded from permanent on Aero's VotingEscrow
- `heldTokenIds`, `isHeld`, `chainLimits`, `rewardRecipient`: views

**Permissions**:
- Safe: the protocol Safe (3-of-4 multisig)
- Keeper: votes and claims within the limits; cannot move stakes or change settings
- Depositor: AeroStakeDeposit v2, the only contract besides the Safe that can send stakes in

- Recovery: The vault also has a delayed recovery path, meant for a lost Safe: its two designated recovery keys, acting together (one proposes a replacement Safe, the other confirms), can replace the Safe as the vault's owner, but the replacement can only take over 14 days after the confirmation, and the current Safe can cancel at any time until it does. (`proposeRecovery`, `confirmRecovery`, `acceptRecovery`, `cancelRecovery`; `RECOVERY_DELAY` = 14 days, counted from confirmation)

### AeroStakeDeposit v2
**Purpose**: deposit AERO or an Aero stake, receive iAERO and LIQ

**Key functions**:
- `depositAero(amount, minLiqToUser)`: lock 1 to 10,000,000 AERO into a new permanent stake in the vault
- `depositStake(tokenId, minLiqToUser)`: move an Aero stake of at least 5,000 AERO into the vault (made permanent if needed)
- `checkAeroDeposit(user, amount)`, `checkStake(tokenId, user)`: list every reason a deposit would be refused
- `previewDeposit(amount)`: iAERO and LIQ a deposit would mint now
- `setPaused`, `blockStakes`, `sweepStake`, `rescueToken`: Safe only

**Economics** (as V5): 5% protocol fee in iAERO; LIQ at 1 per iAERO before halvings, halving every 5,000,000 LIQ minted, counting V5's LIQ; the treasury receives an extra 20% of the user's LIQ.

## Staking (since November 2026)

### StakedIAERO (xiAERO)
**Purpose**: stake iAERO 1:1 and earn USDC, streamed over time

**Key functions**:
- `stake`, `stakeFor`, `stakeWithPermit`: stake iAERO (paused when staking is paused)
- `withdraw`, `withdrawTo`: return iAERO 1:1 (never paused)
- `getReward`, `claim(receiver)`, `exit`: claim USDC (never paused)
- `earned(account)`, `rewardRate`, `periodFinish`: views
- `notifyRewardAmount`: add USDC to the stream (approved notifiers only; today the RewardConverter)
- `syncSurplus`: anyone can stream USDC sent to the contract directly

**Mechanism**:
- Synthetix-style reward stream; each funding paid out over `rewardsDuration` (3 days)
- Transfers settle both accounts first: earned USDC stays with the earner
- Owner (Safe) can pause new stakes, set notifiers, set future stream length (1 to 30 days); xiAERO's `recoverERC20` cannot remove iAERO, USDC or xiAERO. The iAERO token's own roles (above) are separate

### CompoundingVault (aXiAERO)
**Purpose**: auto-compounding shares of xiAERO

**Key functions**:
- `wrap(amount, maxUSDC, minShares, receiver)`: deposit xiAERO plus a proportional USDC contribution, receive shares
- `redeem(shares, minReceipts, minUSDC, receiver)`: receive xiAERO and reserve USDC (never paused)
- `claimUSDC(receiver)`: claim reserved USDC
- `compound(...)`: keeper only; swap USDC to iAERO via an approved route, at most 2% below the reference price, and stake it
- `previewWrap`, `previewWrapUSDC`, `previewRedeem`, `pooledUSDC`: views

### RewardConverter
**Purpose**: receives the stakers' share of each harvest and turns it into USDC for xiAERO

**Key functions**:
- `notifyRewardAmount(token, amount)`: intake from the RewardsHarvester
- `convert(swaps)`: keeper only; up to 32 swaps per call, each with an on-chain minimum from the token's price reference
- `push`: send the USDC to xiAERO

**Notes**: no raw-token payout to users; tokens that cannot be sold wait in the converter. Immediate (Safe): `setRetired(token, true)` (the harvester then sends the stakers' share of that token to the TreasuryDistributor), `setFrozen`, a tightening `setRule`, `setMinPushUSDC`, `pause`. After a 2-day queue: adding a keeper or funder, any other `setRule`, `setRequireOracle(false)`, un-retiring. `releaseRetired` sends retired tokens already held to the TreasuryDistributor 2 days after retirement.

### SwapPolicy, PairPriceOracle, AeroIAEROChainlinkFreshTwapFeed
- **SwapPolicy**: the swap routes the converter and the compounding vault may call. New routes wait 2 days; removal is immediate.
- **PairPriceOracle**: iAERO/USDC reference = iAERO/USD ÷ Chainlink USDC/USD.
- **AeroIAEROChainlinkFreshTwapFeed**: iAERO/USD = 30-minute TWAP of the Aero iAERO/AERO CL pool × Chainlink AERO/USD.

## Rewards

### RewardsHarvesterV2
**Purpose**: collect each week's rewards and split them

**Distribution** (fixed: `PROTOCOL_BASE_BPS` = 1,000, `PEG_ACTION_BPS` = 1,000):
- 80% → stakers (`stakingDistributor` = the RewardConverter)
- 10% → TreasuryDistributor
- 10% → peg reserve

The percentages are constants. The destinations are not: the Safe can change them at once with `setStakingDistributor`, `setTreasuryDistributor` and `setPegDefender`, and can move tokens held by the harvester with `rescueTokens`.

### TreasuryDistributor
**Purpose**: split the treasury's share between LIQ stakers and operations

**Split** (of the 10% it receives; `LIQ_SHARE_BPS` = 8,000, `TREASURY_KEEP_BPS` = 2,000):
- 80% → LIQ stakers (8% of total)
- 20% → treasury (2% of total)

### AeroRewardForwarder v2 and AeroLeafRewardVault
- **AeroRewardForwarder v2**: #1584's reward address on Base; claims and forwards to the RewardsHarvester.
- **AeroLeafRewardVault**: #1584's reward address on Ethereum, Optimism, Arbitrum, Robinhood and Ink (same address on each); the rewards are sold and the proceeds delivered to the RewardsHarvester on Base in USDC.

## Contract Interactions

```
User ── AERO or stake ──► AeroStakeDeposit v2 ── stake ──► AeroStakeVault (#1584 + deposited stakes)
 ▲                               │                               │ votes (keeper, within the Safe's limits)
 └──────── iAERO + LIQ ──────────┘                               ▼
                                                      Aero Voter (Base + 5 other chains)
                                                                 │ rewards
                         AeroRewardForwarder v2 (Base) / AeroLeafRewardVault (other chains)
                                                                 ▼
                                                        RewardsHarvesterV2
                    ┌────────────────────────────────────────────┼──────────────────────────┐
                   80%                                          10%                        10%
                    ▼                                            ▼                          ▼
             RewardConverter                             TreasuryDistributor           Peg reserve
                    │ USDC                               80% │           │ 20%
                    ▼                                        ▼           ▼
  User ──► StakedIAERO (xiAERO) ◄── wrap ── CompoundingVault    LIQ Staking     Treasury
           (stake iAERO)                    (aXiAERO)            Distributor
```

## Legacy Contracts

- **PermalockVault V5**: managed veAERO NFTs on Aerodrome (`deposit`, `depositVeNFT`, `executeNFTAction`, `performMaintenance`). Retired; deposits paused.
- **VotingManager**: voted V5's veNFT on Aerodrome. Retired.
- **EpochStakingDistributor and stiAERO**: the previous iAERO staking, with rewards per token and weekly epoch. No new funding after the 5 November 2026 harvest. Withdrawals and claims of funded rewards are intended to stay open; the distributor is owned by the Safe, and its `unstake`, `exit` and claim functions stop while it is paused.
- **iAERO Auto-USDC Vault**: deposited into the previous staking and paid USDC one week late. Its final week is due to be processed after the 12 November 2026 harvest.
