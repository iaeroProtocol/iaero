# Auto-USDC Vault (legacy)

> **Legacy.** The Auto-USDC Vault is being wound down. Its final reward week was processed by the 12 November 2026 harvest, and it receives nothing new. For USDC rewards today, stake in [xiAERO](what-is-xiaero.md), or choose [aXiAERO](what-is-axiaero.md) to compound. To move, follow [Moving from stiAERO or the Auto-USDC Vault](guides/moving-from-stiaero.md).

## What it was

The Auto-USDC Vault let you deposit iAERO once and receive USDC instead of many reward tokens.

- The vault staked your iAERO in the old staking (the EpochStakingDistributor).
- After each weekly epoch ended, the protocol's keeper claimed the vault's rewards and swapped them to USDC.
- Your USDC for each week was set by your share of the vault at the start of that week (Thursday 00:00 UTC).
- Because a week's rewards could only be processed once that week had ended, the vault always paid **one week late**.
- Deposits had no receipt token, and withdrawals returned iAERO 1:1.

## What changed

- The old staking received its **last funding with the 5 November 2026 harvest**. From 10 November 2026 the stakers' share of each harvest goes to the RewardConverter, which funds xiAERO in USDC.
- Because the vault pays one week late, the week funded on 5 November 2026 was **processed by the 12 November 2026 harvest**, and its USDC is claimable in the app. One small reward from that week, worth about $12 in ZRO across all holders, could not be collected yet. If it is collected later, it is added to the same week and claimed the same way.
- After that week, **the vault earns nothing new**.
- Your share of the final week was fixed at its start, so **withdrawing your shares now does not reduce it**. Claiming is separate from withdrawing: USDC for processed weeks can be claimed after you have withdrawn.
- The Auto-USDC Vault is administered by the protocol Safe, which can pause it. Its withdrawals go through the old staking, which the Safe can also pause. The intent is that withdrawals and claims stay open and no pause is planned, but we recommend moving soon.

## What to do now

1. Open [app.iaero.finance](https://app.iaero.finance) and press **Visit Legacy** (or **Legacy staking & rewards** at the bottom of the page).
2. Press **Open old AutoVault**.
3. Press **Claim old AutoVault USDC** to collect USDC from processed weeks, including the final week.
4. Enter your shares and press **Withdraw old shares**. Each share returns 1 iAERO.
5. Stake the iAERO on **Stake & claim**: **Earn USDC** for xiAERO, or **Auto-compound** for aXiAERO.

Do not make new deposits into the Auto-USDC Vault: it is no longer funded.

## Contracts

| Contract | Address (Base) |
|---|---|
| Auto-USDC Vault | `0xFE5c929677D97723dc822C86c93c7e2D1B59c774` |
| EpochStakingDistributor (the old staking it used) | `0x781A80fA817b5a146C440F03EF8643f4aca6588A` |
| USDC | `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913` |
