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
