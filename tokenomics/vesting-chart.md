### technical/vesting-chart.md
```markdown
# LIQ Vesting Visualization

## 3-Year Linear Vesting Chart

The chart below shows the linear vesting schedule for treasury and team allocations over 3 years.

<style>
.vesting-container {
    background: #1a1a2e;
    padding: 20px;
    border-radius: 8px;
    margin: 20px 0;
}
.chart-svg {
    width: 100%;
    max-width: 800px;
    height: 400px;
}
</style>

<div class="vesting-container">
<svg class="chart-svg" viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
  <!-- Grid -->
  <g stroke="#333" stroke-width="1">
    <line x1="80" y1="350" x2="750" y2="350"/>
    <line x1="80" y1="50" x2="80" y2="350"/>
    
    <!-- Horizontal grid lines -->
    <line x1="80" y1="110" x2="750" y2="110" stroke-dasharray="5,5" opacity="0.3"/>
    <line x1="80" y1="170" x2="750" y2="170" stroke-dasharray="5,5" opacity="0.3"/>
    <line x1="80" y1="230" x2="750" y2="230" stroke-dasharray="5,5" opacity="0.3"/>
    <line x1="80" y1="290" x2="750" y2="290" stroke-dasharray="5,5" opacity="0.3"/>
    
    <!-- Vertical grid lines -->
    <line x1="247" y1="50" x2="247" y2="350" stroke-dasharray="5,5" opacity="0.3"/>
    <line x1="415" y1="50" x2="415" y2="350" stroke-dasharray="5,5" opacity="0.3"/>
    <line x1="582" y1="50" x2="582" y2="350" stroke-dasharray="5,5" opacity="0.3"/>
  </g>
  
  <!-- Y-axis labels -->
  <text x="70" y="55" fill="#999" text-anchor="end" font-size="12">20M</text>
  <text x="70" y="115" fill="#999" text-anchor="end" font-size="12">15M</text>
  <text x="70" y="175" fill="#999" text-anchor="end" font-size="12">10M</text>
  <text x="70" y="235" fill="#999" text-anchor="end" font-size="12">5M</text>
  <text x="70" y="295" fill="#999" text-anchor="end" font-size="12">2.5M</text>
  <text x="70" y="355" fill="#999" text-anchor="end" font-size="12">0</text>
  
  <!-- X-axis labels -->
  <text x="80" y="370" fill="#999" text-anchor="middle" font-size="12">TGE</text>
  <text x="247" y="370" fill="#999" text-anchor="middle" font-size="12">Year 1</text>
  <text x="415" y="370" fill="#999" text-anchor="middle" font-size="12">Year 2</text>
  <text x="582" y="370" fill="#999" text-anchor="middle" font-size="12">Year 3</text>
  <text x="750" y="370" fill="#999" text-anchor="middle" font-size="12">Fully Vested</text>
  
  <!-- Treasury vesting line -->
  <line x1="80" y1="350" x2="750" y2="170" stroke="#4CAF50" stroke-width="3"/>
  
  <!-- Team vesting line -->
  <line x1="80" y1="350" x2="750" y2="170" stroke="#2196F3" stroke-width="3" stroke-dasharray="8,4"/>
  
  <!-- Total vested area -->
  <polygon points="80,350 750,350 750,50 80,350" fill="url(#gradient)" opacity="0.2"/>
  
  <!-- Gradient definition -->
  <defs>
    <linearGradient id="gradient" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" style="stop-color:#4CAF50;stop-opacity:0.5" />
      <stop offset="100%" style="stop-color:#2196F3;stop-opacity:0.5" />
    </linearGradient>
  </defs>
  
  <!-- Legend -->
  <g transform="translate(600, 80)">
    <rect x="0" y="0" width="140" height="80" fill="#1a1a2e" stroke="#333" rx="5"/>
    <line x1="10" y1="25" x2="40" y2="25" stroke="#4CAF50" stroke-width="3"/>
    <text x="45" y="30" fill="#999" font-size="12">Treasury (10M)</text>
    
    <line x1="10" y1="50" x2="40" y2="50" stroke="#2196F3" stroke-width="3" stroke-dasharray="8,4"/>
    <text x="45" y="55" fill="#999" font-size="12">Team (10M)</text>
  </g>
  
  <!-- Title -->
  <text x="400" y="30" fill="#fff" text-anchor="middle" font-size="18" font-weight="bold">
    LIQ Token Vesting Schedule
  </text>
</svg>
</div>

## Key Metrics

| Metric | Value |
|--------|-------|
| **Total Vesting** | 20,000,000 LIQ |
| **Treasury Allocation** | 10,000,000 LIQ |
| **Team Allocation** | 10,000,000 LIQ |
| **Vesting Period** | 3 years (1,095 days) |
| **Daily Release Rate** | ~18,265 LIQ total |
| **Monthly Release** | ~547,945 LIQ total |
| **Yearly Release** | ~6,666,667 LIQ total |

## Vesting Milestones

| Time | Treasury Vested | Team Vested | Total Vested | % Complete |
|------|-----------------|-------------|--------------|------------|
| Day 1 | 9,132 | 9,132 | 18,265 | 0.09% |
| Month 1 | 273,973 | 273,973 | 547,945 | 2.74% |
| Month 6 | 1,643,836 | 1,643,836 | 3,287,671 | 16.44% |
| Year 1 | 3,333,333 | 3,333,333 | 6,666,667 | 33.33% |
| Year 2 | 6,666,667 | 6,666,667 | 13,333,333 | 66.67% |
| Year 3 | 10,000,000 | 10,000,000 | 20,000,000 | 100% |

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
