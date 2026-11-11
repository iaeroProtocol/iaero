# Getting Started with iAERO

## What Problem Does iAERO Solve?

Aero rewards people who lock AERO in a stake and vote with it. But locked AERO cannot be used or sold, and the votes need managing. Most people either leave their AERO unlocked and earn nothing, or lock it and give up access to it.

iAERO pools deposits into permanent, protocol-managed Aero stakes and gives you a liquid token, iAERO, in return. The protocol handles the votes; you can trade iAERO at any time, or stake it to earn USDC.

## How It Works: The 30-Second Version

1. **Get iAERO.** Deposit AERO or an Aero stake (you get 95% as iAERO, plus a LIQ bonus), or buy iAERO on the market.
2. **The AERO goes into iAERO's Aero stakes**, which are permanent locks on Aero, held by the AeroStakeVault. The protocol does not plan to unlock them.
3. **The votes are automated**, for Aero pools on any of six chains: Base, Ethereum, Optimism, Arbitrum, Robinhood and Ink.
4. **Rewards are harvested weekly.** 80% goes to stakers, paid in USDC.
5. **Stake your iAERO** as xiAERO (claim USDC) or aXiAERO (auto-compound).
6. **Trade iAERO any time** if you need liquidity.

## Your First Steps

### Prerequisites
- A wallet on Base (for example MetaMask or Rabby)
- A little ETH on Base for gas
- AERO, an Aero stake, or funds to buy iAERO

### Step 1: Connect your wallet
Go to [app.iaero.finance](https://app.iaero.finance) and connect your wallet. Switch to Base when asked.

### Step 2: Get iAERO
Open **Get iAERO** and pick a route:

- **Buy with Rift**: bring assets from another chain and arrive with iAERO on Base.
- **Deposit AERO**: deposit new AERO. Minimum 1 AERO, maximum 10,000,000 AERO per deposit. See [Depositing AERO](guides/deposit-aero.md).
- **Deposit NFT**: deposit an existing Aero stake of at least 5,000 AERO. See [Depositing an Aero stake](guides/deposit-venft.md).

You can also swap for iAERO on [Aero](https://app.aero.xyz/swap).

### Step 3: Stake your iAERO
Open **Stake & claim** and choose:

- **Earn USDC** to receive [xiAERO](what-is-xiaero.md). Claim USDC whenever you like.
- **Auto-compound** to receive [aXiAERO](what-is-axiaero.md). Your USDC is turned into more staked iAERO.

Enter the amount, press **Review stake** and confirm in your wallet. See [Staking iAERO](guides/staking-iaero.md).

## Understanding the Tokens

### iAERO: your liquid token
- **How it is made:** minted when AERO is deposited into iAERO's permanent Aero stakes; not redeemable for that AERO
- **On deposit:** 1 AERO gives 0.95 iAERO (5% protocol fee)
- **Use it to:** stake for USDC, trade, or provide liquidity
- **Supply:** minted when AERO or an Aero stake is deposited

### xiAERO: staked iAERO
- **1 iAERO staked = 1 xiAERO**, withdrawable at any time
- Earns USDC, streamed over time

### aXiAERO: compounding staked iAERO
- A share of a vault holding xiAERO and its USDC
- The USDC is used to buy more iAERO, so each share is backed by more xiAERO over time

### LIQ: bonus and governance token
- **Minted with each deposit**, at a rate that halves every 5,000,000 LIQ minted (0.5 LIQ per iAERO as of 11 November 2026)
- **Stake LIQ** to share in the 8% of rewards that goes to LIQ stakers
- **Supply:** capped at 100,000,000 LIQ

## Rewards Explained

### Where rewards come from
iAERO's votes earn the trading fees and incentives that Aero pays to voters, on every chain where #1584 votes.

### How they are split

```
Weekly rewards
├── 80% → iAERO stakers, converted to USDC (xiAERO and aXiAERO)
├── 10% → TreasuryDistributor
│         ├── 80% of this (8% of the total) → LIQ stakers
│         └── 20% of this (2% of the total) → treasury
└── 10% → peg reserve
```

### Claiming
- USDC streams to xiAERO holders over 3 days after each funding.
- Claim with **Claim USDC** at any time. There is no deadline and no fee.
- aXiAERO holders do not claim: their USDC is compounded for them.

## Common Questions

**Can I get my AERO back?**
No. Deposits are one-way. You can sell iAERO on the market at any time.

**How often are rewards distributed?**
The rewards are harvested once a week (Thursday, 00:01 UTC). The stakers' share is sold for USDC and streamed to xiAERO holders.

**Is there a lock period for staking?**
No. You can withdraw xiAERO for iAERO, or redeem aXiAERO, at any time.

**What happens to the voting power?**
It is pooled in iAERO's stakes and voted by the protocol's keeper. See [Voting Strategy](../protocol/voting-strategy.md).

## Risk Considerations

- **Smart contract risk:** the protocol relies on smart contracts, some of them new. Published audit reports are listed under Technical Documentation.
- **No redemption:** iAERO cannot be converted back to AERO.
- **Price risk:** iAERO may trade below the value of the AERO behind it.
- **Administration:** the protocol Safe, a 3-of-4 multisig, sets the voting limits and administers iAERO's contracts on Base, including the iAERO token and who may mint and burn it. It can also withdraw a stake from the vault. <!-- OWNER: leaf reward vault management wording -->

## Next Steps

1. [Stake your iAERO](guides/staking-iaero.md)
2. [Add liquidity](guides/providing-liquidity.md) to the iAERO/AERO pools on Aero
3. [Learn about LIQ](../tokenomics/liq-token.md)
