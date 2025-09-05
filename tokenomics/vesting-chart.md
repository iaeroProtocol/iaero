# Vesting Chart

```markdown
# LIQ Vesting Visualization

## 3-Year Linear Vesting Chart

## LIQ Token Distribution & Vesting

| Allocation | Amount | Vesting Period | Release Schedule |
|------------|--------|----------------|------------------|
| **Emissions** | 70M LIQ | Ongoing | Halving every 5M distributed |
| **Treasury** | 20M LIQ | 3 years | Linear monthly unlock |
| **Team** | 10M LIQ | 3 years | Linear monthly unlock |

### Vesting Timeline
- **Month 0**: 0 LIQ available (fully locked)
- **Month 12**: ~8.33M unlocked (Treasury: 5.55M, Team: 2.78M)
- **Month 24**: ~16.67M unlocked (Treasury: 11.11M, Team: 5.56M)
- **Month 36**: 30M fully unlocked (Treasury: 20M, Team: 10M)

## Claiming Process

1. **Check Vested Amount**: Query `vested()` function
2. **View Claimable**: Call `releasable()` to see available
3. **Claim Tokens**: Execute `claim()` for specific stream
4. **Batch Claims**: Use `claimAll()` for both streams

## Post-Vesting

After 3 years:
- All 20M LIQ fully vested and claimed
- Vesting contract can be swept of any dust
- No further emissions from vesting
- Focus shifts to community emissions only
```
