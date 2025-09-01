# Depositing veNFTs

## Overview

Users can deposit existing veAERO NFTs into the vault to receive iAERO and LIQ tokens.

## Prerequisites

- Own a veAERO NFT
- NFT must not be expired
- Disable auto-max lock if enabled

## Steps

### 1. Select Your NFT

In the "Deposit veNFT" section, select your NFT from the dropdown.

### 2. Approve & Deposit

1. Approve the vault to transfer your NFT
2. Click "Deposit veNFT"
3. Confirm the transaction

### 3. Automatic Processing

The vault will:
- Transfer your NFT to the vault
- Merge it with existing positions (if applicable)
- Issue the depositor with iAERO and LIQ tokens

## Troubleshooting

**"Wallet mutated calldata"**: Disable transaction protection in your wallet

**NFT not merging**: Likely the NFT is auto-locked.  The protocol will unlock and merge in its next maintenance period; nothing the depositor needs to do.
