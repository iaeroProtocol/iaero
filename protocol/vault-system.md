# Vault System Architecture

> **Updated November 2026.** Since 5 November 2026 iAERO's Aero stakes are held by the **AeroStakeVault**, and deposits go through **AeroStakeDeposit v2**. PermalockVault V5, which managed iAERO's veAERO on Aerodrome, is retired: see [Legacy: PermalockVault V5](#legacy-permalockvault-v5) at the end of this page.

## Overview

Two contracts on Base hold and grow iAERO's AERO:

- **AeroStakeVault** (`0xA21cA21973681bB68A42B8387E86cA7c144D2de2`) holds iAERO's Aero stakes and lets them be voted, but only inside limits the protocol Safe sets.
- **AeroStakeDeposit v2** (`0xc9B2D05873a3c49a4142a04a2DCBda81aCC2570D`) takes deposits of AERO or Aero stakes, sends them into the vault, and mints iAERO and LIQ.

The vault holds:
- **#1584**, iAERO's main stake: a permanent Aero stake of 4,853,405.56 AERO as of 11 November 2026. It moved into the vault from the protocol Safe on 5 November 2026.
- **Stakes deposited by users** through AeroStakeDeposit v2. Each one is permanent.

## AeroStakeVault

### Roles

| Role | Who | What it can do |
|---|---|---|
| **Safe** | The protocol Safe, a 3-of-4 multisig (`0x1039CB48254a3150fC604d4B9ea08F66f4739D37`) | Set the voting limits and the reward addresses, appoint the keeper and the depositor, and take a stake back into the Safe |
| **Keeper** | An automated bot run by the protocol | Vote the stakes within the limits, and claim rewards to the fixed reward addresses. It can never move a stake, change a setting or raise a limit |
| **Depositor** | AeroStakeDeposit v2 | Send users' stakes into the vault |

### What protects the stakes

- **A stake can only leave the vault to the protocol Safe.** There is no other way out, and the keeper cannot move stakes at all.
- **Rewards can only be claimed to fixed addresses**, one per chain, set by the Safe. The keeper cannot choose where rewards go.
- **No upgrades.** The vault has no upgrade path and no generic call function.

The Safe can take a stake back out of the vault, for example to vote in Aero governance, and send it back afterwards.

### Limits set on-chain by the Safe

As of 11 November 2026:

| Limit | Setting |
|---|---|
| Chains the stakes may vote on | Base, Ethereum, Optimism, Arbitrum, Robinhood, Ink |
| Share of a stake on any one chain | Anywhere from 0% to 100% on each allowed chain |
| Cap per pool | None (100%) |
| Base pools | Only live pools (an activated gauge with a non-zero emission cap) |
| Reward address on Base | AeroRewardForwarder v2 (`0x5ceEF9c4D123c0D0b52311403A30cE115d973884`) |
| Reward address on the other five chains | AeroLeafRewardVault (`0xC6bcDf0d212341B4d6fA6Dd94cEed6431E08985B`) |

The Safe can tighten or loosen these limits. The keeper cannot. See [Voting Strategy](voting-strategy.md) for how the votes are chosen.

## AeroStakeDeposit v2

- **Deposit AERO** (`depositAero`): your AERO is locked into a new permanent Aero stake, which goes straight into the vault.
- **Deposit a stake** (`depositStake`): your existing Aero stake moves straight from your wallet into the vault. If it is not yet permanent, it is made permanent.
- **Minting** follows PermalockVault V5's rules and continues V5's LIQ schedule from V5's counter: 95% of the AERO as iAERO to you, 5% as iAERO to the treasury, and a LIQ bonus to you plus 20% of that amount to the treasury.
- **Limits:** AERO deposits from 1 to 10,000,000 AERO; stake deposits from 5,000 AERO, with no maximum.
- **Checks before you sign:** the contract can list every reason a deposit would be refused, and the app shows each one with its fix.
- The contract holds nothing between transactions and has no upgrade path. It has no admin function that mints.
- The Safe can pause deposits, and can block protocol stake IDs so they can never be deposited for iAERO.

See [Depositing AERO](../getting-started/guides/deposit-aero.md) and [Depositing an Aero stake](../getting-started/guides/deposit-venft.md).

## Fee Structure

### Protocol fee: 5%, on deposits only

- You deposit 100 AERO.
- 95 iAERO is minted to you.
- 5 iAERO is minted to the treasury.

### No other fees
- No fee to stake, withdraw, claim or compound.
- No fee to trade iAERO, beyond the market's own trading fees.

## Risk Considerations

### No redemption
- Deposits are one-way. iAERO cannot be converted back to AERO.
- You exit by selling iAERO, which depends on market liquidity.

### Administration
- The protocol Safe sets the voting limits and reward addresses, can pause deposits, and can take a stake back into the Safe.
- The keeper decides the votes, within those limits.

### Smart contract risk
- The vault and the deposit contract interact with Aero's contracts on several chains.

## Legacy: PermalockVault V5

PermalockVault V5 (`0x180DAB53968e599Dd43CF431E27CB01AA5C37909`) was iAERO's vault on Aerodrome. It held veAERO NFTs, kept a primary NFT at the maximum 4-year lock with regular re-locks, and merged deposited NFTs into it.

- Its deposits were paused for the Aero migration, and it is now retired.
- On 22 October 2026 the protocol's veAERO position was moved out of V5 and converted 1:1 into the permanent Aero stake #1584.
- V5's counters (`totalAEROLocked`, `totalIAEROMinted` and so on) were not reset, so they no longer show where the protocol's stake is held.
- AeroStakeDeposit v2 continues V5's LIQ halving schedule from V5's final count.

V5's audit report is at [Audit PermaLock Vault](../technical/AUDIT-PermaLockVault_V5.md).
