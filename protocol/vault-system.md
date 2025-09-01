# Vault System Architecture

## Overview

The PermalockVault is the core contract that manages all veAERO NFT positions and user deposits. It maintains maximum voting power by keeping positions permanently locked while issuing liquid iAERO tokens to users.

## Core Components

### Deposit Management

The vault accepts two types of deposits:
- **AERO tokens**: Direct deposits that create new veAERO positions
- **veNFT transfers**: Existing veAERO positions transferred to vault management

### NFT Position Structure

**Primary NFT**
- The main veAERO position that receives all deposits and merges
- Maintained at maximum lock duration (4 years)
- Automatically rebased every ~3 months to maintain max lock
- Holds the majority of voting power

**Additional NFTs**
- Temporary holding state for newly deposited veNFTs
- Queued for merging into primary position
- Merged during maintenance operations to minimize gas costs

### Automated Management

**Rebase Operations**
- Checks if lock duration < 3.75 years (4 years - 3 months)
- Extends lock back to maximum 4 years
- Maintains optimal voting power without manual intervention

**Merge Operations**
- Combines multiple NFTs into primary position
- Reduces management overhead
- Consolidates voting power for efficiency

## Fee Structure

### Protocol Fee: 5%

Applied on all deposits:
- User deposits 100 AERO
- 95 iAERO minted to user
- 5 iAERO minted to treasury

This fee:
- Funds protocol development
- Provides treasury reserves
- Aligns long-term incentives

### No Exit Fees
- No fees for trading iAERO
- No fees for unstaking
- No additional performance fees

## Security Features

### Deposit Guards

Prevents accidental NFT transfers:
- `_expectedNftSender`: Validates sender address
- `_expectedNftId`: Confirms correct NFT
- `_expectingNft`: Guards transfer window

Only accepts NFTs during active deposit transactions.

### Access Control
- **Owner**: Protocol multisig for critical functions
- **Authorized**: Voting manager and rewards collector
- **Keeper**: Automated maintenance operations
- **Users**: Can only deposit, not withdraw

### Emergency Controls
- Pause mechanism for deposits
- Emergency pause for critical issues
- Sweep functions for stuck tokens (not user deposits)

## Vault Accounting

### Key Metrics Tracked
- `totalAEROLocked`: Sum of all AERO in vault
- `totalIAEROMinted`: Total iAERO supply issued
- `nftLockedAmount[id]`: AERO per NFT position
- `primaryNFT`: Current primary NFT ID

### Status Function

Returns comprehensive vault state:
- Total user deposits
- Protocol fees collected
- Primary NFT details
- Voting power
- Rebase/merge status

## Integration Points

### With iAERO Token
- Vault has exclusive minting rights
- Mints on deposits only
- Cannot burn or redeem

### With Voting Manager
- Executes votes through `executeNFTAction`
- Maintains vote delegation
- Claims bribes and fees

### With Rewards Harvester
- Sweeps collected rewards
- Enables reward distribution
- Maintains treasury flow

## Operational Flow

1. **User Deposits** → Vault receives AERO/veNFT
2. **Token Minting** → Issues iAERO and LIQ to user
3. **Position Management** → Creates/increases veAERO lock
4. **Maintenance** → Keeper rebases/merges positions
5. **Voting** → Manager executes optimal votes
6. **Rewards** → Harvester collects and distributes

## Risk Considerations

### No Redemption Mechanism
- Deposits are one-way
- Cannot convert iAERO back to AERO
- Relies on secondary market liquidity

### Centralization Points
- Multisig controls critical functions
- Keeper required for maintenance
- Voting strategy determined by protocol

### Smart Contract Risk
- Complex NFT interactions
- Cross-contract dependencies
- Unaudited code (initially)
