# iAERO Documentation
## Permanent AERO stakes. A liquid token. Rewards in USDC.

> **Staking update (November 2026).** iAERO staking now uses [xiAERO](getting-started/what-is-xiaero.md) (claim USDC) and [aXiAERO](getting-started/what-is-axiaero.md) (auto-compounding). The old stiAERO staking received its last rewards from the 5 November 2026 harvest. The Auto-USDC Vault, which pays one week late, processed that final week with the 12 November 2026 harvest, when xiAERO also received its first funding. If you staked before, see [Moving from stiAERO or the Auto-USDC Vault](getting-started/guides/moving-from-stiaero.md).

> **Aero migration (22 Oct 2026).** Aero replaced Aerodrome on Base. iAERO's veAERO became the permanent Aero stake #1584. See [Aero Migration (22 Oct 2026)](getting-started/aero-migration.md).

iAERO is a liquid staking protocol for Aero, the exchange on Base that replaced Aerodrome Finance. Locking AERO in Aero earns voting rewards, but the AERO is locked. iAERO locks AERO permanently on your behalf and gives you **iAERO**, a liquid token you can trade, transfer or stake.

## How it works

1. **Get iAERO.** Deposit AERO, or an existing Aero stake, and receive iAERO (95% of the AERO, after a 5% protocol fee) plus a bonus in LIQ. Or buy iAERO on the market.
2. **Your AERO goes into permanent stakes.** Deposited AERO becomes part of iAERO's Aero stakes, which are permanent locks on Aero, held by the AeroStakeVault contract. The protocol does not plan to unlock them. The main stake, **#1584**, held 4,853,405.56 AERO as of 11 November 2026.
3. **The votes are automated.** A keeper votes #1584 for Aero pools on any of six chains (Base, Ethereum, Optimism, Arbitrum, Robinhood and Ink), inside limits that the protocol Safe sets on-chain.
4. **Rewards are harvested every week.** 80% goes to iAERO stakers, paid in USDC.
5. **Stake to earn.** Hold [xiAERO](getting-started/what-is-xiaero.md) to claim USDC, or [aXiAERO](getting-started/what-is-axiaero.md) to have it turned into more staked iAERO.

## Why use iAERO

| Locking AERO yourself | Holding iAERO |
|---|---|
| Your AERO is locked | iAERO is liquid: trade, transfer or stake it |
| You vote, and re-vote, yourself | Votes are planned and sent for you |
| Rewards arrive in many different tokens | Stakers are paid in USDC |
| You manage your own position | No lock and no cooldown on staked iAERO |

## Things to know

- **Deposits are one-way.** iAERO cannot be redeemed for AERO. To exit, sell iAERO on the market (for example on [Aero](https://app.aero.xyz/swap)).
- **iAERO has its own market price,** which can be below or above the price of AERO.
- **Rewards vary** with what the votes earn each week and with market prices.
- **Smart contract risk.** Like any DeFi protocol, iAERO relies on smart contracts that can have bugs.
- **Administration.** The protocol Safe, a 3-of-4 multisig, administers iAERO's contracts on Base, including the iAERO token and who may mint and burn it, and it can withdraw a stake from the AeroStakeVault. What each contract lets it do is set out on that contract's page. On Ethereum, Optimism, Arbitrum, Robinhood and Ink, rewards collect in iAERO's leaf reward vault, which is managed by the iAERO team, before they are sold and bridged to Base.

## Quick links

- [Launch the app](https://app.iaero.finance)
- [What is iAERO?](getting-started/what-is-iaero.md)
- [What is xiAERO?](getting-started/what-is-xiaero.md) and [What is aXiAERO?](getting-started/what-is-axiaero.md)
- [Key Concepts](getting-started/key-concepts-and-how-to.md)
- [Voting Strategy](protocol/voting-strategy.md)
- [Contract Addresses](technical-documentation/contract-addresses.md)

## Community

- Discord: [discord.gg/YypP6DG3](https://discord.gg/YypP6DG3)
- X: [@iaeroProtocol](https://x.com/iaeroProtocol)

*Not financial advice. Past rewards do not guarantee future rewards.*
