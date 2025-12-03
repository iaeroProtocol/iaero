# Token Sweeper

## Turn Your Dust Into Dollars (or ETH)

Got 47 random tokens cluttering your wallet like forgotten leftovers in your fridge? Token Sweeper is your DeFi spring cleaning solution - batch swap all that dust into USDC or WETH in a few batches and let us take all the hassle out of it.

No more clicking through 47 individual swaps. No more paying 47 separate gas fees. No more wondering "wait, what even is this token?"

---

## ⚠️ Alpha Notice

**Token Sweeper is currently in alpha.** 

This means:
- 🧪 Features are still being tested and refined
- 🐛 You might encounter bugs (please report them!)
- 🚧 UI/UX improvements are ongoing
- 💡 Your feedback directly shapes the product

We're actively developing new features including **cross-chain bridging** and **multi-aggregator price comparison** to get you even better rates.

Use at your own risk, start with small amounts, and remember: alpha software gonna alpha.

---

## Core Features

🔍 **Smart Wallet Scanning** - Automatically finds all your ERC-20 tokens across 9 chains

💰 **Batch Swaps** - Swap 10, 20, or 50 tokens in ONE transaction

🎯 **Best Rates** - Routes through 100+ DEXs via 0x aggregator

🛡️ **Spam Filtering** - Automatically hides scam tokens and honeypots

📊 **Price Impact Warnings** - Know exactly what you're getting before you commit

🔬 **Simulation First** - Every swap is dry-run tested before execution

➕ **Manual Token Add** - Found a token we missed? Add it yourself

---

## The Fee

**0.05% (5 basis points)**

That's it. Swap $1,000 of tokens, pay $0.50 in protocol fees.

For context:
- Uniswap: 0.30% (6x more)
- Most aggregators: 0.10-0.30%
- Us: 0.05%

The fee helps keep the robots running and the devs caffeinated.

---

## Supported Chains

| Chain | Status | Notes |
|-------|--------|-------|
| 🔵 **Base** | ✅ Live | Home chain, best tested |
| ⟠ **Ethereum** | ✅ Live | Higher gas, bigger swaps recommended |
| 🔴 **Arbitrum** | ✅ Live | Fast & cheap |
| 🔴 **Optimism** | ✅ Live | Fast & cheap |
| 🟣 **Polygon** | ✅ Live | Very cheap gas |
| 🟡 **BNB Chain** | ✅ Live | Watch for tax tokens |
| 🔺 **Avalanche** | ✅ Live | C-Chain supported |
| 📜 **Scroll** | ✅ Live | ZK rollup |
| 🟢 **Linea** | ✅ Live | ZK rollup |

---

## How To Use Token Sweeper

### Step 1: Connect Your Wallet

Click "Connect Wallet" and choose your preferred wallet. We support all major wallets via WalletConnect and browser extensions.

### Step 2: Select Your Chain

Make sure you're connected to the chain where your dust lives. The sweeper will automatically detect your network.

### Step 3: Scan Your Wallet

Click **"Scan Wallet"** to discover all tokens in your wallet. This typically takes 10-30 seconds depending on how many tokens you have.

> 💡 **Pro Tip**: If you know you have a token that wasn't detected, use the manual token input field to add it by address.

### Step 4: Review Your Tokens

You'll see a list of all detected tokens with:
- Current balance
- Estimated USD value
- Price per token

Tokens are pre-selected for sweeping. **Uncheck** any you want to keep.

### Step 5: Choose Your Output

Pick what you want to receive:
- **USDC** - Stable value, good for taking profits
- **WETH** - Stay in ETH, good for gas reserves

### Step 6: Preview Quotes

Click **"Sweep X Tokens to USDC/WETH"** to fetch quotes. You'll see:
- Input value vs output value
- Price impact percentage
- Which tokens have high slippage

> ⚠️ **High Impact Warning**: Tokens with >5% price impact need "Force" enabled to swap. This protects you from bad trades.

### Step 7: Approve & Execute

1. **Approve tokens** - One-time approval for each token
2. **Confirm the swap** - Review final amounts
3. **Execute** - Sign the transaction

### Step 8: Collect Your Bag

Watch the magic happen. Your USDC or WETH will arrive in your wallet once the transaction confirms.

---

## Do's and Don'ts

### ✅ Do's

| Do This | Why |
|---------|-----|
| **Check price impact** | High impact = you're getting a bad rate |
| **Use "Force" carefully** | Only for tokens you REALLY want to dump |
| **Scan after airdrops** | New tokens appear all the time |
| **Report spam tokens** | Click ✕ to mark scams - helps everyone |
| **Refresh if prices seem off** | Prices cache for 5 mins |
| **Check the output preview** | Know what you're getting before you sign |

### ❌ Don'ts

| Don't Do This | Why |
|---------------|-----|
| **Don't sweep tokens you want to keep** | Double-check your selection! |
| **Don't ignore high price impact** | >10% impact means bad liquidity |
| **Don't use on BNB tax tokens** | They'll fail - use PancakeSwap directly |
| **Don't expect miracles from honeypots** | If you can't sell it on Uniswap, we can't sell it either |
| **Don't include your main holdings** | This is for DUST, not your retirement fund |

---

## Real Talk: What Can and Can't Be Swept

### ✅ Works Great

- Standard ERC-20 tokens
- DEX LP tokens (converted to underlying value)
- Governance tokens
- Meme coins with liquidity
- Airdropped tokens (legitimate ones)
- Yield farming rewards

### ⚠️ Might Have Issues

- Very low liquidity tokens (high slippage)
- Recently launched tokens (no DEX pools yet)
- Rebasing tokens (amounts change constantly)
- Tokens with transfer limits

### ❌ Won't Work

- Honeypots (can't be sold by design)
- Tax tokens on BSC (>5% transfer tax breaks routing)
- Scam airdrop tokens (no liquidity, no value)
- NFTs (this is for ERC-20 tokens only)
- Native ETH/BNB/MATIC (only ERC-20 wrapped versions)

---

## Contract Addresses

All swaps execute through our audited RewardSwapper contract:

### RewardSwapper Contracts

| Chain | Contract Address | Verified |
|-------|------------------|----------|
| **Base** | `0x25f11f947309df89bf4d36da5d9a9fb5f1e186c1` | [BaseScan ↗](https://basescan.org/address/0x25f11f947309df89bf4d36da5d9a9fb5f1e186c1) |
| **Ethereum** | `0x75f57Faf06f0191a1422a665BFc297bcb6Aa765a` | [Etherscan ↗](https://etherscan.io/address/0x75f57Faf06f0191a1422a665BFc297bcb6Aa765a) |
| **Arbitrum** | `0x75f57Faf06f0191a1422a665BFc297bcb6Aa765a` | [Arbiscan ↗](https://arbiscan.io/address/0x75f57Faf06f0191a1422a665BFc297bcb6Aa765a) |
| **Optimism** | `0x75f57Faf06f0191a1422a665BFc297bcb6Aa765a` | [OP Etherscan ↗](https://optimistic.etherscan.io/address/0x75f57Faf06f0191a1422a665BFc297bcb6Aa765a) |
| **Polygon** | `0x75f57Faf06f0191a1422a665BFc297bcb6Aa765a` | [PolygonScan ↗](https://polygonscan.com/address/0x75f57Faf06f0191a1422a665BFc297bcb6Aa765a) |
| **BNB Chain** | `0x75f57Faf06f0191a1422a665BFc297bcb6Aa765a` | [BscScan ↗](https://bscscan.com/address/0x75f57Faf06f0191a1422a665BFc297bcb6Aa765a) |
| **Avalanche** | `0x75f57Faf06f0191a1422a665BFc297bcb6Aa765a` | [Snowtrace ↗](https://snowtrace.io/address/0x75f57Faf06f0191a1422a665BFc297bcb6Aa765a) |
| **Scroll** | `0x75f57Faf06f0191a1422a665BFc297bcb6Aa765a` | [ScrollScan ↗](https://scrollscan.com/address/0x75f57Faf06f0191a1422a665BFc297bcb6Aa765a) |
| **Linea** | `0x679e6e600E480d99f8aeD8555953AD2cF43bAB96` | [LineaScan ↗](https://lineascan.build/address/0x679e6e600E480d99f8aeD8555953AD2cF43bAB96) |

### Output Token Addresses (USDC)

| Chain | USDC Address | Decimals |
|-------|--------------|----------|
| Base | `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913` | 6 |
| Ethereum | `0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48` | 6 |
| Arbitrum | `0xaf88d065e77c8cC2239327C5EDb3A432268e5831` | 6 |
| Optimism | `0x0b2C639c533813f4Aa9D7837CAf62653d097Ff85` | 6 |
| Polygon | `0x3c499c542cEF5E3811e1192ce70d8cC03d5c3359` | 6 |
| BNB Chain | `0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d` | **18** ⚠️ |
| Avalanche | `0xB97EF9Ef8734C71904D8002F8b6Bc66Dd9c48a6E` | 6 |
| Scroll | `0x06eFdBFf2a14a7c8E15944D1F4A48F9F95F663A4` | 6 |
| Linea | `0x176211869cA2b568f2A7D4EE941E073a821EE1ff` | 6 |

> ⚠️ **Note**: BNB Chain USDC uses 18 decimals, unlike other chains which use 6.

---

## Security Model

### How Your Tokens Stay Safe

1. **You approve to RewardSwapper** - Not to random aggregator contracts
2. **Simulation before execution** - Every swap is tested before real execution
3. **Slippage protection** - Default 3% max slippage prevents sandwich attacks
4. **No infinite approvals by default** - Only approves what's needed
5. **Contract is verified** - Read the code yourself on block explorers

### What You're Trusting

- **RewardSwapper contract** - Our battle-tested swap router
- **0x Aggregator** - Industry-standard DEX aggregation
- **The underlying DEXs** - Uniswap, Curve, Balancer, etc.

### What We Can't Protect Against

- Tokens that are designed to be unsellable (honeypots)
- Market conditions changing between quote and execution
- Blockchain congestion causing delayed transactions
- You accidentally including tokens you wanted to keep

---

## Troubleshooting

### "No swappable tokens found"

- Make sure you're on the correct chain
- Try clicking "Re-scan" to fetch fresh data
- Some tokens may have zero balance after fees

### "Quote failed" for a specific token

- Token may have no liquidity on DEXs
- Token may be a honeypot/scam
- Try marking it as spam and sweeping the rest

### "Transaction reverted"

- Slippage exceeded - try increasing slippage or use "Force"
- Token has transfer tax - common on BSC
- Approval may have expired - refresh and try again

### "High price impact" warning

- Token has low liquidity
- You're selling a large portion of the pool
- Consider selling smaller amounts or accepting the loss

### Stuck on "Checking approvals"

- Wallet may need a refresh
- RPC might be slow - wait or try again
- Check that you have enough native token for gas

---

## Coming Soon

### 🌉 Cross-Chain Bridging
Sweep tokens on Arbitrum, bridge proceeds to Base, all in one flow.

### 🔀 Multi-Aggregator Routing  
Compare quotes from 0x, 1inch, Paraswap, KyberSwap, and more - automatically pick the best rate for each token.

### 📊 Analytics Dashboard
See your sweeping history, total value recovered, gas saved vs individual swaps.

### 🤖 Scheduled Sweeps
Set it and forget it - auto-sweep new airdrops weekly.

### 🎯 Custom Output Tokens
Sweep to any token, not just USDC/WETH.

---

## The TL;DR

Token Sweeper batch-swaps your wallet dust into USDC or WETH.

- **Fee**: 0.05% (five basis points)
- **Chains**: 9 (Base, ETH, Arb, OP, Polygon, BSC, Avax, Scroll, Linea)
- **Status**: Alpha (use small amounts first)
- **Safety**: Simulated before execution, slippage protected

Connect wallet → Scan → Select tokens → Preview → Sweep → Profit.

Stop leaving money scattered across your wallet. Let the robots do the cleanup.

---

## Links

- 🚀 [Launch Token Sweeper](https://sweeper.iaero.io)
- 🏠 [iAERO Protocol](https://iaero.io)
- 💬 [Discord](https://discord.gg/iaero)
- 🐦 [Twitter](https://twitter.com/iaaboratory)
- 📖 [GitHub](https://github.com/iaeroProtocol)

---

*Built with ❤️ by [iAERO Protocol](https://iaero.io)*

*Disclaimer: Token Sweeper is provided as-is. Always verify transactions before signing. Not financial advice. Past sweeping performance does not guarantee future dust accumulation. Please sweep responsibly.*
