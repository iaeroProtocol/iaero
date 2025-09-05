# Security Audit Report: iAEROToken

**Contract Address:** `0x81034Fb34009115F215f5d5F564AAc9FfA46a1Dc`  
**Date:** September 2025
**Network:** Base Mainnet  

## Executive Summary

iAEROToken is an ERC20 receipt token representing permalocked AERO positions. The contract implements role-based access control for minting with optional burning functionality.

## Critical Issues

### None Identified

## High Severity Issues

### 1. Unrestricted burnFrom Function
**Issue:** `burnFrom` bypasses ERC20 allowance checks, allowing BURNER_ROLE to burn any user's tokens without approval  
**Risk:** Users' tokens can be burned without consent if BURNER_ROLE is compromised  
**Recommendation:** Add allowance check or remove function if not required  
```solidity
function burnFrom(address from, uint256 amount) external onlyRole(BURNER_ROLE) {
    _spendAllowance(from, msg.sender, amount); // ADD THIS
    _burn(from, amount);
}
```
**Status:** ⚠️ UNRESOLVED (function exists but BURNER_ROLE never granted)

## Medium Severity Issues

### 1. No Initial MINTER_ROLE Assignment
**Issue:** Constructor doesn't grant MINTER_ROLE to any address  
**Risk:** Requires manual setup post-deployment  
**Mitigation:** MINTER_ROLE granted to vault via multisig  
**Status:** ✅ RESOLVED post-deployment

## Low Severity Issues

### 1. Redundant Zero Checks
**Issue:** Manual zero address checks duplicate OpenZeppelin's internal validation  
**Impact:** Minor gas inefficiency  
**Status:** ℹ️ INFORMATIONAL

### 2. Missing Emergency Pause
**Issue:** No pause mechanism for emergency situations  
**Impact:** Cannot halt operations in case of exploit  
**Status:** ℹ️ ACCEPTABLE (simple token contract)

## Security Features

- **Access Control:** OpenZeppelin AccessControl with DEFAULT_ADMIN_ROLE
- **ERC20Permit:** Gasless approvals via signatures
- **Role Separation:** Distinct MINTER and BURNER roles
- **Input Validation:** Amount and address checks

## Deployment Status

- [x] Contract deployed and verified
- [x] DEFAULT_ADMIN_ROLE held by multisig
- [x] MINTER_ROLE granted to PermalockVault
- [x] BURNER_ROLE not assigned (good security practice)

## Conclusion

The iAEROToken contract is secure for production use with the current configuration. The unresolved `burnFrom` issue poses no immediate risk as BURNER_ROLE is not assigned.

**Risk Rating:** LOW  
**Recommendation:** APPROVED

---

# Security Audit Report: LIQToken

**Contract Address:** `0x7ee8964160126081cebC443a42482E95e393e6A8`  
**Date:** September 2025
**Network:** Base Mainnet  

## Executive Summary

LIQToken is a governance token with a fixed maximum supply of 100 million tokens, mintable only by authorized contracts with built-in supply cap enforcement.

## Critical Issues

### None Identified

## High Severity Issues

### None Identified

## Medium Severity Issues

### 1. Supply Cap Calculation
**Issue:** `totalSupply() + amount <= MAX_SUPPLY` could theoretically overflow with massive amounts  
**Risk:** Extremely unlikely in practice but mathematically possible  
**Recommendation:** Use `amount <= MAX_SUPPLY - totalSupply()`  
**Status:** ⚠️ LOW RISK (requires unrealistic amount values)

### 2. No Initial MINTER_ROLE Assignment  
**Issue:** Constructor doesn't grant MINTER_ROLE  
**Mitigation:** MINTER_ROLE granted to vault via multisig  
**Status:** ✅ RESOLVED post-deployment

## Low Severity Issues

### 1. No burnFrom Function
**Issue:** No delegated burning capability  
**Impact:** Design choice, not a security issue  
**Status:** ℹ️ INFORMATIONAL

### 2. Missing Emergency Pause
**Issue:** No pause mechanism  
**Impact:** Cannot halt minting in emergencies  
**Status:** ℹ️ ACCEPTABLE (immutable supply cap provides protection)

## Security Features

- **Supply Cap:** Immutable 100M token limit
- **Access Control:** Role-based minting restrictions  
- **ERC20Permit:** Signature-based approvals
- **View Helper:** `remainingMintableSupply()` for transparency

## Economic Security

1. **Hard Cap:** 100M maximum supply enforced at contract level
2. **Minting Control:** Only authorized contracts can mint
3. **Burn Capability:** Deflationary mechanism available
4. **No Admin Minting:** Even admin cannot bypass MINTER_ROLE requirement

## Deployment Status

- [x] Contract deployed and verified
- [x] DEFAULT_ADMIN_ROLE held by multisig  
- [x] MINTER_ROLE granted to PermalockVault
- [x] Supply cap functioning correctly
- [x] No unauthorized minting possible

## Conclusion

The LIQToken contract implements a secure governance token with appropriate supply constraints and access controls. The minor calculation issue poses no practical risk given realistic usage patterns.

**Risk Rating:** LOW  
**Recommendation:** APPROVED

---

*Both token contracts demonstrate security-first design with minimal attack surface. The integration with PermalockVault has been properly configured with appropriate role assignments.*
