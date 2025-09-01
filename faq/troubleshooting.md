# Troubleshooting

## Common Issues

### Transaction Failures

**Problem**: "Wallet mutated or stripped calldata"

**Solution**: 
- Disable transaction simulation in wallet
- Try using MetaMask or Rabby
- Use the Safe transaction builder for multisigs

### NFT Not Merging

**Problem**: Deposited NFT shows "needsMerge: true"

**Solution**:
- Ensure auto-max lock is disabled
- Call `performMaintenance()` manually
- Wait for next deposit to trigger merge

### No Rewards Showing

**Problem**: Staked iAERO but no rewards visible

**Solution**:
- Rewards are distributed weekly
- Check if rewards have been harvested
- Ensure you're staking in the correct contract

### Approval Issues

**Problem**: Can't approve AERO/veNFT

**Solution**:
- Reset approval to 0 first
- Try increasing gas limit
- Check wallet has sufficient ETH

## Getting Help

- Discord: [Join our community](https://discord.gg/RtGST592)
- Twitter: @iAEROProtocol
