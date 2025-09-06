# Security Audit Report: PermalockVault V5

**Contract Address:** `0x9322A2155815CD636A16e66BEF717B1B848e3248`  
**Auditor:** Independent Security Review  
**Date:** September 2025
**Network:** Base Mainnet  

## Executive Summary

The PermalockVault V5 contract implements a permalocking mechanism for AERO tokens via veAERO NFTs, with a 5% protocol fee structure and dual-token emission system (iAERO and LIQ). The audit identified several critical issues in earlier versions that have been successfully mitigated in the deployed version.

## Critical Issues Identified & Mitigated

### 1. Reentrancy Vulnerability (FIXED)
**Original Issue:** State variables updated after external calls in deposit functions  
**Risk:** Potential reentrancy attacks allowing manipulation of accounting  
**Mitigation:** State updates moved before external calls (lines 229-230, 269-270)  
**Status:** ✅ RESOLVED

### 2. Emission Rate Overflow (FIXED)
**Original Issue:** Uncapped halvings could cause emission rate to become 0 after ~256 halvings  
**Risk:** Complete failure of LIQ emission system  
**Mitigation:** Added 100-halving cap in `calculateLIQAmount` (lines 411, 417)  
**Status:** ✅ RESOLVED

### 3. Gas Griefing Attack (FIXED)
**Original Issue:** Unbounded loop in `_mergeAllNFTs` function  
**Risk:** DoS attack by adding excessive NFTs making deposits impossibly expensive  
**Mitigation:** Added MAX_MERGES limit of 10 per operation (line 551)  
**Status:** ✅ RESOLVED

### 4. NFT Transfer Vulnerability (FIXED)
**Original Issue:** `executeNFTAction` allowed arbitrary calls including NFT transfers  
**Risk:** Authorized users could steal managed NFTs  
**Mitigation:** Strict selector whitelist allowing only merge/increase operations  
**Status:** ✅ RESOLVED

### 5. Silent Failure on LIQ Cap (FIXED)
**Original Issue:** Function silently returned when LIQ cap reached  
**Risk:** Users pay fees but receive no LIQ tokens  
**Mitigation:** Added explicit revert with clear error message (line 532)  
**Status:** ✅ RESOLVED

## Medium Severity Issues

### 1. Emergency Recovery Mechanism
**Risk:** Owner could abuse rescue mechanism  
**Mitigation:** 48-hour timelock, requires pause + emergency state, predefined safe address  
**Status:** ✅ ACCEPTABLE with proper governance

### 2. Centralization Risk
**Risk:** Owner has significant control  
**Mitigation:** Ownership transferred to multisig, time-locked operations  
**Status:** ✅ ACCEPTABLE with multisig

## Security Features Implemented

1. **Reentrancy Guards:** All external functions protected
2. **Access Control:** Role-based permissions with clear separation
3. **Pause Mechanism:** Emergency pause capability for incident response
4. **Input Validation:** Comprehensive checks on all user inputs
5. **Decimal Validation:** Ensures all tokens are 18 decimals
6. **NFT Intake Guards:** Prevents accidental NFT transfers
7. **Safe Math:** Solidity 0.8.24 built-in overflow protection

## Economic Security

1. **Supply Cap Enforcement:** LIQ minting respects 100M cap with proper distribution
2. **Fee Structure:** Immutable 5% protocol fee prevents manipulation
3. **Treasury Split:** 20% of LIQ emissions to treasury ensures sustainability
4. **Halving Mechanism:** Predictable emission schedule with overflow protection

## Operational Security

### Access Control Hierarchy
- **Owner:** Contract administration (multisig)
- **Keeper:** Maintenance operations only
- **Voting Manager:** Gauge voting delegation
- **Rewards Collector:** Protocol fee claiming

### Emergency Procedures
1. Pause mechanism for immediate response
2. 48-hour timelock for NFT rescue operations
3. Sweep functions for recovering stuck tokens (excluding protocol tokens)

## Testing Recommendations

1. Implement comprehensive unit tests for all functions
2. Conduct integration testing with veAERO
3. Perform gas optimization analysis
4. Execute formal verification of critical invariants

## Post-Deployment Requirements

- [x] Grant MINTER_ROLE on iAERO to vault
- [x] Grant MINTER_ROLE on LIQ to vault
- [x] Set keeper address
- [x] Set voting manager
- [ ] Set rescue safe address
- [ ] Deposit initial veNFT to establish primary
- [ ] Set rewards collector

## Conclusion

The PermalockVault V5 contract demonstrates robust security practices with all critical vulnerabilities from earlier versions successfully mitigated. The implementation follows established patterns for DeFi protocols with appropriate access controls, emergency mechanisms, and economic safeguards. The contract is suitable for production use following completion of remaining setup steps.

**Risk Rating:** LOW (with multisig governance)  
**Recommendation:** APPROVED for mainnet deployment

---

*This audit is based on contract version deployed at block height 35146801 and does not cover any subsequent modifications.*


### Configuration Requirements

* **Vault:**

  * `setVotingManager(<VotingManager>)`
  * `setAuthorizedTarget(<Voter>, true)`
  * Vault must **own** a valid `primaryNFT`.
* **Manager:**

  * `setKeeper(<bot>, true)`
  * `setOracle(address(0), ETH/USD, …, true)`
  * `setOracle(<bribeToken>, <feed>, …, true)` for each token
  * `setAllowedBribeToken(<token>, true)` (or batch with oracles).
  * Add pools: `addPools([pool1, pool2, ...])`.

### Operational Notes

* **Refunds** are available if: epoch passed + grace, and **either** epoch wasn’t executed **or** the pool wasn’t included.
* **Treasury claims** are chunked (bounded by `MAX_CLAIM_CHUNK`=50).
* Auto‑allocation ensures **exact 10,000 bps** after min/cap and remainders.

---

## PermalockVault\_V5 — Function Table

**Purpose:** Custodies and manages veAERO NFTs, mints iAERO/LIQ on deposits, gates veAERO actions, and exposes sweep/rescue + controlled maintenance (merge/rebase). Acts as the **hub** for both Harvester and VotingManager.

### Immutables & Constants

* `AERO`, `veAERO`, `iAERO`, `LIQ`, `treasury` (all **18d**)
* Fees & limits: `PROTOCOL_FEE_BPS=500` (5%), `MIN_DEPOSIT=1e18`, `MAX_SINGLE_LOCK=10,000,000e18`
* Emissions: `baseEmissionRate` (init `1e18`), halving every `HALVING_STEP=5,000,000e18` LIQ minted

### Access Model

* **Owner:** pausing, emergency flags, role/target authorization, rescue, keeper, emissions rate (pre‑mint only).
* **Authorized:** can call `executeNFTAction`.
* **Keeper or Owner:** `performMaintenance`.
* **RewardsCollector:** permitted to sweep rewards (to **self** only).

### User / Public Functions

| Function                                            | Signature                                                                     |                     Access |                                                   Guards | Notes                                                                                                                                      |
| --------------------------------------------------- | ----------------------------------------------------------------------------- | -------------------------: | -------------------------------------------------------: | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **previewDeposit**                                  | `(uint256 aeroAmount) → (iAeroToUser, iAeroToTreasury, liqToUser)`            |                     Public |                                                     view | Validates amount within min/max.                                                                                                           |
| **previewDepositVeNFT**                             | `(uint256 tokenId) → (iAeroToUser, iAeroToTreasury, liqToUser, lockedAmount)` |                     Public |                                                     view | Reads veAERO lock (must be > 0).                                                                                                           |
| **deposit**                                         | `(uint256 amount)`                                                            |                     Public |      **nonReentrant, whenNotPaused, notEmergencyPaused** | Increases **existing** `primaryNFT` lock; mints iAERO & LIQ (with treasury split). Auto‑merge optional.                                    |
| **depositVeNFT**                                    | `(uint256 tokenId)`                                                           |                     Public |      **nonReentrant, whenNotPaused, notEmergencyPaused** | Guarded ERC‑721 intake; marks managed; choose/promote primary; optional merge; mints iAERO & LIQ.                                          |
| **performMaintenance**                              | `()`                                                                          |           **Keeper/Owner** |                                         **nonReentrant** | Merges additional → primary (max 10 per call); rebase primary to max if needed. **Not paused** by Pausable.                                |
| **executeNFTAction**                                | `(uint256 tokenId, address target, bytes data) → bytes`                       |       **Authorized/Owner** |                                         **nonReentrant** | Target must be in `authorizedTargets`. If `target==veAERO`, only `increaseAmount/increaseUnlockTime/merge` allowed; **transfers blocked**. |
| **sweepERC20**                                      | `(address[] tokens, address to) → uint256[] amounts`                          | **Owner/RewardsCollector** |                                         **nonReentrant** | Owner cannot sweep **AERO**; collector can (for rewards) but **must** sweep to self. Never sweeps `iAERO`/`LIQ`.                           |
| **sweepETH**                                        | `(address to) → uint256 amount`                                               | **Owner/RewardsCollector** |                                         **nonReentrant** | Collector must sweep to self.                                                                                                              |
| **rescueVeNFT**                                     | `(uint256 tokenId, address to)`                                               |                  **Owner** |                                         **nonReentrant** | For veAERO **not managed** by vault.                                                                                                       |
| **rescueERC721**                                    | `(address token, uint256 tokenId, address to)`                                |                  **Owner** |                                         **nonReentrant** | Non‑veAERO ERC‑721 only.                                                                                                                   |
| **setRescueSafe**                                   | `(address safe)`                                                              |                  **Owner** |                                                        — | Break‑glass destination.                                                                                                                   |
| **proposeManagedRescue**                            | `(uint256 tokenId, string reason)`                                            |                  **Owner** |                               `paused && emergencyPause` | Starts a **time‑locked** rescue plan (`RESCUE_DELAY`=48h).                                                                                 |
| **cancelManagedRescue**                             | `(uint256 tokenId)`                                                           |                  **Owner** |                                                        — | Cancels plan.                                                                                                                              |
| **executeManagedRescue**                            | `(uint256 tokenId)`                                                           |                  **Owner** | **nonReentrant**, time ≥ ETA, `paused && emergencyPause` | Transfers managed veNFT to `rescueSafe` and cleans bookkeeping.                                                                            |
| **calculateLIQAmount**                              | `(uint256 iAeroAmount) → uint256`                                             |                     Public |                                                     view | Applies halving schedule vs. `totalLIQMinted`.                                                                                             |
| **getCurrentEmissionRate**                          | `() → uint256`                                                                |                     Public |                                                     view | Current halved rate.                                                                                                                       |
| **vaultStatus**                                     | `() → rich struct`                                                            |                     Public |                                                     view | TVL, protocol share, primary NFT stats, merge/rebase hints.                                                                                |
| **getManagedNFTs**                                  | `() → uint256[]`                                                              |                     Public |                                                     view | IDs (primary + additional managed).                                                                                                        |
| **getNFTInfo**                                      | `(uint256 tokenId) → tuple`                                                   |                     Public |                                                     view | Managed flag, amounts, voting power, unlock time, primary?, permanent?                                                                     |
| **getTotalValueLocked**                             | `() → uint256`                                                                |                     Public |                                                     view | Alias of `totalAEROLocked`.                                                                                                                |
| **getProtocolShareBPS / getProtocolEffectiveShare** | `() → uint256`                                                                |                     Public |                                                     pure | Returns `PROTOCOL_FEE_BPS`.                                                                                                                |
| **receive**                                         | `()`                                                                          |                          — |                                                        — | Accepts ETH.                                                                                                                               |
| **onERC721Received**                                | `(...) → selector`                                                            |                          — |                                                        — | Validates only expected veAERO transfers initiated by `depositVeNFT`.                                                                      |

### Owner / Admin Functions

| Function                | Signature                 | Notes                                                          |
| ----------------------- | ------------------------- | -------------------------------------------------------------- |
| **setKeeper**           | `(address keeper)`        | Keeper can run maintenance.                                    |
| **setAuthorized**       | `(address account, bool)` | Grants `executeNFTAction` right.                               |
| **setAuthorizedTarget** | `(address target, bool)`  | Must include Aerodrome **Voter**.                              |
| **setVotingManager**    | `(address mgr)`           | Also sets `authorized[mgr]=true`.                              |
| **setRewardsCollector** | `(address coll)`          | Also sets `authorized[coll]=true`. **Required** for Harvester. |
| **setBaseEmissionRate** | `(uint256 rate)`          | **Only before** any LIQ minted.                                |
| **setEmergencyPause**   | `(bool)`                  | Additional kill‑switch for deposits.                           |
| **pause / unpause**     | `()`                      | Standard Pausable.                                             |

### Configuration Requirements

* **At Deploy:** Supply **18‑decimals** `AERO`, `iAERO`, `LIQ`, `veAERO`, `treasury`.
* **Before First Deposit:** If you will change emissions, call `setBaseEmissionRate(...)` (cannot change after first LIQ mint).
* **Wire Modules:**

  * `setRewardsCollector(Harvester)`
  * `setVotingManager(VotingManager)`
  * `setAuthorizedTarget(AerodromeVoter, true)`
* **Optional:** `setKeeper(<bot>)` for maintenance; `setEmergencyPause(true)` if you need break‑glass prior to operations; configure rescue safe & plans.

### Operational Notes

* **Owner cannot sweep AERO** (safety), but **RewardsCollector can** (for rewards routing).
* `performMaintenance` is **not paused** by `Pausable`—use `emergencyPause`/pause + rescue controls when necessary.
* `executeNFTAction` **blocks veAERO transfers**; only growth/extend/merge actions permitted.

---


