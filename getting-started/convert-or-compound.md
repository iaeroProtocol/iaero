# Rewards Harvesting

How iAERO's voting rewards are collected, converted to USDC and paid to stakers.

> **Updated November 2026.** Stakers no longer receive dozens of reward tokens to claim and swap. The stakers' share of every harvest is now sold for USDC automatically and streamed to [xiAERO](what-is-xiaero.md) holders. The claim-and-convert tools described on earlier versions of this page are still available for rewards from the previous staking: see [Legacy rewards](#legacy-rewards-from-the-previous-staking) below.

## The flow at a glance

```
iAERO's stake #1584 votes on Aero
  ├── Base rewards ─────────► AeroRewardForwarder v2 ─────────────┐
  └── Rewards on Ethereum, Optimism, Arbitrum, Robinhood, Ink      │
        ─► AeroLeafRewardVault on that chain                       │
        ─► sold and sent back to Base as USDC ─────────────────────┤
                                                                   ▼
                                RewardsHarvester (weekly, Thursday 00:01 UTC)
          ┌───────────────────────────────┬────────────────────────┴──────┐
         80%                             10%                             10%
          ▼                               ▼                               ▼
   RewardConverter                 TreasuryDistributor                Peg reserve
   (sells everything for USDC)     (80% LIQ stakers, 20% treasury)
          ▼
   xiAERO USDC stream (each funding paid out over 3 days)
          ▼
   xiAERO holders claim USDC · the aXiAERO vault compounds its USDC
```

## Step by step

### 1. Earning
iAERO's main stake, #1584, votes for pools on Aero. Voters earn the trading fees and incentives paid to the pools they vote for. Because Aero lets one stake vote on several chains, #1584 can earn on Base, Ethereum, Optimism, Arbitrum, Robinhood and Ink. See [Voting Strategy](../protocol/voting-strategy.md).

### 2. Collecting
- **On Base**, the rewards are claimed by the **AeroRewardForwarder v2** (`0x5ceEF9c4D123c0D0b52311403A30cE115d973884`) and passed straight to the RewardsHarvester.
- **On each other chain**, the rewards are claimed into iAERO's **AeroLeafRewardVault** on that chain (`0xC6bcDf0d212341B4d6fA6Dd94cEed6431E08985B`, the same address on every chain). They are then sold, and the proceeds are delivered to the RewardsHarvester on Base in USDC.
- The AeroStakeVault, which holds #1584, only lets rewards be claimed to these addresses. Only the protocol Safe can change them.

### 3. Splitting
Every week (Thursday, 00:01 UTC) the **RewardsHarvester** (`0x77f90d2dDB15Ffe28fa322aDA351d11da3B8bFe5`) splits what it has collected. The split is fixed in the contract:

| Share | Goes to |
|---|---|
| 80% | **Stakers**, through the RewardConverter |
| 10% | **TreasuryDistributor**, which passes 80% of it to LIQ stakers and keeps 20% for the treasury |
| 10% | **Peg reserve** |

### 4. Converting to USDC
The **RewardConverter** (`0xA397Ad9f43967b9AE7Ab54E2dDf61CF8c5585bd8`) receives the stakers' 80% in whatever tokens were earned.

- The protocol's keeper sells each token for USDC, in batches of up to 32 tokens, using only swap routes approved in the SwapPolicy contract.
- Every sale must return at least a minimum worked out on-chain from a price reference for that token (a price feed, or a fixed reference price set by the Safe). If a token has no working price reference, or cannot be sold at that price, it waits in the converter until it can be.
- Stakers are only ever paid in USDC. The converter has no way to pay out the raw tokens.
- If a token can never be sold, the Safe can retire it. Two days later it can be released to the TreasuryDistributor instead.

### 5. Streaming
- Whenever the converter holds at least 1 USDC, it adds the USDC to the **xiAERO reward stream**.
- Each addition is paid out over the next 3 days, together with anything left of the previous stream. Because tokens can be converted in more than one batch, USDC can be added more than once after a harvest.
- **xiAERO holders** claim their USDC whenever they like. The **aXiAERO vault** collects its share and the keeper compounds it into more xiAERO.

The first funding of xiAERO is planned for the weekly harvest on Thursday 12 November 2026.

## What you need to do

- **xiAERO:** nothing until you want your USDC. Then press **Claim USDC** on **Stake & claim**.
- **aXiAERO:** nothing. Your USDC is compounded for you.

## Legacy rewards from the previous staking

The previous staking (stiAERO) paid rewards in many different tokens, per weekly epoch. Its last funding was the 5 November 2026 harvest. Those rewards stay claimable on the app's **Legacy** page:

1. Press **Visit Legacy**, then **Find my legacy rewards**.
2. Press **Find all funded reward weeks** to load every week with rewards.
3. Choose one:
   - **Claim all** or **Claim week**: receive the tokens as they are;
   - **Claim & convert to USDC**: claim, then swap the tokens received to USDC;
   - **Claim & convert to iAERO**: claim, then swap them to liquid iAERO.

To convert reward tokens that are already in your wallet, use **Sweep to USDC**, **Convert to liquid iAERO** or **Customise tokens to sweep** on the same page. You choose which tokens to convert and review the quotes before anything is swapped. Known spam tokens are hidden. For other wallet clean-ups, see [Token Sweeper](token-sweeper.md).

The Auto-USDC Vault's last reward week is due to be processed after the 12 November 2026 harvest. See [Auto-USDC Vault (legacy)](auto-usdc-vault.md).

## Contracts

| Contract | Address (Base) |
|---|---|
| RewardsHarvester | `0x77f90d2dDB15Ffe28fa322aDA351d11da3B8bFe5` |
| AeroRewardForwarder v2 | `0x5ceEF9c4D123c0D0b52311403A30cE115d973884` |
| AeroLeafRewardVault (on Ethereum, Optimism, Arbitrum, Robinhood and Ink) | `0xC6bcDf0d212341B4d6fA6Dd94cEed6431E08985B` |
| RewardConverter | `0xA397Ad9f43967b9AE7Ab54E2dDf61CF8c5585bd8` |
| SwapPolicy | `0x2F75ae9a734ddDD073E28fDe72E54991E37d28f0` |
| TreasuryDistributor | `0xD36b84EeFd1F481a737595C8212c43A9cD76C8e0` |
| xiAERO | `0x44D8172b95a208082327f58c5b42269d48a34847` |
