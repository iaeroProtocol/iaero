# What is stiAERO? (legacy)

> **Legacy.** stiAERO was the receipt for iAERO's previous staking. It receives no new rewards after the 5 November 2026 harvest. iAERO staking now uses [xiAERO](what-is-xiaero.md) (USDC rewards) and [aXiAERO](what-is-axiaero.md) (auto-compounding). To move, follow [Moving from stiAERO or the Auto-USDC Vault](guides/moving-from-stiaero.md).

## What it was

Until November 2026, staking iAERO meant depositing it in the **EpochStakingDistributor**. In return you received **stiAERO**, a transferable ERC-20 receipt, one for each iAERO staked.

- Rewards were paid in the many different tokens iAERO's votes earned, bucketed by weekly epoch.
- Your share of each week was fixed by your staked balance at the start of that week (Thursday 00:00 UTC).
- You claimed each token for each week, or used the app's "Claim & convert" to swap them in one flow.
- To unstake, the matching stiAERO was burned and your iAERO returned.

## What changed

- The **5 November 2026 harvest was the last one** paid into this staking. On 10 November 2026 the stakers' share of each harvest was pointed at the RewardConverter, which funds xiAERO in USDC.
- Your old stake keeps the iAERO you put in, but **earns nothing new**.
- **Rewards already funded stay claimable** by the wallet that earned them. Withdrawing your stake does not reduce them.

## What to do now

1. Open [app.iaero.finance](https://app.iaero.finance) and press **Visit Legacy** (or **Legacy staking & rewards** at the bottom of the page).
2. Optionally claim your old rewards with **Find my legacy rewards**.
3. Withdraw your stake with **Manage previous stake** → **Withdraw old iAERO**. You need the matching stiAERO in the same wallet.
4. Stake the iAERO again on **Stake & claim**, as xiAERO or aXiAERO.

Do not stake new iAERO in the old staking: it is no longer funded.

## Contracts

| Contract | Address (Base) |
|---|---|
| stiAERO, "Staked iAERO" | `0x72C135B8eEBC57A3823f0920233e1A90FF4D683D` |
| EpochStakingDistributor | `0x781A80fA817b5a146C440F03EF8643f4aca6588A` |

The audit report for these contracts is at [Audits Epoch Staking Distributor & stiAERO](../technical/AUDIT-EpochStakingDistributor-stiAERO.md).
