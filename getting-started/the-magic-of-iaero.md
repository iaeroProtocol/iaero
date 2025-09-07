# Why the “Rake” Favors Long‑Term Stakers (Math & Examples)

Users sometimes ask why the protocol takes a “rake” (deposit fee + weekly splits) and whether that’s extractive.
This section shows, with formulas and concrete examples, why the design **shifts value from short‑term sellers to long‑term aligned users**, so that people who **stake iAERO** *and* **stake LIQ** can, in many scenarios, **earn more than 100% of a plain veAERO baseline**.

---

## The Flows (What Happens to Every Dollar of Rewards)

Let **R** be the total weekly rewards the vault harvests from the veAERO strategy (bribes/fees/gauge rewards), normalized to a common unit (e.g. USD or AERO):

* **80% → iAERO stakers** (via the Staking Distributor).
* **10% → TreasuryDistributor**
  → **80% of this (8% of R) → LIQ stakers**
  → **20% of this (2% of R) → Protocol operations**.
* **10% → Peg Defense Reserve** (used only when iAERO trades **below \$0.85** to defend the peg and become buyer of last resort).

Additionally, on **deposit**, the protocol mints **5% iAERO** to the treasury (the depositor gets 95%). That 5% is **protocol‑owned iAERO** (POI) which we typically **stake**. As sellers push price below \$0.85, peg defense buys **more** iAERO at a discount, increasing POI.

**Policy:** The protocol routes **80% of the protocol’s own iAERO staking rewards** to **LIQ stakers** (on top of the 8% base TreasuryDistributor flow).

> Bottom line: **Sellers fund the protocol’s iAERO stack (via deposit fee + cheap peg buys)** → the protocol **stakes** that iAERO → **80%** of that **staking income** is **re‑routed to LIQ stakers**.
> If you are *both* an iAERO staker and a LIQ staker, you capture value in both streams.

---

## Notation (What Variables Mean)

* **R** – total weekly harvested rewards (normalized).
* **x** – your share of the **iAERO staking pool** (0–1).
* **y** – your share of the **LIQ staking pool** (0–1).
* **P** – the **protocol’s share of the iAERO staking pool** (0–1).
  (Grows from the 5% mint on deposits and from peg defense buys below \$0.85.)
* **Price threshold:** Peg defense *only buys* when iAERO **< \$0.85**.

---

## Your Weekly Income (Formula)

Your income has two legs:

1. **iAERO staking leg (80% of R):**
   You receive your pro‑rata share:

   $$
   I_{\text{iAERO}} = x \cdot 0.80 \cdot R
   $$

2. **LIQ staking leg:** It receives:

   * **Base 8%** of R (from TreasuryDistributor), **plus**
   * **80% of the Protocol’s iAERO rewards**.
     Protocol’s iAERO rewards are **P × 80% × R**; routing **80%** of that to LIQ stakers adds **0.64·P·R** to the LIQ pool.

   So the **LIQ pool** distributes $(0.08 + 0.64P) \cdot R$, and **you** get:

   $$
   I_{\text{LIQ}} = y \cdot (0.08 + 0.64 P) \cdot R
   $$

**Total weekly income to a user who stakes both iAERO and LIQ:**

$$
\boxed{I_{\text{total}} = x \cdot 0.80R \;+\; y \cdot (0.08 + 0.64P)\,R}
$$

> **Key idea:** As **P** rises (more protocol‑owned iAERO from fees + cheap peg buys), the **LIQ pool** grows **non‑linearly** via the **+0.64 P** term.
> Sellers → more protocol iAERO → more flow to LIQ stakers.

---

## “More than 100%” – What Does That Mean?

When people say “more than 100% of veAERO rewards,” they mean: compared to a **plain veAERO baseline** where a user with **1% of the veAERO** would expect **1% of R** each week, a user who **stakes iAERO (x)** and **also stakes LIQ (y)** can **exceed** that simple baseline because they tap **two** spigots (iAERO and LIQ), and **LIQ’s spigot** gets boosted by sellers via **P**.

You’re not minting extra tokens out of thin air; you’re **capturing more of the pie** because:

* **Some holders sell** iAERO under peg → protocol accumulates iAERO **cheaply**.
* Protocol **stakes** those tokens → routing **80%** of that **staking income** to **LIQ stakers**.
* If **you** are a LIQ staker, **you** get that incremental flow.

---

## Numerical Examples

### Example A – Baseline, No Protocol Share (P = 0)

* **R = \$100,000** weekly.
* You stake **x = 1%** of iAERO; **y = 2%** of LIQ.
* **P = 0** (no protocol iAERO yet).

Income:

* iAERO: $$1\% \times 80\% \times 100{,}000 = \$800$$
* LIQ: $$2\% \times (8\% + 0.64\cdot 0)\times 100{,}000 = 0.02 \times 0.08 \times 100{,}000 = \$160$$
* **Total = \$960**

**Plain veAERO baseline (1% of R) = \$1,000.**
Here you’re at **96%** of that baseline. (Reasonable when P=0.)

---

### Example B – Sellers Push Below \$0.85 → Protocol P = 25%

* Still **R = \$100,000**; **x = 1%**, **y = 2%**.
* Now **P = 25%** (protocol accumulated iAERO from deposit fees + cheap peg buys during drawdowns and staked it).

Income:

* iAERO: $$1\% \times 80\% \times 100{,}000 = \$800$$
* LIQ: $$2\% \times (8\% + 0.64 \cdot 0.25) \times 100{,}000      = 0.02 \times (0.08 + 0.16)\times 100{,}000      = 0.02 \times 0.24 \times 100{,}000      = \$480$$
* **Total = \$1,280**

**Plain veAERO baseline (1% of R) = \$1,000.**
Now you’re at **128%** of the baseline.
The **extra \$280** comes **entirely** from owning **LIQ** while **P > 0**.

---

### Example C – Peg Defense Buys at a Discount (Why P Grows Faster)

If the Peg Reserve buys iAERO at **\$0.80** using its **10% of R** budget:

* Weekly peg budget \~ **0.10·R = \$10,000**,
* Price **\$0.80** → it acquires **\$10,000 / 0.80 = 12,500 iAERO** for the same spend,
* Those tokens are **staked**; over time, **P** rises **faster** the deeper the discount.

This is why drawdowns **help long‑term stakers**: sellers fund the protocol’s iAERO bag at a discount, and **80% of that bag’s yield** flows to **LIQ stakers**.

---

## Deposit Fee (5%) Is a Permanent Tailwind for P

On every deposit:

* User mints **95% iAERO** to themselves,
* Protocol mints **5% iAERO** to treasury (protocol‑owned),
* **Total iAERO issued = 100% of AERO deposited.**

If both user and protocol stake at similar rates, the protocol’s **baseline share of the staking pool** starts near **\~5%** and **rises** as peg buys accumulate. Even **without** drawdowns, **P** trends upward with growth.

---

## What’s the Maximum “Community Capture”?

At the **system** level, the split is always **80/10/10** of **R**. We don’t “create” extra rewards.

But for **you**, if you hold **both** iAERO and LIQ, you combine:

* Your **iAERO share** of the **80%**, **plus**
* Your **LIQ share** of **(8% + 0.64·P)**.

That’s how your **personal** capture can be **>100% of a plain veAERO baseline**, especially when **P** rises due to sellers.

---

## Edge Cases & FAQs

* **Q: Does this guarantee >100%?**
  **No.** It depends on **P** (protocol iAERO share), your **x** and **y**, the weekly **R**, and market conditions. When there are few sellers and P stays small, your combined capture may sit near or below 100% of a plain veAERO baseline.

* **Q: Is the peg always defended?**
  The **peg reserve buys only below \$0.85**. It is **opportunistic**, not a constant market maker.

* **Q: Why not send 100% to iAERO stakers?**
  The 10% → **TreasuryDistributor** (of which **8%** reaches **LIQ stakers**) and 10% → **Peg Reserve** make the system **antifragile**: sellers subsidize long‑term holders, and there’s capital dedicated to **buying at a discount** when it matters most.

* **Q: Isn’t the 5% deposit fee just a tax?**
  It’s a **commitment device**: it seeds **protocol‑owned iAERO** that is **staked**, and **80%** of those earnings are **recycled to LIQ stakers**. Over time, this **transfers value from churn to conviction**.

* **Q: Where does my LIQ yield come from?**
  Two places: (i) **8%** of weekly R (via TreasuryDistributor), and (ii) **80% of the protocol’s iAERO staking rewards** $\Rightarrow 0.64·P·R$ flows to the LIQ pool. When **P** grows, LIQ yield grows.

---

## TL;DR

* **Sellers** of iAERO → increase **protocol iAERO** (via peg buys & the 5% deposit fee).
* Protocol stakes that iAERO and routes **80% of its staking income** to **LIQ stakers**.
* If **you** stake **iAERO** (**x**) **and** **LIQ** (**y**), your weekly income is:

  $$
  I_{\text{total}} = x \cdot 0.80R \;+\; y \cdot (0.08 + 0.64P)\,R
  $$
* As **P** rises (especially during drawdowns), **LIQ** yield **accelerates**.
* That’s how aligned users can **exceed** a plain **veAERO** baseline; not by printing new rewards, but by **capturing more of the pie** that sellers walk away from.

> **Not financial advice.** Actual outcomes depend on markets, vote results, pool composition, and protocol params. The peg reserve only acts **below \$0.85**.

---

## Worked “Quick Calculator”

For a back‑of‑the‑envelope check:

* Pick **R** (weekly rewards in USD).
* Estimate your **x** (share of iAERO staking pool).
* Estimate your **y** (share of LIQ staking pool).
* Estimate **P** (protocol’s share of iAERO staking).

  > Rough guide: start \~**5%** from deposits; rises when iAERO **< \$0.85** and peg defense buys; can be materially higher in stress.

Then compute:

* **iAERO leg:** $x \cdot 0.80 \cdot R$
* **LIQ leg:** $y \cdot (0.08 + 0.64P)\cdot R$
* **Total = sum**. Compare to a **plain veAERO baseline** of $x \cdot R$.

If $y > 0$ and **P** is meaningful, you’ll often see **Total > $x \cdot R$**.

---

### Example Card (Doc Sidebar)

> **Example:** $$R=\$100k$$, $x=1\%$, $y=2\%$, $P=25\%$
>
> * iAERO: $$0.01 \times 0.80 \times 100k = \$800$$
> * LIQ: $$0.02 \times (0.08+0.64 \times 0.25)\times 100k = \$480$$
> * **Total = \$1,280** vs plain veAERO baseline $$= 0.01\times 100k = \$1,000$$.
>   **You capture 128%** of the plain veAERO baseline.

---
