# What is iAERO?

iAERO is a liquid staking token for Aero, the exchange on Base that replaced Aerodrome Finance. iAERO is minted when AERO is deposited into iAERO's Aero stakes, which are permanent locks on Aero. iAERO cannot be redeemed for that AERO.

> **Updated November 2026.** iAERO's main Aero stake, #1584, is held by the AeroStakeVault and voted automatically. Staking iAERO now gives you [xiAERO](what-is-xiaero.md) or [aXiAERO](what-is-axiaero.md). See [Aero Migration (22 Oct 2026)](aero-migration.md) for the history.

## The Problem

To earn voting rewards on Aero you lock AERO in a stake. While it is locked, you cannot use or sell that AERO, and you have to manage the votes yourself. You choose between:
- **Liquidity**: keep your AERO free, but earn no voting rewards; or
- **Rewards**: lock your AERO, and lose access to it.

## The Solution

iAERO removes that choice:

1. **Pooling deposits.** Your AERO joins iAERO's permanent Aero stakes, held by the AeroStakeVault.
2. **Permanent lock.** The stakes are permanent locks on Aero, so they keep full voting power without being re-locked. The protocol does not plan to unlock them, but the Safe can withdraw a stake from the vault (see [Vault System](../protocol/vault-system.md)).
3. **A liquid receipt.** You receive iAERO, which you can trade, transfer or stake.
4. **Automated votes.** A keeper votes the main stake across Aero pools on several chains.
5. **USDC for stakers.** 80% of the weekly rewards go to iAERO stakers, paid in USDC.

## How It Works

### Key Components

- **iAERO**: the liquid token. You receive 95% of the AERO you deposit as iAERO; 5% is the protocol fee.
- **LIQ**: iAERO's bonus and governance token, minted with each deposit on a halving schedule. See [What is LIQ?](what-is-liq.md).
- **AeroStakeVault**: holds iAERO's Aero stakes, including the main stake **#1584**, and lets the keeper vote them only inside limits set by the protocol Safe. See [Vault System](../protocol/vault-system.md).
- **AeroStakeDeposit v2**: the deposit contract. Send it AERO or an Aero stake; it locks the AERO permanently in the vault and mints your iAERO and LIQ.
- **RewardsHarvester**: claims the rewards every week and splits them: 80% to stakers, 10% to the TreasuryDistributor, 10% to the peg reserve.
- **RewardConverter**: sells the stakers' share for USDC and funds xiAERO.
- **xiAERO and aXiAERO**: the two ways to stake iAERO. See [What is xiAERO?](what-is-xiaero.md) and [What is aXiAERO?](what-is-axiaero.md).
- **PermalockVault V5 (legacy)**: managed iAERO's veAERO on Aerodrome until the Aero migration. It is retired.

### Good to know

- iAERO cannot be redeemed for AERO. You exit by selling iAERO on the market.
- iAERO trades at its own market price, which can differ from the price of AERO.
- The protocol Safe (a 3-of-4 multisig) administers the iAERO token, including who may mint and burn it.
