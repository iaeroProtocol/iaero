# Security Audit Report — `RewardsHarvester`

**Client:** iAero Protocol
**Date:** 2025-09-05
**Artifact:** `RewardsHarvester.sol` (Solidity 0.8.24)
**Standards/Libraries:** OpenZeppelin v5 (`Ownable`, `ReentrancyGuard`, `SafeERC20`)
**External Dependencies:**

* `IPermalockVault` (PermalockVault\_V5 with collector-aware sweepers)
* Aerodrome `IVoter` (claimBribes / claimFees / claimRewards / claimRebase)
* `IStakingDistributor` (notifyRewardAmount)
* `ITreasuryDistributor` (distribute)

---

## 1) Executive Summary

`RewardsHarvester` is a keeper/owner-gated operations contract that:

1. Claims bribes/fees/rewards via the vault-owned veNFT (using the vault’s `executeNFTAction`),
2. Sweeps reward assets **from the vault to itself**, and
3. Splits the sweep per policy: **10% protocol → TreasuryDistributor (or treasury fallback), 10% peg reserve, 80% to iAERO stakers** via `StakingDistributor`.

Following your latest deployment of **PermalockVault\_V5** with the improved sweep authorization (collector may sweep to self; owner may not sweep AERO; iAERO/LIQ always blocked), the Harvester can operate without changes on its side.

**Overall Risk:** **Low** (contract-level). Most residual risk is **operational** (correctly configuring the vault allowlists/roles and distributor addresses). The on-chain logic uses solid access control, reentrancy guards, and CEI discipline.

---

## 2) Scope

* Full review of `RewardsHarvester.sol` as provided.
* Integration assumptions with `PermalockVault_V5`:

  * Vault’s `rewardsCollector` is set to the deployed `RewardsHarvester` address.
  * Vault’s `sweepERC20/sweepETH` now authorize `rewardsCollector` to sweep **to itself**, block `iAERO`/`LIQ`, allow **AERO** only for the collector path.
  * Vault’s `executeNFTAction` allowlist includes relevant Aerodrome selectors (see §5.M-2).

Out of scope: concrete Aerodrome voter/gauge contracts, distributor implementations, token contracts.

---

## 3) Methodology

* Manual line-by-line review for access control, reentrancy, CEI, value-flow correctness, and external call surfaces.
* Reasoning through integration paths with the vault and distributors (failure modes, revert behavior).
* Consideration of fee-on-transfer tokens, ETH flows, and allowance hygiene.

---

## 4) Architecture & Roles

* **Owner** — sets keepers, voter, distributors, pegDefender, etc.
* **Keepers** — may call `claimAerodromeRewards`, `processAndDistribute`, `processAndDistributeETH`.
* **Vault (PermalockVault\_V5)** — owns veNFT; enforces selector allowlists and collector-aware sweepers.
* **Distributors** — receive protocol/peg funds; staking distributor receives 80% via `notifyRewardAmount`.

All state-changing external functions are `nonReentrant`.

---

## 5) Findings

Severity: **C**ritical / **H**igh / **M**edium / **L**ow / **I**nformational

### M-1 — Integration dependency: Vault sweep permissions & blocklist

**Where:** `processAndDistribute()`, `processAndDistributeETH()` call `vault.sweepERC20/ETH`.
**Risk:** If the vault isn’t configured exactly as deployed (collector-aware sweeps; AERO allowed for collector; iAERO/LIQ blocked), Harvester won’t receive rewards (ops failure, not an exploit).
**Status:** **Resolved by your deployed PermalockVault\_V5 patch** (collector can sweep to self; AERO allowed only for collector; iAERO/LIQ always blocked).
**Action:** Keep this invariant in runbooks; monitor sweeps with events.

### M-2 — Vault allowlist must include claim selectors

**Where:** `claimAerodromeRewards` → `executeNFTAction(voter, data)` for:

* `claimBribes(address[],address[][],uint256)`
* `claimFees(address[],address[][],uint256)`
* `claimRewards(address[])` (and optionally `claimRebase(uint256)` if used)
  **Risk:** If the vault’s allowlist is not seeded for these selectors, claims revert (ops failure).
  **Recommendation:** Ensure vault has:
  `setAuthorizedTarget(voter, true)` and `setAllowedSelector` for the above selectors.
  **Client Stance:** Accepted operational requirement.

### L-1 — External distributor calls (notify/distribute) are reentrancy surfaces (mitigated)

**Where:**

* `IStakingDistributor.notifyRewardAmount(token, amount)` (ERC-20 path uses `forceApprove` then resets to 0; ETH path uses payable call)
* `ITreasuryDistributor.distribute(token)` after transferring funds
  **Risk:** Malicious distributors could attempt reentry.
  **Mitigation:** All entry points are `nonReentrant`; allowances reset to 0 immediately after use; ETH transfers check return values.
  **Action:** None required; keep distributors simple.

### L-2 — Fee-on-transfer tokens cause split skew

**Where:** `processAndDistribute` splits by **pre-transfer balance**; if a token levies transfer fees on outbound legs, the receiving legs will be slightly off target.
**Risk:** Minor accounting drift (not security).
**Recommendation:** If precise ratios are critical, compute amounts from **post-transfer deltas** per leg; otherwise accept small skew.

### I-1 — Optional input hardening

* `setVoter`: you may require `code.length > 0` to ensure a contract: `require(_voter.code.length > 0, "voter invalid");`
* `setTreasuryDistributor`: if always a contract, also assert `code.length > 0`.

---

## 6) Positive Observations

* **Access control:** clear separation of owner vs keeper; sensitive setters are owner-only.
* **Reentrancy:** `nonReentrant` on all functions that move value; ETH sends use `.call` with success checks.
* **Allowance hygiene:** `forceApprove` then reset to `0` avoids lingering approvals.
* **Fee-on-transfer safety on intake:** not applicable (Harvester receives balances by sweep, then splits based on current balance).
* **Events:** Good coverage for claimed rewards and distribution legs (consider adding sweep events in vault if not already).

---

## 7) Tests & Runbooks (recommended)

**Unit/Integration tests**

1. **Vault integration happy path**

   * After claims, call `processAndDistribute([AERO, tokenX])`: Harvester receives sweeps; protocol/peg/staker splits match policy; allowances reset to 0.
   * ETH variant: `processAndDistributeETH()`, verify transfers and `notifyRewardAmount{value: ...}` success.
2. **Blocked tokens**

   * Ensure iAERO/LIQ never leave the vault via sweeps.
3. **Selector allowlist**

   * Remove one allowlist entry → `claimAerodromeRewards` reverts; add it → succeeds.
4. **Reentrancy guards**

   * Simulate malicious distributor attempting reentry → blocked by `nonReentrant`.
5. **Fee-on-transfer token**

   * Use a mock that burns a fee on transfer; verify minor skew is acceptable or adjust logic.

**Ops / Runbooks**

* After deploys:

  * Set `rewardsCollector = RewardsHarvester` in the vault (emit check).
  * Seed vault selector allowlist for voter claim functions.
  * Set `stakingDistributor`, `treasuryDistributor`, `pegDefender` as intended.
* Monitoring:

  * Alert on failed sweep/notify/distribute transactions.
  * Track balances stuck in the vault for tokens intended to be harvested.

---

## 8) Compatibility

* **Solidity 0.8.24** with OZ v5 — compatible.
* **Ethers v6** — ABI surface is standard.
* No assembly; straightforward to verify and instrument.

---

## 9) Conclusion

`RewardsHarvester` is **production-ready** for your split policy (10% protocol, 10% peg, 80% stakers) given the now-deployed **PermalockVault\_V5** sweep authorization and vault allowlists. We found **no exploitable vulnerabilities** in the Harvester. Remaining risks are **operational** (configuration/permissions), which you’ve addressed. With the above test and monitoring recommendations, overall risk is **Low**.

**Final Rating:** **Low Risk** (with operational dependencies documented).


## RewardsHarvesterV2 — Function Table

**Purpose:** Claims Aerodrome rewards/fees/rebases to the Vault’s veNFT and routes all incoming rewards to **Protocol / Peg / Stakers** according to fixed BPS splits, delivering staker share to your Distributor.

### Key Constants / Addresses

* `BPS = 10_000`
* `PROTOCOL_BASE_BPS = 1_000` (10%)
* `PEG_ACTION_BPS = 1_000` (10%)
* `vault` *(immutable)*, `AERO`, `iAERO` (read from vault)
* Configurable: `voter`, `stakingDistributor`, `treasuryDistributor`, `pegDefender`

### External/Public Functions

| Function                    | Signature                                                                                              |           Access |           Guards | Notes                                                                                                                                                                        |
| --------------------------- | ------------------------------------------------------------------------------------------------------ | ---------------: | ---------------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **constructor**             | `(address _vault, address _voter, address _stakingDistributor)`                                        |                — |                — | Sets immutables; reads `AERO/iAERO` from Vault.                                                                                                                              |
| **setKeeper**               | `(address who, bool allowed)`                                                                          |        **Owner** |                — | Grants keeper rights for harvesting & distribution.                                                                                                                          |
| **setVoter**                | `(address _voter)`                                                                                     |        **Owner** |                — | Must be Aerodrome **Voter** address; also add as Vault `authorizedTarget`.                                                                                                   |
| **setStakingDistributor**   | `(address dst)`                                                                                        |        **Owner** |                — | Must be a contract (`dst.code.length > 0`).                                                                                                                                  |
| **setTreasuryDistributor**  | `(address _distributor)`                                                                               |        **Owner** |                — | Optional; used to route protocol share and optionally split.                                                                                                                 |
| **setPegDefender**          | `(address _defender)`                                                                                  |        **Owner** |                — | Receiver of peg reserve share; falls back to Vault `treasury` if unset.                                                                                                      |
| **setRouter**               | `(address _router)`                                                                                    |        **Owner** |                — | Reserved for future peg actions.                                                                                                                                             |
| **setPairConfig**           | `(address _pair, bool _stable)`                                                                        |        **Owner** |                — | Reserved for future peg actions.                                                                                                                                             |
| **setBuyThresholdBps**      | `(uint256 _bps)`                                                                                       |        **Owner** |                — | Must be ≤ `BPS`. Reserved for future peg logic.                                                                                                                              |
| **claimAerodromeRewards**   | `(address[] bribes, address[][] bribeTokens, address[] fees, address[][] feeTokens, address[] gauges)` | **Keeper/Owner** | **nonReentrant** | Calls **Vault.executeNFTAction** on `voter` with the primary veNFT. Requires: Vault authorizes Harvester and `authorizedTarget(voter)=true`.                                 |
| **processAndDistribute**    | `(address[] tokens)`                                                                                   | **Keeper/Owner** | **nonReentrant** | `Vault.sweepERC20(tokens, this)`; splits: Protocol / Peg / Stakers. Calls `Distributor.notifyRewardAmount` per token; on failure falls back to Treasury/TreasuryDistributor. |
| **processAndDistributeETH** | `()`                                                                                                   | **Keeper/Owner** | **nonReentrant** | `Vault.sweepETH(this)`; same split; attempts ETH notify on Distributor via `call`.                                                                                           |
| **receive**                 | `()`                                                                                                   |                — |                — | Accepts ETH.                                                                                                                                                                 |

### Configuration Requirements

* **Vault:**

  * `setRewardsCollector(Harvester)` (and optionally `setAuthorized(Harvester, true)`)
  * `setAuthorizedTarget(Voter, true)`
* **Harvester:**

  * `setKeeper(<bot>, true)`
  * `setStakingDistributor(<Distributor>)`
  * *(Optional)* `setTreasuryDistributor(<TreasuryDistributor>)`
  * *(Optional)* `setPegDefender(<address>)`

### Operational Notes

* **Splits:** Protocol(10%) → `TreasuryDistributor` (if set) or `Vault.treasury`; Peg(10%) → `pegDefender` or `Vault.treasury`; Stakers(80%) → Distributor.
* **Safety:** Uses `forceApprove` (reset to 0 after use). If Distributor reverts, staker share is **safely rerouted** to Treasury/TreasuryDistributor.

---

