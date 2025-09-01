# Providing Liquidity for iAERO/AERO

## Why Provide Liquidity?

Adding liquidity to the iAERO/AERO pool:
- Earns trading fees (0.3% of volume)
- Earns additional AERO emissions (if gauged)
- Helps maintain iAERO peg to AERO
- Supports protocol growth
- earns LIQ community emissions

## Understanding the Pool

### Pool Type: Variable Rate (Volatile)
- Not a stable pool (prices can diverge)
- Standard xy=k AMM formula
- Suitable for correlated but not pegged assets

### Expected Price Relationship
- iAERO should trade near 0.85 AERO
- May trade at premium if high demand
- May trade at discount if low demand

## How to Add Liquidity

### Step 1: Prepare Tokens
You need both tokens in the pair:
- iAERO tokens
- AERO tokens

**Optimal Ratio**: Check current pool ratio on Aerodrome

### Step 2: Navigate to Aerodrome
1. Go to [aerodrome.finance/liquidity](https://aerodrome.finance/liquidity)
2. Search for "iAERO/AERO" pool
3. Click "Add Liquidity"

### Step 3: Enter Amounts
- Enter amount of iAERO
- AERO amount auto-calculates based on pool ratio
- Or enter AERO and let iAERO auto-calculate

### Step 4: Approve Tokens
1. Approve iAERO spending (if needed)
2. Approve AERO spending (if needed)
3. Wait for confirmations

### Step 5: Add Liquidity
1. Click "Supply"
2. Review terms and price impact
3. Confirm transaction
4. Receive LP tokens

## Managing Your Position

### LP Token Benefits
- Represents your pool share
- Automatically earns fees
- Can be staked in gauge for emissions
- Transferable and tradeable

### Staking LP in Gauge
1. Go to Aerodrome Finance Dashboard
2. Find iAERO/AERO position
3. Stake LP tokens
4. Earn additional AERO emissions

## Impermanent Loss Considerations

### What Is IL?
Impermanent loss occurs when token prices diverge from deposit ratio.

### iAERO/AERO Specific Factors
- **Lower IL Risk**: Both tokens tied to AERO value
- **Main Risk**: iAERO depegging significantly from expected ratio
- **Mitigation**: Fees often offset IL for correlated pairs

### IL Scenarios

**Scenario 1: iAERO trades at premium**
- You'll have more AERO, less iAERO
- Good if you're bullish on AERO

**Scenario 2: iAERO trades at discount**
- You'll have more iAERO, less AERO
- Good if you believe peg will restore

## Calculating Returns

### Total Returns =
1. **Trading Fees** (0.3% of volume)
2. **Plus: Emissions** (if staked in gauge)
3. **Plus/Minus: IL** (from price changes)
4. **Plus: iAERO staking** (if you stake remaining iAERO)

### Example APR Calculation
- Pool TVL: $1,000,000
- Daily Volume: $100,000
- Daily Fees: $300
- Annual Fees: $109,500
- Base APR: 10.95%
- Plus emissions: +20-50% APR (varies)

## Removing Liquidity

### How to Withdraw
1. Go to your liquidity positions
2. Select iAERO/AERO position
3. Choose % to remove (25%, 50%, 100%)
4. Click "Remove"
5. Receive both tokens proportionally

### If Staked in Gauge
1. First unstake from gauge
2. Then remove liquidity
3. Two-step process

## Risk Management

### Risks to Consider
- **Smart Contract Risk**: Unaudited protocols
- **IL Risk**: Price divergence between tokens
- **Liquidity Risk**: May face slippage on large withdrawals
- **Opportunity Cost**: Could earn more just staking iAERO

### Risk Mitigation
- Start with small position
- Monitor price ratios regularly
- Consider single-sided staking if unsure
- Keep some dry powder for rebalancing

## Advanced Strategies

### 1. Range Positioning
If using Slipstream (concentrated liquidity):
- Set range around 0.80-1.0 AERO per iAERO
- Tighter range = more fees but more IL risk
- Monitor and rebalance as needed

### 2. Gauge Voting
- Vote for iAERO/AERO gauge with your veAERO
- Increases emissions to the pool
- Higher APR for all LPs

## FAQ

**Q: Which is better - staking or LPing?**
A: Depends on risk tolerance. Staking is simpler and safer. LPing potentially earns more but has IL risk.

**Q: Can I lose tokens providing liquidity?**
A: You can't lose tokens, but value can decrease from IL if prices diverge significantly.

**Q: How often are LP rewards paid?**
A: Trading fees accumulate in real-time. Emissions paid weekly if staked in gauge.

**Q: Should I provide equal dollar values?**
A: The pool automatically requires the current ratio. You can't choose arbitrary ratios.
