# Smart Contracts Overview

## Core Protocol Contracts

### PermalockVault.sol
**Purpose**: Main vault managing veAERO NFTs and deposits

**Key Functions**:
- `deposit()`: Deposit AERO tokens
- `depositVeNFT()`: Deposit existing veNFTs
- `executeNFTAction()`: Execute actions on managed NFTs
- `performMaintenance()`: Rebase and merge NFTs

**Permissions**:
- Owner: Protocol Multi-sig
- Authorized: Voting manager, rewards collector
- Keeper: Maintenance operations

### iAEROToken.sol
**Purpose**: Liquid receipt token for deposited AERO

**Features**:
- ERC20 + Permit
- Minting restricted to vault
- 1:1 backing (minus fees)
- Transferable and tradeable

### LIQToken.sol
**Purpose**: Governance and incentive token

**Features**:
- 100M max supply
- Minting restricted to vault
- Burnable by holders
- ERC20 + Permit

## Contract Interactions
graph TD
    User[User]
    Vault[PermalockVault]
    iAERO[iAERO Token]
    LIQ[LIQ Token]
    SD[StakingDistributor]
    LSD[LIQ StakingDistributor]
    RH[RewardsHarvester]
    TD[TreasuryDistributor]
    VM[VotingManager]
    Treasury[Treasury]
    
    User -->|Deposit AERO| Vault
    Vault -->|Mint| iAERO
    Vault -->|Mint| LIQ
    User -->|Stake iAERO| SD
    User -->|Stake LIQ| LSD
    RH -->|80%| SD
    RH -->|10%| TD
    TD -->|80% of 10%| LSD
    TD -->|20% of 10%| Treasury
    VM -->|Execute Votes| Vault
    Vault -->|Vote| Aerodrome

## Reward Distribution

### StakingDistributor.sol
**Purpose**: Distribute rewards to iAERO stakers

**Mechanism**:
- Accumulator-based rewards
- Multi-token support (ERC20 + ETH)
- No lockup required
- Pro-rata distribution

### RewardsHarvester.sol
**Purpose**: Claim and distribute Aerodrome rewards

**Distribution**:
- 80% → iAERO stakers
- 10% → TreasuryDistributor (splits 80/20)
- 10% → Peg defense reserve

### TreasuryDistributor.sol
**Purpose**: Split treasury's share between LIQ stakers and operations

**Split** (of the 10% it receives):
- 80% → LIQ stakers (8% of total)
- 20% → Treasury operations (2% of total)

