# Vesting Chart

```markdown
## LIQ Token Distribution & Vesting

| Allocation | Amount | Vesting Period | Release Schedule |
|------------|--------|----------------|------------------|
| **Emissions** | 70M LIQ | Ongoing | Halving every 5M distributed |
| **Treasury** | 20M LIQ | 3 years | Linear monthly unlock |
| **Team** | 10M LIQ | 3 years | Linear monthly unlock |

### Vesting Timeline

| Milestone | Unlocked Amount | Breakdown |
|-----------|----------------|-----------|
| **Month 0** | 0 LIQ | Fully locked |
| **Month 12** | ~8.33M | Treasury: 5.55M, Team: 2.78M |
| **Month 24** | ~16.67M | Treasury: 11.11M, Team: 5.56M |
| **Month 36** | 30M | Treasury: 20M, Team: 10M (fully vested) |

### Claiming Process

| Step | Function | Description |
|------|----------|-------------|
| 1 | `vested()` | Check vested amount |
| 2 | `releasable()` | View claimable tokens |
| 3 | `claim()` | Claim specific stream |
| 4 | `claimAll()` | Batch claim both streams |
```
