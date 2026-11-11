# Moving from stiAERO or the Auto-USDC Vault

iAERO staking moved to a new system in November 2026. If you staked iAERO before (you hold **stiAERO**) or deposited into the **Auto-USDC Vault**, this guide shows you how to move to the new staking and keep earning.

## What changed

- **The 5 November 2026 harvest was the last one paid into the old staking.** On 10 November 2026 the stakers' share of each harvest was pointed at the new RewardConverter, which pays stakers in USDC through [xiAERO](../what-is-xiaero.md).
- **The first funding of the new staking is planned for the weekly harvest on Thursday 12 November 2026 (00:01 UTC).**
- **Old stakes earn nothing new.** Your stiAERO stake and your Auto-USDC Vault shares do not receive any rewards beyond those already funded.
- **Nothing moves automatically.** You withdraw from the old position and stake in the new one yourself. Each step needs your confirmation.
- **Your old rewards stay yours.** Rewards already funded stay claimable by the wallet that earned them. You do not have to claim them before you withdraw.

**The Auto-USDC Vault pays one week late.** Its last reward week, funded by the 5 November 2026 harvest, is due to be processed after the 12 November 2026 harvest. Once processed, that USDC is claimable in the Legacy section. After that the vault receives nothing new.

**Withdrawing now does not cost you rewards already funded.** Each old reward week was split by balances at the start of that week (Thursday 00:00 UTC). Withdrawing your stake or vault shares does not reduce your share of weeks that have already started.

## Before you start

- Use the wallet that holds your old stake or Auto-USDC Vault shares.
- Be on Base, with a little ETH for gas.
- For an old stake: keep your **stiAERO** in that wallet. You can only withdraw as much old stake as the stiAERO you hold. If you moved stiAERO to another wallet, bring it back first.

## Step 1: open Legacy

1. Go to [app.iaero.finance](https://app.iaero.finance) and connect your wallet.
2. Press **Visit Legacy** in the banner "Looking for your old staking rewards?". You can also use **Legacy staking & rewards** at the bottom of any page.

The Legacy page has three cards: **Find & claim old rewards**, **Withdraw your old stake** and **Manage your old AutoVault**.

## Step 2 (optional): claim your old rewards

You can do this before or after you withdraw.

1. Press **Find my legacy rewards**.
2. Press **Find all funded reward weeks** to check every funded week, or pick a week.
3. Choose how to receive them:
   - **Claim all** or **Claim week** sends you the reward tokens as they are;
   - **Claim & convert to USDC** or **Claim & convert to iAERO** claims and swaps them in one flow.

Some tokens may show as unavailable, for example if they have no price or swap route right now. Unavailable amounts stay unclaimed; they do not become zero.

## Step 3: withdraw your old stake (stiAERO holders)

1. On the Legacy page, press **Manage previous stake**.
2. Enter the amount (part or all of it).
3. Press **Withdraw old iAERO** and confirm in your wallet.

Your stiAERO is burned and the same amount of iAERO comes back to your wallet.

## Step 4: withdraw from the Auto-USDC Vault (vault depositors)

1. On the Legacy page, press **Open old AutoVault**.
2. Press **Claim old AutoVault USDC** to collect USDC from processed weeks.
3. Enter the number of shares and press **Withdraw old shares**. Each share returns 1 iAERO.

You can still claim USDC for processed weeks after you withdraw, including the final week once it has been processed.

## Step 5: stake in the new pool

1. Press **Go to new staking** (or open **Stake & claim**).
2. Choose how you want to earn:
   - **Earn USDC** gives you [xiAERO](../what-is-xiaero.md). You claim USDC whenever you like.
   - **Auto-compound** gives you [aXiAERO](../what-is-axiaero.md). Your USDC is turned into more staked iAERO for you. Joining may need a small amount of USDC.
3. Enter the amount, press **Review stake**, and confirm in your wallet.

Withdrawing alone does not move your stake. You only earn again once Step 5 is done. Rewards streamed before you stake are not backdated.

## Questions

**Do I have to move?**
No. Your old stake is still yours and you can withdraw it later. It simply earns nothing new.

**Is there a deadline?**
The old contracts have no deadline for withdrawing, or for claiming rewards that were already funded. The sooner you stake in the new pool, the sooner you start earning again.

**Why can't I see my old rewards on the new staking page?**
The new staking pays USDC to xiAERO holders. Old rewards were recorded per token and per week by the old contracts, so they live on the Legacy page.

**Can I still deposit into the old staking or the Auto-USDC Vault?**
Don't. The app no longer offers it, and nothing new is paid there.

## Legacy contracts

| Contract | Address (Base) |
|---|---|
| stiAERO (old staking receipt) | `0x72C135B8eEBC57A3823f0920233e1A90FF4D683D` |
| EpochStakingDistributor (old staking) | `0x781A80fA817b5a146C440F03EF8643f4aca6588A` |
| Auto-USDC Vault | `0xFE5c929677D97723dc822C86c93c7e2D1B59c774` |

See also [What is stiAERO? (legacy)](../what-is-stiaero.md) and [Auto-USDC Vault (legacy)](../auto-usdc-vault.md).
