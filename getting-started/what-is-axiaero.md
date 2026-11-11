# What is aXiAERO?

aXiAERO ("Compounding Staked iAERO") is the auto-compounding way to hold staked iAERO. Instead of paying you USDC, it uses the USDC your share earns to buy more iAERO and stake it. The number of aXiAERO shares you hold stays the same, while the amount of [xiAERO](what-is-xiaero.md) behind each share grows.

In the app this is the **Auto-compound** option on **Stake & claim**.

## In one minute

- **A share of a pool.** The aXiAERO vault holds xiAERO plus the USDC that xiAERO has earned. Each aXiAERO share is a proportional claim on both.
- **Compounding is done for you.** The protocol's keeper swaps the vault's USDC for iAERO and stakes it as more xiAERO inside the vault.
- **Leave at any time.** Redeeming returns your share of the xiAERO, and reserves your share of any USDC not yet compounded for you to claim.
- **Joining can need a little USDC** (explained below). The app shows the exact maximum before you approve anything.
- **A normal token.** aXiAERO is an ERC-20 token on Base. If you move it, your whole share moves with it, including any USDC not yet compounded.

## How compounding works

1. The vault holds xiAERO, so it earns USDC from the xiAERO reward stream like any other holder.
2. The protocol's keeper swaps that USDC for iAERO, using only swap routes the protocol Safe has approved.
3. Each swap is checked on-chain against an iAERO/USDC reference price. It fails unless it returns at least the reference amount minus at most 2%.
4. The iAERO bought is staked straight into xiAERO and stays in the vault.

No new shares are created when the vault compounds. Each share is simply backed by more xiAERO than before.

**Today.** As of 11 November 2026, 1 aXiAERO redeems for exactly 1 xiAERO and no USDC, because no USDC has been streamed yet. The first funding of the stream is planned for the weekly harvest on Thursday 12 November 2026 (00:01 UTC).

**The reference price** is the 30-minute average price of iAERO in AERO on the Aero iAERO/AERO concentrated-liquidity pool, multiplied by Chainlink's AERO/USD price, and divided by Chainlink's USDC/USD price. If the reference is unavailable or out of date, compounding cannot run until it recovers. Redeeming never uses the price.

## Join

1. Open [app.iaero.finance](https://app.iaero.finance), connect your wallet and switch to Base.
2. On **Stake & claim**, choose **Auto-compound**.
3. Under **Deposit from**, choose **iAERO** or **Existing xiAERO**, then enter the amount.
4. Press **Review stake**. The review shows the shares you will receive at minimum and the most USDC you may need to add.
5. Approve and confirm in your wallet.

If you deposit iAERO, the app first stakes it for xiAERO and then deposits the xiAERO into the vault. These are separate transactions. If the second one does not go through, your xiAERO stays in your wallet and you can review the remaining deposit again.

**Shares you receive** = xiAERO deposited × (aXiAERO in existence ÷ xiAERO held by the vault). Today that is 1 share per xiAERO.

### Why joining can need USDC

Between compounding runs, the vault can hold USDC that the existing shares have already earned. To join on equal terms you add USDC in the same proportion:

**USDC to add** = xiAERO deposited × (USDC the vault holds for its shares ÷ xiAERO held by the vault), rounded up.

This stops a newcomer from taking a share of USDC that was earned before they joined. When the vault holds no uncompounded USDC, you add nothing. You need the USDC in your wallet when you join.

## Leave

1. Open **Withdraw & manage**.
2. Choose **aXiAERO → xiAERO** and enter the number of shares.
3. Press **Review share redemption**, then confirm in your wallet.

You receive:
- **your share of the vault's xiAERO**, in your wallet straight away; and
- **your share of the USDC not yet compounded**, reserved for you. It appears as **Reserved exit USDC**. Press **Claim exit USDC** to receive it. If part of that USDC has not reached the vault yet, the claim pays what is there and keeps the rest reserved for you.

To get iAERO, then withdraw the xiAERO: **Withdraw & manage → xiAERO → liquid iAERO**.

Redeeming works even when the vault is paused, and does not depend on any price.

## xiAERO or aXiAERO?

| | xiAERO | aXiAERO |
|---|---|---|
| What you earn | USDC, which you claim | More xiAERO behind each share |
| What you do | Claim when you like | Nothing |
| Joining | Stake iAERO | Deposit iAERO or xiAERO, plus USDC if the vault holds uncompounded USDC |
| Leaving | Withdraw iAERO 1:1 | Redeem for xiAERO (and reserved USDC), then withdraw iAERO |
| Exposure | Your iAERO amount stays fixed; your rewards are in USD | Your rewards buy iAERO at market prices, so their USD value moves with iAERO |
| If you move the token | USDC already earned stays with the sender | The whole share moves, including uncompounded USDC |
| Protocol fee | None | None (normal swap costs reduce what compounding buys) |

aXiAERO is not an ERC-4626 vault, and 1 aXiAERO is not always 1 xiAERO or 1 iAERO. Always use the app's preview, or the contract's `previewRedeem`, to see what a share is worth.

## What aXiAERO's owner can and cannot do

aXiAERO's owner is the protocol Safe, a 3-of-4 multisig (`0x1039CB48254a3150fC604d4B9ea08F66f4739D37`).

The owner **can**:
- pause new deposits and compounding. Redeeming, claiming reserved USDC and transfers stay open;
- add or remove the compounding keeper. Adding one only takes effect after a 2-day wait;
- approve swap routes in the SwapPolicy contract. A new route only takes effect after a 2-day wait; removing one is immediate;
- return other tokens that were sent to the vault by mistake.

Through the aXiAERO contract, the owner **cannot**:
- take the vault's xiAERO or USDC, or aXiAERO;
- create shares without a deposit;
- loosen the 2% loss limit on compounding, which is fixed in the contract;
- upgrade the contract. It has no upgrade mechanism.

The protocol Safe (a 3-of-4 multisig) administers the iAERO token, including who may mint and burn it. Those token-level powers sit outside aXiAERO's own rules above.

## Contracts

| Contract | Address (Base) |
|---|---|
| aXiAERO, "Compounding Staked iAERO" (CompoundingVault) | `0xa00FeB9AA79612f380D523894321F52AD34EB416` |
| xiAERO (StakedIAERO) | `0x44D8172b95a208082327f58c5b42269d48a34847` |
| SwapPolicy (approved swap routes) | `0x2F75ae9a734ddDD073E28fDe72E54991E37d28f0` |
| PairPriceOracle (iAERO/USDC reference) | `0x9b801f50AD1fC458146ec69E0010BcDB926E5705` |
| AeroIAEROChainlinkFreshTwapFeed (iAERO/USD) | `0x65B65Def72080EBc82b999D91437f6e083504e23` |

## Risks

- **Smart contract risk.** The vault and its price contracts are new. Code can have bugs.
- **Market risk.** Compounding buys iAERO at market prices. If iAERO's price falls, your position can be worth less in USD even though it holds more xiAERO.
- **Swap costs.** Each compounding swap can return up to 2% less than the reference price.
- **Price reference.** If the reference price is unavailable, compounding waits. USDC keeps building up in the vault and stays part of your share.
