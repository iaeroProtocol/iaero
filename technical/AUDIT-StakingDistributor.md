# Security Audit Report — `StakingDistributor`

**Client:** iAero Protocol
**Date:** 2025-09-05
**Contract audited:** `StakingDistributor` (Solidity ^0.8.24)
**Frameworks/Libraries:** OpenZeppelin v5 (`Ownable`, `ReentrancyGuard`, `SafeERC20`, `Math`)
**Ecosystem dependencies:**

* `RewardsHarvester` → calls `notifyRewardAmount` for ERC-20 and ETH rewards
* Front-end hooks (`useStaking`) → call `stake`, `unstake`, `exit`, `claimRewards`, `claimReward`
* Keeper → monitors `totalStaked`, `rewardTokensLength`

---

## 1. Scope of Changes

The baseline design (stake iAERO, stream rewards with per-share accumulators) was kept. The following **hardening changes** were introduced:

1. **Funder allowlist**

   * Added `allowedFunders` mapping and `setAllowedFunder` (owner-only).
   * `notifyRewardAmount` now requires caller to be in the allowlist or owner.
   * Purpose: prevent arbitrary users from registering malicious reward tokens (DoS or spam vector).

2. **Balance-delta accounting in `notifyRewardAmount`**

   * For ERC-20 tokens, contract now measures `balanceOf(this)` before and after `transferFrom`.
   * Actual `received` amount is accrued to the accumulator, not the raw `amount`.
   * Purpose: ensures fee-on-transfer tokens cannot underfund the distributor and cause payout failures.

3. **Reward token cap**

   * Enforced `MAX_REWARD_TOKENS = 64`.
   * Purpose: prevent unbounded growth of `_rewardTokens` and gas-based DoS on staking/claim loops.

4. **Removed redundant reward debt write in `claimReward`**

   * `_harvestOne` already updates `rewardDebt[user][token]`; no need to call `_writeOneRewardDebt`.

5. **Documentation & clarity**

   * Comments clarified ETH sentinel handling, funder policy, and queued reward flushing semantics.

---

## 2. Findings & Risk Analysis

### \[M] Open reward-token registration (fixed)

* **Issue (before):** Any user could call `notifyRewardAmount` with a malicious token, spamming `_rewardTokens` and breaking `claimRewards`.
* **Fix:** Added `allowedFunders` gating. Only trusted addresses (Harvester, owner, etc.) may notify.

### \[M] Fee-on-transfer ERC-20s (fixed)

* **Issue (before):** Accrued `amount` even if fewer tokens arrived due to transfer fees. Future claims could revert due to underfunding.
* **Fix:** Balance-delta accounting ensures only tokens actually received are distributed.

### \[L] Unbounded reward token list (mitigated)

* **Issue (before):** Arbitrary growth of `_rewardTokens` array could lead to gas blowups.
* **Fix:** Capped at 64 tokens.

### \[I] Redundant state writes (improved)

* **Removed** extra `rewardDebt` update in `claimReward` (safe to drop).

### \[I] Queued reward flushing semantics (unchanged)

* Behavior: the first staker does not instantly receive queued rewards; they accrue as pending for the next harvest. This is intentional and documented.

---

## 3. Security Posture After Changes

* **Reentrancy:** All external functions that move value are `nonReentrant`; payouts use CEI and SafeERC20. ✅
* **Access control:** `notifyRewardAmount` now hardened with an allowlist. Staking/unstaking remain permissionless. ✅
* **Overflow:** Uses Solidity ^0.8 checked math and `Math.mulDiv` for accumulators. ✅
* **ETH handling:** Payouts use `.call` and revert on failure; `receive()` is present for plain ETH. ✅
* **Gas safety:** Reward token cap prevents unbounded loops. ✅
* **Compatibility:**

  * **Front-end:** All ABI functions used (`stake`, `unstake`, `exit`, `claimRewards`, `claimReward`, views) remain unchanged.
  * **Harvester:** Still calls `notifyRewardAmount(address,uint256)` (ERC-20 pull) and `notifyRewardAmount(address(0),amount)` with `msg.value` for ETH. Works without modification.
  * **Keeper:** Continues to read `totalStaked`, `rewardTokensLength`. ABI stable.

---

## 4. Recommendations

* **Ops runbook:** After deploy, owner must call `setAllowedFunder(<RewardsHarvester>, true)` so Harvester can notify rewards.
* **Monitoring:** Track `RewardNotified` events for each epoch; alert if `received=0`.
* **Token policy:** Prefer to only allow reward tokens you support (AERO, ETH, USDC, LIQ, etc.), to avoid surprises with exotic ERC-20s. This can be layered by adding an `allowedRewardToken` map if desired.

---

## 5. Conclusion

The updated `StakingDistributor` is **production-ready**. The two key vulnerabilities (open reward registration and fee-on-transfer underfunding) were fixed. Gas safety and clarity were improved. No breaking changes were introduced for your Harvester, Keeper, or front-end.

**Overall Risk Rating:**

* **Before changes:** Medium (due to griefing/underfunding vectors).
* **After changes:** Low.

The system is now robust against common attack surfaces while retaining full compatibility.
