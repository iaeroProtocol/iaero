# Staking iAERO for Rewards

> **Updated November 2026.** Staking now gives you xiAERO (USDC rewards) or aXiAERO (auto-compounding). If you staked before and hold stiAERO, or used the Auto-USDC Vault, see [Moving from stiAERO or the Auto-USDC Vault](moving-from-stiaero.md).

## Why Stake iAERO?

iAERO in your wallet earns nothing. Staked iAERO earns a share of 80% of all of iAERO's voting rewards, paid in USDC. Those rewards come from the trading fees and incentives that iAERO's Aero stake earns on every chain where it votes.

You can stake in two ways:

| | **Earn USDC** (xiAERO) | **Auto-compound** (aXiAERO) |
|---|---|---|
| You receive | xiAERO, 1 for each iAERO | aXiAERO shares |
| Rewards | USDC you claim | USDC turned into more staked iAERO for you |
| Learn more | [What is xiAERO?](../what-is-xiaero.md) | [What is aXiAERO?](../what-is-axiaero.md) |

## How to Stake

### Step 1: Open Stake & claim
Go to [app.iaero.finance](https://app.iaero.finance), connect your wallet and switch to Base. **Stake & claim** is the first page.

If you don't have iAERO yet, open **Get iAERO** to buy or deposit for it.

### Step 2: Choose how you earn
Choose **Earn USDC** or **Auto-compound**.

With **Auto-compound**, choose under **Deposit from** whether to use iAERO or xiAERO you already hold.

### Step 3: Enter the amount
- Enter the amount, or press **Use available balance**.
- There is no minimum.

### Step 4: Review and confirm
1. Press **Review stake**. Check the amount, the network (Base) and, for Auto-compound, the maximum USDC you may need to add.
2. The first time, your wallet asks you to approve the token. The app asks for an unlimited approval, but only the reviewed amount is staked.
3. Press **Confirm in wallet**.

For Auto-compound from iAERO, there are two transactions: the app stakes your iAERO for xiAERO, then deposits the xiAERO into the compounding vault.

## Understanding Your Position

The **Stake & claim** page shows:
- **Your USDC rewards**: USDC you can claim now (xiAERO only);
- **USDC stream active** or **No active USDC stream**: whether USDC is being streamed right now;
- **Your positions**: your xiAERO and aXiAERO balances;
- **Reserved exit USDC**, if you have redeemed aXiAERO shares and have USDC waiting.

**Portfolio** shows all your positions and wallet balances in one place.

### No lock period
- Withdraw at any time, with no penalty and no cooldown.
- The protocol can pause new stakes, but withdrawals, claims and redemptions stay open.

## Claiming Rewards (xiAERO)

1. Press **Claim USDC** in **Your USDC rewards**.
2. Confirm in your wallet.

Your xiAERO stays staked. There is no deadline to claim.

## Withdrawing

### xiAERO → iAERO
1. Open **Withdraw & manage**.
2. Choose **xiAERO → liquid iAERO** and enter the amount (part or all).
3. Press **Review withdrawal** and confirm.

You get the same amount of iAERO. Unclaimed USDC stays claimable.

### aXiAERO → xiAERO
1. Open **Withdraw & manage**.
2. Choose **aXiAERO → xiAERO** and enter the number of shares.
3. Press **Review share redemption** and confirm.

You get your share of the vault's xiAERO. Any USDC not yet compounded is reserved for you: claim it with **Claim exit USDC**. Then withdraw the xiAERO for iAERO as above.

## Rewards Math

### How xiAERO rewards are calculated

Your USDC = (your xiAERO ÷ all xiAERO) × USDC streamed while you hold it.

**Example:**
- You hold 1,000 xiAERO out of 100,000 in total: 1%.
- 10,000 USDC is streamed over 3 days.
- You earn 100 USDC over those 3 days, if you hold your xiAERO the whole time.

Each funding is streamed over 3 days. If you stake halfway through a stream, you earn only from the second half.

### APR

APR = (USDC streamed over a year ÷ value of all staked iAERO) × 100. It changes from week to week with the rewards and with iAERO's price.

## Common Issues

**"Review stake" is disabled**
- Check that you are on Base and have iAERO (or xiAERO) in your wallet.
- If an earlier wallet request is still open, finish or dismiss it in **Previous wallet request** first.

**No rewards showing**
- USDC only accrues while a stream is active. Check the stream status on the page.
- Rewards are not backdated to before you staked.
- aXiAERO does not show claimable USDC: it is compounded for you.

**Transaction failed**
- Make sure you have a little ETH on Base for gas.
- Press **Refresh**, then review again with fresh balances.

## FAQ

**When does USDC arrive?**
After each weekly harvest (Thursday, 00:01 UTC) the stakers' share is sold for USDC and added to the stream. Each addition is paid out over the next 3 days. The first funding is planned for 12 November 2026.

**Can I lose my staked iAERO?**
Only you can withdraw your xiAERO, and the xiAERO contract gives its owner no way to withdraw staked iAERO. The protocol Safe (a 3-of-4 multisig) administers the iAERO token, including who may mint and burn it. Smart contract risk also applies.

**What's the minimum stake time?**
None. You earn for exactly the time you hold xiAERO while USDC is streaming.

**Do I earn while withdrawing?**
You earn until the moment you withdraw.
