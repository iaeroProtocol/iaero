# Depositing AERO

> **Deposits are open.** Since 5 November 2026, deposits go through the **AeroStakeDeposit v2** contract (`0xc9B2D05873a3c49a4142a04a2DCBda81aCC2570D`). The old PermalockVault V5 is retired. See [Aero Migration (22 Oct 2026)](../aero-migration.md).

Depositing AERO adds it to iAERO's Aero stakes, which are permanent locks on Aero, and gives you liquid iAERO, plus a bonus in LIQ.

## What You Get

For each deposit:
- **iAERO: 95% of the AERO you deposit.** The other 5% is minted as iAERO to the protocol treasury (the protocol fee).
- **LIQ: a bonus on top**, at the current emission rate. The rate halves every 5,000,000 LIQ minted. As of 11 November 2026 it is **0.5 LIQ per iAERO** you receive.
- The treasury also receives LIQ equal to 20% of yours. This is extra: it does not come out of your share.

**Example (11 November 2026 rates):** deposit 1,000 AERO → you receive **950 iAERO** and **475 LIQ**. The treasury receives 50 iAERO and 95 LIQ.

The app reads the live rate from the contract and shows exactly what you will receive before you confirm.

## Limits

- **Minimum:** 1 AERO per deposit.
- **Maximum:** 10,000,000 AERO per deposit.
- **Only the new Aero AERO** (`0x3f5bF3EdAc59d650D8fDf9657a71F2f904A3a4Ee`) can be deposited. If you hold the old Aerodrome AERO, the app shows an **Upgrade AERO** link. Upgrade first, then come back.

## Prerequisites

- AERO in your wallet, on Base
- A little ETH on Base for gas

## Steps

### 1. Open Deposit AERO
Go to [app.iaero.finance](https://app.iaero.finance), connect your wallet and switch to Base. Open **Get iAERO** and choose **Deposit AERO**.

### 2. Enter the amount
Enter the amount under **New AERO to deposit**, or press **Max**. The preview shows the iAERO and LIQ you will receive and the 5% protocol fee.

### 3. Review
Press **Review AERO deposit**. The review is titled "One deposit. Two tokens."

### 4. Approve
Press **Approve exact new AERO in wallet**. The app only asks you to approve the exact amount you are depositing.

### 5. Deposit
After a fresh check, press **Confirm deposit in wallet**.

If the LIQ rate changes before your deposit goes through (for example because a halving is reached), the deposit stops and the app asks you to review again. Nothing is taken from your wallet in that case.

### 6. Receive your tokens
iAERO and LIQ are minted straight to your wallet. Your AERO is now a new permanent Aero stake, held by the AeroStakeVault.

Next: [stake your iAERO](staking-iaero.md) to earn USDC.

## Important Notes

- **One-way:** AERO → iAERO cannot be reversed. To exit, sell iAERO on the market.
- **5% protocol fee**, taken as iAERO minted to the treasury.
- **iAERO is liquid straight away**: trade it, transfer it or stake it.
- **If deposits are paused,** the app says so and nothing can be sent. Nothing is taken from your wallet.

## Contract

| Contract | Address (Base) |
|---|---|
| AeroStakeDeposit v2 | `0xc9B2D05873a3c49a4142a04a2DCBda81aCC2570D` |
| AeroStakeVault (holds the stakes) | `0xA21cA21973681bB68A42B8387E86cA7c144D2de2` |
| AERO (Aero) | `0x3f5bF3EdAc59d650D8fDf9657a71F2f904A3a4Ee` |
