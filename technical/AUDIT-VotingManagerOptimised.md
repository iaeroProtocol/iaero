# Security Audit Report — `VotingManagerOptimised`

**Client:** iAero Protocol
**Date:** 2025-09-05
**Audited artifact:** `VotingManagerOptimised.sol` (Solidity ^0.8.24)
**Standards/Libraries:** OpenZeppelin v5 series (`Ownable`, `ReentrancyGuard`, `Pausable`, `Math`, `SafeERC20`)
**External deps:**

* `IVoter` / `IVoterTime` (Aerodrome-style voting window & epoch math)
* `IPermalockVault` (selector-allowlisted `executeNFTAction`)
* Chainlink-style `AggregatorV3Interface` price feeds

---

## 1) Executive Summary

`VotingManagerOptimised` manages epoch-based voting for a custodial veNFT (held by an external PermalockVault) and handles bribe deposits/refunds in ERC-20 and ETH with USD thresholding via oracles. Keeper roles execute votes either with explicit weights or via an auto-allocation algorithm that uses bribes + base revenue signals. Per-epoch records enable claims to treasury when a pool was actually voted; otherwise depositors may refund after a grace period. Storage is partitioned by `(pool, epochId)` and includes pruning.

**Assessment:** The contract exhibits a strong baseline: pervasive `nonReentrant`, CEI discipline, fee-on-transfer-safe ERC20 intake (balance diff), oracle validation (stale/round checks + decimals normalization), per-epoch mutexing, and explicit refund/claim windows. Observed risks are bounded and acceptable given the client’s stated risk appetite.

**Overall risk:** Low–Medium.

---

## 2) In-Scope Components

* `VotingManagerOptimised` (full contract)
* External interfaces assumed correct: `IVoter`, `IVoterTime`, `IPermalockVault`, `AggregatorV3Interface`.
* Out of scope: concrete voter/gauge implementations, concrete vault implementation, token contracts, oracle deployments.

---

## 3) Threat Model & Assumptions

* **Admin/keeper honesty:** Owner is trusted; keepers are semi-trusted (can add pools/execute votes within contract rules).
* **Vault policy:** `IPermalockVault.executeNFTAction` enforces a **per-target selector allowlist** such that only `IVoter.reset` and `IVoter.vote` are permitted for `voter`. Any deviation would be operational, not a flaw here.
* **Oracles:** USD price oracles are configured correctly, up-to-date, and correspond to the intended tokens (or ETH at `address(0)`).
* **voter/epoch schedule:** `IVoterTime` returns sensible epoch boundaries (weekly cadence).
* **Tokens:** Standard ERC-20 semantics; no malicious reentrancy from token callbacks (we use `nonReentrant` and CEI regardless).

---

## 4) Methodology

* Manual line-by-line review for access control, reentrancy, CEI, math/overflow, oracle handling, epoch arithmetic, and storage growth.
* Adversarial reasoning for bribe lifecycle (deposit → claim/refund → prune), keeper voting flows, and external call rollback behavior.
* Gas and UX notes where they intersect with safety.

---

## 5) Findings

Severity scale: **C**ritical / **H**igh / **M**edium / **L**ow / **I**nformational

### M-1 — Allocation reducer can over-shrink a pool during final canonicalization

**Location:** `_calculateOptimalAllocation()` — final “reduce to exactly 10\_000 bps” branch.
**Issue:** When total weights exceed 10\_000 bps, the reducer chooses largest donors and may skip donors that cannot give without violating `minVoteWeightBPS`. The current approach can set a donor’s allocation to `0` temporarily while searching alternative donors, which can unintentionally persist a zero allocation.
**Impact:** Minor allocation distortion vs policy intent; not a safety issue.
**Recommendation:** Use a non-mutating skip of ineligible donors and compute `take = min(excess, alloc[i] - minVoteWeightBPS)`; do not write `0` to “skip.”
**Client stance:** Acceptable.

---

### M-2 — No local voting window pre-check

**Location:** `executeVotesWithWeights`, `executeVotesAuto`.
**Issue:** The functions rely on `IVoter` to revert if outside the voting window.
**Impact:** Operational (wasted gas, noisier ops).
**Recommendation:** Add `require(inVotingWindow(), "not in voting window")` before attempting actions.
**Client stance:** Acceptable.

---

### L-1 — Epoch start underflow guard is implicit

**Location:** `_epochStart(t) = epochVoteStart(t) - 1 hours`.
**Issue:** If external schedule ever returned `≤ 1 hour`, subtraction would underflow.
**Impact:** View/function revert if upstream schedule is misconfigured.
**Recommendation:** Add `require(vs > 1 hours, "bad schedule")` defensively.
**Client stance:** Acceptable.

---

### I-1 — Operational dependency: vault allowlist must enable `reset` and `vote`

**Location:** Calls via `IPermalockVault.executeNFTAction` to `voter`.
**Issue:** If the vault’s selector allowlist is not properly seeded (or later restricted), votes will revert.
**Impact:** Operational availability only.
**Recommendation:** Codify allowlist seeding in deployment runbooks/multisig scripts; monitor events.
**Client stance:** Understood.

---

### I-2 — Pruning reorders bribe slices

**Location:** `pruneBribes` (swap-and-pop).
**Issue:** Intended; indices are not promised stable.
**Impact:** UI/indexing should not assume stable ordering.
**Recommendation:** Document for integrators.
**Client stance:** Acceptable.

---

## 6) Positive Observations

* **Reentrancy:** All state-mutating externals use `nonReentrant`; reward/refund payers use CEI and avoid external state dependencies.
* **Fee-on-transfer safety:** ERC-20 bribe intake uses `before/after` balance-diff to compute `received`.
* **Oracle hygiene:** Validates `enabled`, `feed != 0`, `answer > 0`, `answeredInRound >= roundId`, and staleness; normalizes decimals to 1e18.
* **Refund safety:** Refunds require (epoch ended + grace) AND (not executed OR executed but pool not voted). Prevents treasury double-spend collisions.
* **Emergency withdrawal policy:** Blocks ETH and any **allowed** bribe tokens from sweeping—protects refundable user funds.
* **Mutex & rollback:** `epochLock` prevents concurrent votes; `try/catch` reverts with lock cleanup.

---

## 7) Recommended Tests (High-value)

1. **Bribe lifecycle (ERC-20 & ETH):** deposit across multiple epochs; enforce per-epoch USD minimums; ensure claims are transferable only when pool voted; refunds after grace only when not voted (or not executed).
2. **Oracle decimals & staleness:** feeds with 6/8/18 decimals; stale feed → revert paths.
3. **Auto allocation edges:**

   * Zero scores → revert.
   * Sum < 100% → top-up best pool.
   * Sum > 100% → reducer keeps non-zero pools above `minVoteWeightBPS`.
   * Cap & min interplay; largest-remainder distribution respects cap.
4. **Voting window:** success inside window; revert via downstream outside window.
5. **Mutex failure:** Force a revert in the second `executeNFTAction` call and assert `epochLock` resets.

---

## 8) Compatibility & Deployment Notes

* **Solidity:** ^0.8.24 is compatible with OZ v5.
* **EVMs:** No assembly beyond event array shrinking in a view; standard opcodes only.
* **Clients (ethers v6):** ABI surface is standard; large arrays handled in views.
* **Ops:** Ensure vault allowlist grants `IVoter.reset` and `IVoter.vote` selectors for the configured `voter`. Keepers should be managed via multisig and can be rotated.

---

## 9) Conclusion

From a security perspective, `VotingManagerOptimised` is well-structured and appropriate for production given your stated risk tolerance. The identified issues are mainly **operational or allocation-policy nits** rather than exploitable vulnerabilities. If you later want to harden UX and allocation determinism, the suggested patches are straightforward and do not alter core behavior.

**Final rating:** **Low–Medium risk** (acceptable).

## VotingManagerOptimised — Function Table

**Purpose:** Bribe intake, oracle‑valued scoring, and execution of Aerodrome votes via the Vault’s veNFT. Handles **refunds** for unused bribes and **treasury collection** for used bribes.

### Core Parameters

* `minBribeUSDPerEpoch` (default `10e18`)
* `bribeDiscountBPS` (default `10_000`=100%)
* `maxPoolAllocationBPS` (default `7_000`=70%)
* `minVoteWeightBPS` (default `5`=0.05%)
* `refundGraceSeconds` (default 1 day after epoch end)

### Access Control

* **Owner:** pause/unpause, oracles, allowed bribe tokens, keeper set, global params, remove pools, emergency withdraw (restricted).
* **Keeper:** add pools, set base revenue per epoch, execute votes (auto or manual weights).

### External/Public Functions

| Function                       | Signature                                                                                                                                                                                                                     |           Access |                                   Guards | Notes                                                                                               |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------: | ---------------------------------------: | --------------------------------------------------------------------------------------------------- |
| **constructor**                | `(address vault, address voter, address treasury)`                                                                                                                                                                            |                — |                                        — | Enables ETH as bribe token by default; still need **ETH oracle**.                                   |
| **setOracle**                  | `(address token, address feed, uint48 maxStaleSec, bool enabled)`                                                                                                                                                             |        **Owner** |                                        — | For `token=address(0)` configures **ETH/USD**.                                                      |
| **batchConfigureOracles**      | `(address[] tokens, address[] feeds, uint48[] maxStale, bool[] enabled, bool alsoSetAllowed)`                                                                                                                                 |        **Owner** |                                        — | Optional `alsoSetAllowed` to set `allowedBribeTokens`.                                              |
| **batchSetAllowedBribeTokens** | `(address[] tokens, bool[] allowed)`                                                                                                                                                                                          |        **Owner** |                                        — | Controls accepted bribe tokens.                                                                     |
| **setAllowedBribeToken**       | `(address token, bool allowed)`                                                                                                                                                                                               |        **Owner** |                                        — | Single‑token allow/disallow.                                                                        |
| **getPriceUSD**                | `(address token) → uint256 1e18`                                                                                                                                                                                              |           Public |                                     view | Reverts if oracle missing/stale/bad.                                                                |
| **getOracleMeta**              | `(address token) → (feed, maxStale, enabled, decimals)`                                                                                                                                                                       |           Public |                                     view | Helper for UI/ops.                                                                                  |
| **activePoolsSlice**           | `(uint256 start, uint256 max) → address[]`                                                                                                                                                                                    |           Public |                                     view | Paginates active pools.                                                                             |
| **currentEpochId**             | `() → uint256`                                                                                                                                                                                                                |           Public |                                     view | Epoch start timestamp (aligned).                                                                    |
| **isEpochStart**               | `(uint256 ts) → bool`                                                                                                                                                                                                         |           Public |                                     view | Epoch alignment helper.                                                                             |
| **inVotingWindow**             | `() → bool`                                                                                                                                                                                                                   |           Public |                                     view | Uses Aerodrome Voter window.                                                                        |
| **depositBribe**               | `(address pool, address token, uint256 amount, uint256 epochs)`                                                                                                                                                               |           Public |          **nonReentrant, whenNotPaused** | ERC‑20 path; splits across `epochs` (1–8); each slice must meet `minBribeUSDPerEpoch`.              |
| **depositETHBribe**            | `(address pool, uint256 epochs)`                                                                                                                                                                                              |           Public | **payable, nonReentrant, whenNotPaused** | Requires ETH oracle to be configured/enabled.                                                       |
| **claimTreasuryBribes**        | `(address pool, uint256 epochId, uint256 start, uint256 maxCount)`                                                                                                                                                            |           Public |                         **nonReentrant** | Only if `epoch.executed && pool was voted`; transfers slices to Treasury. Chunked.                  |
| **refundMyBribes**             | `(address pool, uint256 epochId, uint256 start, uint256 maxCount)`                                                                                                                                                            |           Public |                         **nonReentrant** | After `epoch + WEEK + refundGraceSeconds` and if not consumed; refunds caller’s slices.             |
| **pruneBribes**                | `(address pool, uint256 epochId, uint256 maxScan)`                                                                                                                                                                            |           Public |                         **nonReentrant** | Compacts paid/refunded entries (swap‑and‑pop).                                                      |
| **executeVotesWithWeights**    | `(address[] pools, uint256[] weightsBps)`                                                                                                                                                                                     | **Keeper/Owner** |          **nonReentrant, whenNotPaused** | Validates weights (min/cap) and exact 10\_000 sum. Resets then votes via Vault.                     |
| **executeVotesAuto**           | `()`                                                                                                                                                                                                                          | **Keeper/Owner** |          **nonReentrant, whenNotPaused** | Computes proportional weights from **(discounted bribes + base revenue)** with min/cap, then votes. |
| **addPools**                   | `(address[] pools)`                                                                                                                                                                                                           | **Keeper/Owner** |                                        — | Adds active pools (checks gauge exists & `isAlive`).                                                |
| **removePool**                 | `(address pool)`                                                                                                                                                                                                              |        **Owner** |                                        — | Removes from active set.                                                                            |
| **setKeeper**                  | `(address who, bool status)`                                                                                                                                                                                                  |        **Owner** |                                        — | Grants operator rights.                                                                             |
| **setParams**                  | `(uint256 minBribeUSDPerEpoch, uint256 bribeDiscountBPS, uint256 maxPoolAllocationBPS, uint256 minVoteWeightBPS)`                                                                                                             |        **Owner** |                                        — | Caps: discount ≤ 100%; cap ≤ 100%; min within (0,100%].                                             |
| **setRefundGrace**             | `(uint256 seconds_)`                                                                                                                                                                                                          |        **Owner** |                                        — | Refund grace after epoch end.                                                                       |
| **setBaseRevenueForEpoch**     | `(address[] pools, uint256[] epochIds, uint256[] usdAmounts)`                                                                                                                                                                 | **Keeper/Owner** |                                        — | Requires `isEpochStart(epochId)`; stored in USD 1e18.                                               |
| **pause / unpause**            | `()`                                                                                                                                                                                                                          |        **Owner** |                                        — | Halts bribe deposits and voting (claims/refunds use time guards).                                   |
| **emergencyWithdraw**          | `(address token, uint256 amount)`                                                                                                                                                                                             |        **Owner** |                         **nonReentrant** | **Forbidden** for ETH and **allowed bribe tokens**. For stranded ops funds only.                    |
| **view helpers**               | `getActivePools, getPoolInfo, getBribeCount, getBribes, getBribeAt, nextUnpaidBribeIndex, canRefund, nextRefundableBribeIndex, previewClaimTotals, getOptimalAllocation, wasExecuted, canExecuteVotes, getDepositedBribesUSD` |           Public |                                     view | Pagination and state introspection.                                                                 |
| **receive**                    | `()`                                                                                                                                                                                                                          |                — |                                        — | Accepts ETH bribes.                                                                                 |


