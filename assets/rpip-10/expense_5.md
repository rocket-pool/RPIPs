This expense funds the core protocol development team from **2025-10-25** until **2026-10-24** (inclusive).

The team has completed a prioritization discussion, which has been summarized here: [[TBC]]

The team has released a roadmap and timelines, which can be found here: [[TBC]]

The core protocol development team will provide quarterly updates on the roadmap progress before the following dates:
- 2026-01-26
- 2026-04-26
- 2026-07-26
- 2026-10-24

The amount will be paid in a lump-sum payment of: `54683.160115634004`, which is `5%` of RPL inflation over that time.

### Calculation 
Calculates the amount of RPL inflation to be spent on core protocol development, by calculating the projected RPL inflation 1 year and multiplying by the percentage of inflation allocated to core protocol development.

```math
$$
ProtocolFunding = StartSupply * (InflationPerDay^{365}-1) * DevelopmentPercentage
$$
```

Where:
- $ProtocolFunding$ is the lump-sum payment of RPL to be paid for protocol development funding.
- $StartSupply$ is the RPL supply at the given start block. The core protocol team have been paid up to reward interval 40 (2025-10-24). So the start supply will be calculated from the block after interval 40. The supply at that block is expected to be `21873264.04 RPL`.
- $InflationPerDay$ is the standard RPL inflation day rate, queried from the RPL token contract (`getInflationIntervalRate`).
  https://etherscan.io/token/0xd33526068d116ce69f19a9ee46f0bd304f21a51f#readContract 
- $DevelopmentPercentage$ is set at `5%` of RPL inflation.
- If the RPL inflation day rate changes in this period, the corrected payment difference is to be added to the team's next yearly budget

Calculated as:

```python
>>> supply = 21873264046243601583579994
for _ in range(365):
  supply *= 1000133680617113500
  supply //= 10**18
inflation = supply - 21873264046243601583579994
inflation  *  0.05 / 10**18
54683.160115634004
```

