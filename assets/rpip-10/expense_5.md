This expense funds the core protocol development team from **2025-10-24** until **2026-10-22** (inclusive).

The team has started a [prioritization discussion](https://dao.rocketpool.net/t/protocol-development-roadmap-prioritisation-2025/3709), which was continued with a [request for direction](https://dao.rocketpool.net/t/roadmap-summary-direction-needed/3755).

The team then released a roadmap and timelines [TBC].


The core protocol development team will provide quarterly updates on the roadmap progress before the following dates:
- 2026-01-26
- 2026-04-26
- 2026-07-26
- 2026-10-24

The amount will be paid in a lump-sum payment of: `54376.1983684802`*, which is `5%` of RPL inflation over that time.

### Calculation 
Calculates the amount of RPL inflation to be spent on core protocol development, by calculating the projected RPL inflation 1 year and multiplying by the percentage of inflation allocated to core protocol development.

```math
$$
ProtocolFunding = StartSupply * (InflationPerDay^{365}-1) * DevelopmentPercentage
$$
```

Where:
- $ProtocolFunding$ is the lump-sum payment of RPL to be paid for protocol development funding.
- $StartSupply$ is the RPL supply at the given start block. The core protocol team have been paid up to reward interval 41 (2025-10-23). So the start supply will be calculated from the block after interval 41. The supply at that block is expected to be `21870340.41 RPL`.
- $InflationPerDay$ is the standard RPL inflation day rate, queried from the RPL token contract (`getInflationIntervalRate`).
  https://etherscan.io/token/0xd33526068d116ce69f19a9ee46f0bd304f21a51f#readContract 
- $DevelopmentPercentage$ is set at `5%` of RPL inflation.
- If the RPL inflation day rate changes in this period, the corrected payment difference is to be added to the team's next yearly budget

Calculated as:

```python
>>> supply = 21870340405641693088131008
for _ in range(364):
  supply *= 1000133680617113500
  supply //= 10**18
inflation = supply - 21870340405641693088131008
inflation  *  0.05 / 10**18
54522.380398575624
```

*Note: Last year's funding interval had a duration of 365 days and funded one day into reward period 41. In order to define this and future funding periods to begin and end with reward periods, this funding period begins the same day the last period's ends.  `146.18` RPL is subtracted from the above figure so as not to pay twice for the same day, giving a final funding amount of `54376.1983684802` RPL.  

