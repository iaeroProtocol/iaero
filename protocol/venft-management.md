# veNFT Management

## Understanding veAERO NFTs

### What are veNFTs?
Voting Escrow NFTs represent locked AERO positions in Aerodrome:
- Each NFT contains locked AERO tokens
- Lock duration determines voting power
- Longer locks = more voting power
- NFTs can be transferred, merged, and managed

### NFT Properties
- **ID**: Unique identifier
- **Amount**: Locked AERO balance
- **End**: Unlock timestamp
- **isPermanent**: Max-locked status
- **Voting Power**: Calculated from amount × time remaining

## Vault NFT Operations

### Depositing veNFTs

**Prerequisites**
- NFT must not be expired
- Must contain locked AERO
- Auto-max lock must be disabled
- User must own the NFT

**Process**
1. User approves NFT transfer to vault
2. Vault receives NFT via `safeTransferFrom`
3. Validates NFT properties
4. Mints iAERO based on locked amount
5. Adds to management queue

### NFT Lifecycle in Vault
User's veNFT → Deposit → Additional Queue → Merge → Primary NFT
↓
iAERO + LIQ tokens to user

### Primary NFT Management

**Selection Criteria**
- First valid NFT becomes primary
- Highest balance preferred
- Must not be expired
- Must be owned by vault

**Maintenance Schedule**
- Rebase every ~3 months
- Merge on new deposits
- Weekly voting execution
- Continuous optimization

## Advanced NFT Functions

### Merging Operations

**Why Merge?**
- Consolidates voting power
- Reduces gas costs
- Simplifies management
- Improves capital efficiency

**Merge Rules**
- Only merge into primary
- Source NFT destroyed
- Balances combined
- Voting power consolidated

**Auto-Merge Triggers**
- New deposit received
- Maintenance called
- Gas optimization batch

### Rebase Operations

**Purpose**
Maintains maximum voting power by extending lock duration

**When Rebasing Occurs**
- Lock duration < 3.75 years
- Called by keeper
- Part of maintenance routine

**Process**

if (timeRemaining < MAX_LOCK - 3 months) {
    extendLock(MAX_LOCK_DURATION);
}
Vote Execution
Through executeNFTAction
solidityvault.executeNFTAction(
    primaryNFT,
    voterContract,
    voteCalldata
);
Enables:

Gauge voting
Bribe claiming
Fee collection
Delegation

NFT Security Model
Ownership Structure

Vault owns NFTs permanently
Users cannot withdraw NFTs
Only authorized contracts can execute actions

Protected Functions

Transfer: Disabled except deposits
Merge: Only vault-controlled NFTs
Split: Not supported
Burn: Never allowed

Emergency Procedures
Stranded NFT Recovery
For NFTs sent outside deposit flow:

Owner can rescue if not managed
Requires proof of non-management
One-time recovery option

Failed Merge Handling

Retry in next maintenance
Manual merge by owner
Skip if consistently failing

Optimization Strategies
Gas Optimization

Batch merges when possible
Combine rebase with merge
Skip unnecessary operations
Cache state reads

Voting Power Maximization

Always maintain max lock
Merge quickly after deposits
Never let positions expire
Compound all rewards

Management Efficiency

Automate via keepers
Monitor merge success
Track rebase schedule
Alert on anomalies

Integration with Aerodrome
Gauge Voting

Vote on liquidity gauges
Direct emissions
Maximize bribes
Optimize returns

Reward Claims

Collect trading fees
Harvest bribes
Claim emissions
Process rebases

Compatibility

Supports all veAERO functions
Handles permanent locks
Manages standard locks
Processes all reward types

User Considerations
Before Depositing NFTs

Check lock isn't expired
Disable auto-max lock
Verify ownership
Understand permanence

After Depositing

NFT permanently in vault
Receive iAERO tokens
Cannot reclaim NFT
Voting automated

Benefits vs Direct Holding

No manual voting needed
No rebase management
Liquid iAERO tokens
Professional optimization
Compound efficiency

Technical Specifications
Supported Operations
✅ Deposit
✅ Merge
✅ Extend lock
✅ Vote
✅ Claim rewards
❌ Withdraw
❌ Split
❌ Reduce lock
❌ Transfer out
Gas Costs (Estimated)

Deposit veNFT: ~200k gas
Merge NFTs: ~150k gas
Rebase lock: ~100k gas
Vote execution: ~300k gas

These operations are optimized and batched where possible to minimize costs.
