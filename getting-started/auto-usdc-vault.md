# Auto-USDC Vault

## Deposit iAERO once. Earn USDC every Thursday. No clicking around.

Tired of claiming 21 different reward tokens every epoch and manually swapping each one to USDC? The Auto-USDC Vault does it all for you, weekly, with one signature at the end to claim.

You deposit iAERO. The keeper handles the rest. You click claim when you want your USDC.

That's it.

---

## What it does

The Auto-USDC Vault wraps the protocol's existing iAERO staking. On every Thursday epoch boundary:

1. **Claims** every reward token your stake earned (AERO, WETH, USDbC, EIGEN, weETH, bribe tokens — everything)
2. **Converts** each one to USDC via the same battle-tested 0x router that powers the regular Rewards page
3. **Buckets** the USDC by epoch, ready for you to pull

Your iAERO stake is yours — withdraw any time, instantly, no cooldown.

---

## Core Features

🪙 **Set-and-forget** - Deposit once, USDC accrues weekly without lifting a finger

⚡ **Instant withdrawals** - No cooldown, no lockup. Get your iAERO back any time.

🔄 **Multi-tier retry** - Keeper does up to 3 sweep passes per epoch to catch every token

🛡️ **Battle-tested swap path** - Uses the same RewardSwapper + 0x v2 integration that handles rewards on the main page

🎯 **Per-batch quote refresh** - 0x quotes are refreshed seconds before each swap broadcast, not bundled up-front

📊 **Pro-rata fair distribution** - USDC split by your share at each epoch's Thursday 00:00 UTC snapshot

🔐 **Withdraw protected** - Even if admin pauses, withdrawal stays open

---

## How it works

```
You deposit iAERO  ────►  Vault stakes it in EpochStakingDistributor (instant)
                                                        │
        Every Thursday 00:00 UTC ──► keeper claims rewards ──► swaps everything to USDC
                                                        │
                                            USDC bucketed per epoch ◄────
                                                        │
                                        You click "Claim USDC" ◄──── (any time)
```

The keeper runs ~1 hour after each Thursday epoch boundary. Your USDC entitlement for an epoch = (your share at the epoch start) × (vault-wide USDC harvested that epoch) ÷ (total shares at epoch start).

---

## How To Use

### Step 1: Get iAERO

If you don't already have iAERO, lock AERO via the standard [iAERO lock flow](https://app.iaero.finance). 1 AERO → 1 iAERO, permanent lock, you get a liquid receipt.

### Step 2: Open the Auto-Vault tab

In the iAERO app, the **Auto-Vault** tab is the fifth tab (vault icon). You'll see a brief intro and your position.

### Step 3: Deposit

1. Enter the iAERO amount (or click **MAX**)
2. Click **Deposit** — first deposit asks for an iAERO approval (one signature)
3. Confirm the deposit signature
4. Your iAERO is now auto-staked. You'll see it under "Your deposit".

### Step 4: Wait for Thursday

The keeper processes the previous epoch's rewards every Thursday around 01:00 UTC. After it runs, a green banner appears at the top of the tab: **"Pending USDC ready to claim"**.

### Step 5: Claim USDC

Click **Claim USDC** in the banner. One signature, USDC arrives in your wallet.

You can claim:
- Any time after the keeper runs — there's no deadline
- Multiple epochs at once (the contract handles it in one tx)
- Even after withdrawing your iAERO principal

### Step 6 (optional): Withdraw

Click the **Withdraw** toggle, enter amount (or MAX), confirm. iAERO returns instantly. Pending USDC remains claimable separately.

---

## Reward eligibility — the snapshot rule

The underlying distributor uses a balance-at-epoch-start model. The Auto-Vault inherits this exactly:

- **Your USDC share for an epoch is based on your vault balance at that epoch's boundary (Thursday 00:00 UTC).**
- Deposit *before* Thursday 00:00 UTC → you earn for the upcoming week
- Deposit *after* Thursday 00:00 UTC → you earn starting from the *next* Thursday

There's no timing edge to chase — the snapshot decides.

### Worked example

You deposit 100 iAERO on Wednesday 23:55 UTC. 5 minutes later the epoch boundary triggers. The snapshot at that boundary includes your 100 iAERO. You earn a pro-rata share of next week's USDC.

If you'd deposited Thursday 00:05 UTC instead, you'd start earning from the *following* Thursday.

---

## The Fee

**Zero protocol fee.**

The vault charges no skim. You receive your full pro-rata share of the underlying epoch rewards, minus only the swap slippage paid to 0x's underlying DEX routes (typically 0.3% per swap, capped by the keeper's slippage logic).

---

## Auto-Vault vs. regular Stake tab

Both options earn the same underlying rewards. The difference is *what you have to do with them*:

| | Auto-Vault | Regular Stake (uses stiAERO) |
|---|---|---|
| Receive a transferable receipt token | ❌ no | ✅ stiAERO (ERC20) |
| Auto-claim rewards weekly | ✅ yes | ❌ click claim per epoch |
| Auto-convert to USDC | ✅ yes | ❌ click convert per epoch |
| Choose which reward tokens to keep raw | ❌ everything → USDC | ✅ |
| Compose with other DeFi protocols | ❌ shares aren't a token | ✅ stiAERO is ERC20 |
| Withdraw lockup | none | none |
| Underlying yield | same | same |

Pick Auto-Vault if you want to **set and forget**. Pick regular Stake if you want **per-token control** or to **use your position as collateral elsewhere**.

---

## Frequently Asked Questions

**Q: Do I need to do anything between claims?**
No. The keeper runs weekly. Just open the app and click claim when you want your USDC.

**Q: What happens if a reward token has no liquidity / can't be swapped?**
The keeper attempts up to 3 retry sweeps with progressively higher slippage tolerance. Anything genuinely unswappable stays in the vault until admin resolves it manually. Your iAERO principal is never at risk.

**Q: Can I lose my iAERO?**
No. iAERO is permanently protected by the vault contract — even the admin role cannot rescue it. Withdrawals are always 1:1.

**Q: What about my pending USDC?**
Same — USDC is protected from admin rescue. Only your own `claimUSDC` call can move it, and only your pro-rata share for each epoch you held shares in.

**Q: Do I get a receipt token (like stiAERO)?**
**No** — by deliberate design. Your position lives in the vault contract's `sharesOf[your_address]` mapping. We don't issue a transferable token because of the epoch-snapshot reward model: transferring a receipt mid-week would create ambiguous "who earns this epoch's USDC" semantics. If you need a transferable ERC20 position, use the regular **Stake** tab (gives you stiAERO).

**Q: Can I use my Auto-Vault position as collateral elsewhere?**
Not directly — no transferable token. For DeFi composability use the Stake tab (stiAERO is a standard ERC20).

**Q: Why pull-claim instead of auto-pushing USDC to my wallet?**
Pulling lets you control claim timing (tax year, gas costs). Most major DeFi vaults (Curve, Convex, Yearn v2) use the same pattern. The frontend surfaces a prominent banner so you never miss a claim.

**Q: How is APR calculated?**
Last 4 epochs of vault-wide USDC harvested, annualized, divided by current vault TVL in USD. It's an estimate — actual returns vary with Aerodrome reward levels and vault TVL.

**Q: Who runs the keeper?**
A protocol-operated bot. The keeper EOA holds the `KEEPER_ROLE` and can be revoked + replaced by the multisig at any time. The contract supports `unfinalize(epoch)` if late rewards arrive after a premature finalization.

**Q: Is this audited?**
The vault contract was through 4 internal audit rounds (7 fixes applied) plus a 50-test Foundry suite (45 unit + 5 fork tests). 256-run fuzz on the pro-rata math. All 10 protocol contracts are under multisig control. Source verified on [Basescan](https://basescan.org/address/0xFE5c929677D97723dc822C86c93c7e2D1B59c774).

**Q: Can I deposit on behalf of someone else?**
No — only `msg.sender` can deposit for themselves. Prevents griefing.

---

## Contract Reference

| Contract | Address |
|---|---|
| **Auto-USDC Vault** | [`0xFE5c929677D97723dc822C86c93c7e2D1B59c774`](https://basescan.org/address/0xFE5c929677D97723dc822C86c93c7e2D1B59c774) |
| iAERO | [`0x81034Fb34009115F215f5d5F564AAc9FfA46a1Dc`](https://basescan.org/address/0x81034Fb34009115F215f5d5F564AAc9FfA46a1Dc) |
| USDC (Base) | [`0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`](https://basescan.org/address/0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913) |
| EpochStakingDistributor | [`0x781A80fA817b5a146C440F03EF8643f4aca6588A`](https://basescan.org/address/0x781A80fA817b5a146C440F03EF8643f4aca6588A) |
| RewardSwapper (Base) | [`0x25F11F947309df89bF4D36DA5D9A9fb5F1E186c1`](https://basescan.org/address/0x25F11F947309df89bF4D36DA5D9A9fb5F1E186c1) |
| Treasury Multisig (admin) | [`0x1039CB48254a3150fC604d4B9ea08F66f4739D37`](https://basescan.org/address/0x1039CB48254a3150fC604d4B9ea08F66f4739D37) |

### Key functions

| Function | Caller | Notes |
|---|---|---|
| `deposit(uint256)` | user | Auto-stakes into upstream distributor |
| `withdraw(uint256)` | user | Always open, even when paused |
| `claimUSDC(uint256[] epochs)` | user | Up to 50 epochs per call |
| `previewUSDC(address, uint256)` | view | Per-epoch pending lookup |
| `previewUSDCMany(address, uint256[])` | view | Batch pending lookup |
| `harvest(...)` | keeper | Per-epoch claim + swap orchestration |
| `pause()` / `unpause()` | admin | Withdraw + claim remain open during pause |
| `unfinalize(uint256)` | admin | Re-open epoch if late rewards arrive |
| `rescue(address, address, uint256)` | admin | iAERO, USDC, stiAERO permanently blocked |

### Events

```solidity
event Deposited (address indexed user, uint256 amount);
event Withdrawn (address indexed user, uint256 amount);
event Harvested (uint256 indexed epoch, uint256 usdcGained, bool finalized);
event Claimed   (address indexed user, uint256 indexed epoch, uint256 usdc);
```

---

## Security Model

| Property | How it's enforced |
|---|---|
| User principal (iAERO) cannot be drained by anyone | `rescue()` explicitly blocks iAERO |
| Earned USDC cannot be drained by admin | `rescue()` explicitly blocks USDC |
| Vault's downstream receipt (stiAERO) cannot be drained | `rescue()` blocks the upstream's `receiptToken()` |
| Reentrancy | `nonReentrant` on every state-changing function; chained protection from upstream + swapper |
| Withdrawal liveness | `withdraw()` is NOT pause-gated |
| Claim liveness | `claimUSDC()` is NOT pause-gated |
| Double-claim | `claimedByUser[user][epoch]` enforces idempotency |
| Compromised keeper | Bounded to one week of unclaimed rewards via slippage; multisig revokes role instantly |
| First-depositor share manipulation | Defused by a 1-iAERO seed deposit at deploy time |

---

## The TL;DR

The Auto-Vault is the lazy man's iAERO staking. Same yield as the regular Stake tab, but you don't have to babysit it.

- **Deposit asset:** iAERO
- **Reward asset:** USDC, claimed weekly
- **Withdrawal:** Instant, no cooldown
- **Fee:** Zero protocol skim
- **Claim:** Pull model (you click)
- **Receipt token:** None (use Stake tab if you need stiAERO)

Connect → Deposit iAERO → Wait until Thursday → Claim USDC → Repeat.

---

## Links

- 🚀 [Launch the app](https://app.iaero.finance)
- 🏠 [iAERO Protocol](https://iaero.finance)
- 📖 [Verified contract](https://basescan.org/address/0xFE5c929677D97723dc822C86c93c7e2D1B59c774)
- 💬 [Discord](https://discord.gg/iaero)

---

*Disclaimer: The Auto-Vault is a smart contract on Base mainnet. Smart contracts carry inherent risk. Always verify transactions before signing. Past yields do not guarantee future returns. Not financial advice.*
