# Reward Distribution

> **Updated November 2026.** iAERO stakers are now paid in USDC through [xiAERO](../getting-started/what-is-xiaero.md). The split between stakers, LIQ stakers, the treasury and the peg reserve is unchanged.

## Sources of Rewards

**Aero voting rewards**, earned by iAERO's stake #1584 on every chain where it votes (Base, Ethereum, Optimism, Arbitrum, Robinhood and Ink are allowed):
- incentives (bribes) that protocols pay to the voters of their pools;
- trading fees from the pools voted for.

## Distribution Flow

The RewardsHarvester splits every weekly harvest. The percentages are fixed in the contracts and were read on-chain on 11 November 2026:

```
Weekly harvest (RewardsHarvester)
├── 80% → iAERO stakers: sold for USDC by the RewardConverter, streamed to xiAERO
├── 10% → TreasuryDistributor
│         ├── 80% of this (8% of the total) → LIQ stakers
│         └── 20% of this (2% of the total) → treasury
└── 10% → peg reserve
```

## Claiming Process

### For iAERO stakers

1. Stake iAERO as **xiAERO** (or **aXiAERO** to compound). See [Staking iAERO](../getting-started/guides/staking-iaero.md).
2. After each weekly harvest, the stakers' share is sold for USDC and streamed to xiAERO holders over 3 days.
3. Claim your USDC at any time with **Claim USDC**. aXiAERO compounds instead.

### For LIQ stakers

LIQ stakers receive their share through the LIQ Staking Distributor, in the tokens it is paid in. Claim them on the app's **LIQ** page. See [What is LIQ?](../getting-started/what-is-liq.md).

### Reward tokens

- **iAERO stakers:** USDC only.
- **LIQ stakers:** the reward tokens themselves (for example AERO, USDC, WETH and other incentive tokens).

## Rewards From Before November 2026

The previous staking (stiAERO) and the Auto-USDC Vault paid rewards differently. Their last funding was the 5 November 2026 harvest, and rewards already funded stay claimable on the app's Legacy page. See [Moving from stiAERO or the Auto-USDC Vault](../getting-started/guides/moving-from-stiaero.md).

More detail on each step: [Rewards Harvesting](../getting-started/convert-or-compound.md).
