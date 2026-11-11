# Troubleshooting

## Staking (xiAERO and aXiAERO)

### No USDC is accruing

**Problem**: you staked, but **Your USDC rewards** does not go up.

**Solution**:
- Check the stream status on **Stake & claim**. USDC only accrues while it shows **USDC stream active**. Each funding is streamed over 3 days after the weekly harvest (Thursday, 00:01 UTC).
- The first funding of xiAERO is planned for the harvest on 12 November 2026.
- Rewards are not backdated: you earn only from the moment you stake.
- If you hold **aXiAERO**, there is nothing to claim: your USDC is compounded into more xiAERO for you.

### "Review stake" is disabled or the transaction fails

**Solution**:
- Switch your wallet to Base.
- Make sure you have iAERO (or xiAERO) in the wallet, and a little ETH for gas.
- For **Auto-compound**, check that you have the USDC contribution the review asks for.
- If **Previous wallet request** is shown, finish or cancel that request in your wallet, then press **Request finished or cancelled** or **Check confirmation**.
- Press **Refresh** and review again with fresh balances.

### Staking is paused

New stakes can be paused by the protocol. Withdrawing, claiming USDC, redeeming aXiAERO and claiming reserved exit USDC stay open.

### I redeemed aXiAERO but did not get all my USDC

Redeeming returns xiAERO straight away and reserves your share of uncompounded USDC. Press **Claim exit USDC** to receive it. If part of it has not yet reached the vault, the claim pays what is there and keeps the rest reserved for you.

## Previous staking (stiAERO) and the Auto-USDC Vault

### My old stake or old rewards are not on the staking page

Old positions and rewards live on the **Legacy** page. Press **Visit Legacy**, or **Legacy staking & rewards** at the bottom of any page, and connect the wallet you used before. See [Moving from stiAERO or the Auto-USDC Vault](../getting-started/guides/moving-from-stiaero.md).

### I can't withdraw all of my old stake

You can only withdraw as much old stake as the **stiAERO** you hold in that wallet. If you moved stiAERO elsewhere, bring it back first.

### Some old rewards show as unavailable

A token may be awaiting processing, or have no current price or swap route. Unavailable amounts stay unclaimed; they do not become zero. Try again later.

### My Auto-USDC Vault USDC for the last week is missing

The vault pays one week late. Its final week, funded by the 5 November 2026 harvest, is due to be processed after the 12 November 2026 harvest. Once that processing has run, claim it with **Claim old AutoVault USDC**.

## Deposits

### "Upgrade your AERO first"

Your wallet holds the old Aerodrome AERO. Only the new Aero AERO (`0x3f5bF3EdAc59d650D8fDf9657a71F2f904A3a4Ee`) can be deposited. Use the **Upgrade AERO** link in the app, then come back.

### My Aero stake is refused

The app lists each reason and its fix. The most common:
- the stake holds less than 5,000 AERO;
- it has expired;
- it has votes on a chain the iAERO vault cannot manage;
- it has an operator set on Base.

See [Depositing an Aero stake](../getting-started/guides/deposit-venft.md#troubleshooting).

### Old Aerodrome veAERO NFTs

They cannot be deposited. PermalockVault V5, which accepted them, is retired. Move your position to Aero first.

### "Wallet mutated or stripped calldata"

**Solution**:
- Turn off your wallet's transaction simulation or protection feature for this transaction
- Try MetaMask or Rabby
- For a multisig, use the Safe Transaction Builder

## Approvals

**Problem**: an approval fails.

**Solution**:
- Some tokens need the allowance reset to 0 before a new approval
- Check you have enough ETH on Base for gas
- Try again with a fresh review

## Getting Help

- Discord: [Join our community](https://discord.gg/YypP6DG3)
- X: [@iaeroProtocol](https://x.com/iaeroProtocol)
