This expense funds the core protocol development team from **2023-10-26** until **2024-10-24** (inclusive).

The core protocol development team have formed an indicative [roadmap](https://medium.com/rocket-pool/rocket-pool-houston-upgrade-fa2de86118ad) from community feedback. The roadmap will evolve as more research, analysis, and community direction is incorporated.

The core protocol development team will provide quarterly updates on the roadmap progress before the following dates:
- 2024-01-26
- 2024-04-26
- 2024-07-26
- 2024-10-24

The amount will be paid in a lump-sum payment of: `49,420.55 RPL`, which is `5%` of RPL inflation over that time.

### Calculation 
Calculates the amount of RPL inflation to be spent on core protocol development, by calculating the projected RPL inflation 1 year and multiplying by the percentage of inflation allocated to core protocol development.

```math
$$
ProtocolFunding = StartSupply * (InflationPerDay^{365}-1) * DevelopmentPercentage
$$
```

Where:
- $ProtocolFunding$ is the lump-sum payment of RPL to be paid for protocol development funding.
- $StartSupply$ is the RPL supply at the given start block. The core protocol team have been paid up to reward interval 14 (2023-09-28). So the start supply will be calculated from the block after interval 14 (`18232826`). The supply at that block is queried as `19,76,220.1920667 RPL`.
- $InflationPerDay$ is the standard RPL inflation day rate, queried from the RPL token contract (`getInflationIntervalRate`).
  https://etherscan.io/token/0xd33526068d116ce69f19a9ee46f0bd304f21a51f#readContract 
- $DevelopmentPercentage$ is set at `5%` of RPL inflation.

Calculated as:

```python
>>> 19768220.1920667*(1.000133680617113500**365-1)*0.05
49420.55048018786
```

