# Reward Distribution

## Sources of Rewards

1. **Aerodrome Voting Rewards**
   - Bribes from protocols
   - Trading fees from gauges
   - AERO emissions

2. **Partner Incentives**
   - Direct bribes to the vault
   - Partnership rewards

## Distribution Flow

Aerodrome Rewards → RewardsHarvester → Distribution
├── 80% to iAERO stakers
├── 10% to treasury
└── 10% to peg reserve

## Claiming Process

### For iAERO Stakers

1. Stake iAERO in the StakingDistributor
2. Rewards accumulate automatically and distribute after each epoch ends
3. Claim anytime through the UI

### Reward Tokens

Common reward tokens include:
- AERO
- USDC
- ETH
- Various protocol tokens

## Compounding

The vault automatically:
- Claims all available rewards weekly
- Processes and distributes to stakers
- Reinvests protocol fees

