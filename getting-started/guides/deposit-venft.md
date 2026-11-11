# Depositing an Aero Stake (NFT)

> **Deposits are open.** Since 5 November 2026, you can deposit an existing **Aero stake** (an NFT on Aero's VotingEscrow, the successor to veAERO) through the **AeroStakeDeposit v2** contract (`0xc9B2D05873a3c49a4142a04a2DCBda81aCC2570D`). Old **Aerodrome veAERO NFTs cannot be deposited**: PermalockVault V5, which took them, is retired. Move your position to Aero first. See [Aero Migration (22 Oct 2026)](../aero-migration.md).

## Overview

You can turn an Aero stake you already own into liquid iAERO. Your whole stake moves into iAERO's AeroStakeVault and becomes permanent. You receive iAERO and a LIQ bonus, worked out on the stake's full amount, exactly as for an [AERO deposit](deposit-aero.md):
- **iAERO: 95%** of the AERO in the stake (5% is the protocol fee, minted as iAERO to the treasury);
- **LIQ: a bonus** at the current emission rate (0.5 LIQ per iAERO received, as of 11 November 2026).

**Example:** a stake holding 10,000 AERO → 9,500 iAERO and, at the 11 November 2026 rate, 4,750 LIQ.

## Requirements

Your stake must:
- **belong to the wallet you deposit from;**
- **hold at least 5,000 AERO.** Smaller stakes are refused: add AERO to the stake in Aero, merge it with another of your stakes, or deposit the AERO directly instead;
- **not have expired.** An expired stake cannot be made permanent. Withdraw its AERO in Aero, then [deposit the AERO](deposit-aero.md);
- **only have votes on chains the iAERO vault can manage**: today Base, Ethereum, Optimism, Arbitrum, Robinhood and Ink. Remove votes on any other chain first, and wait for the AERO to return to Base;
- **have no operator set on Base.** An operator could claim the stake's rewards after you deposit. Remove it first.

A stake that is not yet permanent is upgraded to permanent as part of the deposit.

**Claim your stake's rewards in Aero before you deposit.** After the deposit, only the iAERO vault can claim them, and they go to iAERO stakers instead of you.

## Steps

### 1. Open Deposit NFT
Go to [app.iaero.finance](https://app.iaero.finance), connect your wallet and switch to Base. Open **Get iAERO** and choose **Deposit NFT**.

### 2. Choose your stake
Pick a stake from **Your stakes**, or type its number under **Stake number**. The app checks the stake on-chain and shows what you would receive.

If the stake does not meet a requirement, the app tells you exactly what to change and how. Nothing is sent until every check passes.

### 3. Review
Press **Review stake deposit**.

### 4. Approve the stake
Press **Approve stake #… in wallet**. The app asks you to approve the deposit contract for that one stake only, not for all your NFTs.

### 5. Deposit
After a fresh check, press **Confirm deposit in wallet**. The stake moves straight from your wallet into the AeroStakeVault, and iAERO and LIQ are minted to your wallet.

## Important Notes

- **One-way:** a deposited stake cannot be returned. To exit, sell iAERO on the market.
- **Your whole stake is deposited.** There are no partial stake deposits.
- **iAERO's stake #1584 itself** can never be deposited.
- Deposited stakes are held in the AeroStakeVault as permanent locks, alongside #1584. The protocol does not plan to unlock them, but the protocol Safe can withdraw a stake from the vault (see [Vault System](../../protocol/vault-system.md)).

## Troubleshooting

The app lists every reason a stake would be refused, with the fix. The most common:

| Message | What to do |
|---|---|
| Stake holds less than 5,000 AERO | Add AERO to it in Aero, merge it with another of your stakes, or deposit AERO directly |
| Stake has expired | Withdraw its AERO in Aero, then deposit the AERO |
| Votes on a chain the iAERO vault cannot manage | Remove all of the stake's votes on that chain, wait for the AERO to return to Base, then deposit again |
| Operator set on Base | Remove the operator in Aero, then deposit again |
| Voting record does not match the stake | Add some AERO to the stake in Aero so Aero books it again, then deposit again |
| Stake belongs to another wallet | Connect the wallet that owns it |
