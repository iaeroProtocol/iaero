# What is xiAERO?

xiAERO is staked iAERO. When you stake iAERO you receive the same amount of xiAERO. While you hold xiAERO you earn a share of iAERO's protocol rewards, paid in USDC.

xiAERO replaced stiAERO as iAERO's staking token in November 2026. If you still have an old stake, see [Moving from stiAERO or the Auto-USDC Vault](guides/moving-from-stiaero.md).

## In one minute

- **1 iAERO in, 1 xiAERO out.** Withdraw at any time and get back the same amount of iAERO.
- **Rewards in USDC.** USDC is streamed to xiAERO holders over time. Claim it whenever you like.
- **No lock, no cooldown, no staking fee.**
- **A normal token.** xiAERO is an ERC-20 token on Base, so you can move it to another wallet. USDC you have already earned stays with you.
- **Prefer to compound?** Hold [aXiAERO](what-is-axiaero.md) instead. It turns the USDC into more staked iAERO for you.

## Where the USDC comes from

1. Every week (Thursday, 00:01 UTC) the protocol harvests the rewards earned by iAERO's Aero stake **#1584** on every chain it votes on.
2. 80% of each harvest is the stakers' share. It goes to the **RewardConverter**.
3. The RewardConverter sells those reward tokens for USDC and adds the USDC to xiAERO's reward stream.

Stakers only ever receive USDC. A token that cannot yet be sold at a fair price waits in the converter. The protocol Safe decides which tokens the converter handles and where each share of the harvest goes. See [Rewards Harvesting](convert-or-compound.md) for the full flow.

## How the stream works

- **Each time USDC is added, it is paid out evenly over the next 3 days.** If USDC is added while a stream is still running, what is left of the old stream and the new USDC are spread together over a fresh 3 days.
- **Every second of the stream is shared by all xiAERO holders**, in proportion to how much xiAERO each one holds at that moment.
- **You earn only while you hold xiAERO.** Rewards streamed before you staked are not backdated, and nothing accrues to you after you withdraw.
- **Between streams nothing accrues.** If 3 days pass with no new USDC, the stream stops until the next funding. The app shows this as "USDC stream active" or "No active USDC stream".

**Example.** You hold 1,000 xiAERO and 100,000 xiAERO exist in total. You hold 1%, so you earn 1% of every USDC streamed while you hold it.

**First funding.** xiAERO was first funded by the weekly harvest on **Thursday 12 November 2026**. From then on, each weekly harvest adds the stakers' share to the stream.

## Stake

1. Open [app.iaero.finance](https://app.iaero.finance), connect your wallet and switch to Base.
2. On **Stake & claim**, choose **Earn USDC**.
3. Enter the amount of iAERO to stake (or press **Use available balance**), then press **Review stake**.
4. The first time, your wallet asks you to approve iAERO. Then press **Confirm in wallet**.

xiAERO arrives in your wallet straight away. You start earning from the next second of streamed USDC.

Don't have iAERO yet? Use **Get iAERO** in the app, or see [Depositing AERO](guides/deposit-aero.md).

## Claim

Press **Claim USDC** in the **Your USDC rewards** card. Claiming leaves your xiAERO staked. There is no deadline to claim.

## Withdraw

1. Open **Withdraw & manage**.
2. Choose **xiAERO → liquid iAERO** and enter the amount.
3. Press **Review withdrawal**, then confirm in your wallet.

You get the same amount of iAERO back. Withdrawing does not claim your USDC: it stays claimable separately.

## Moving xiAERO to another wallet

- When xiAERO moves, the contract first settles both wallets. **USDC earned up to that moment stays with the sender**, who can still claim it.
- **The receiver earns from the moment the xiAERO arrives.**
- If you send xiAERO to a smart contract, that contract earns the USDC, and only that contract can claim it.
- There is no setting that sends your rewards somewhere else. Each claim pays the wallet that earned the USDC, or an address that wallet chooses when it claims.

## What xiAERO's owner can and cannot do

xiAERO's owner is the protocol Safe, a 3-of-4 multisig (`0x1039CB48254a3150fC604d4B9ea08F66f4739D37`).

The owner **can**:
- pause new stakes. Withdrawals, claims and transfers stay open while staking is paused;
- choose which contracts may add USDC to the stream (today, the RewardConverter);
- change the length of future streams, between 1 and 30 days, but only after the current stream has ended;
- return other tokens that were sent to the contract by mistake.

Through the xiAERO contract, the owner **cannot**:
- take staked iAERO, unclaimed USDC or xiAERO out of the contract;
- create xiAERO without iAERO behind it;
- upgrade the contract. It has no upgrade mechanism.

The protocol Safe (a 3-of-4 multisig) administers the iAERO token, including who may mint and burn it. Those token-level powers sit outside xiAERO's own rules above.

## xiAERO and the old stiAERO

| | xiAERO (current) | stiAERO (legacy) |
|---|---|---|
| Rewards | USDC, streamed continuously | Many different tokens, per weekly epoch |
| When you start earning | From the moment you stake | From the next weekly snapshot (Thursday 00:00 UTC) |
| Claiming | One USDC claim | Per token and per week |
| New rewards | Yes | None after the 5 November 2026 harvest |

## Contract

| Contract | Address (Base) |
|---|---|
| xiAERO, "Staked iAERO (USDC)" (StakedIAERO) | `0x44D8172b95a208082327f58c5b42269d48a34847` |
| Staking token: iAERO | `0x81034Fb34009115F215f5d5F564AAc9FfA46a1Dc` |
| Reward token: USDC | `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913` |
| RewardConverter | `0xA397Ad9f43967b9AE7Ab54E2dDf61CF8c5585bd8` |

The full list is on [Contract Addresses](../technical-documentation/contract-addresses.md).

## Risks

- **Smart contract risk.** xiAERO and the RewardConverter are new contracts. Code can have bugs.
- **Rewards vary.** The USDC you receive depends on what #1584 earns each week and on the prices the converter gets when it sells.
- **iAERO price risk.** xiAERO is always 1:1 with iAERO, so its value moves with iAERO's market price. iAERO cannot be redeemed for AERO.
