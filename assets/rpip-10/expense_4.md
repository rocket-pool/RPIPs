This expense funds the core protocol development team from **2024-10-25** until **2025-10-24** (inclusive).

The team has completed a prioritization discussion, which has been summarized here: https://discord.com/channels/405159462932971535/405163979141545995/1291631201597263883

The team has not yet released the roadmap and timelines and so it cannot be included here; it is expected to be released in October 2024.  

The core protocol development team will provide quarterly updates on the roadmap progress before the following dates:
- 2025-01-26
- 2025-04-26
- 2025-07-26
- 2025-10-24

The amount will be paid in a lump-sum payment of: `52079.2001101265 RPL`, which is `5%` of RPL inflation over that time.

### Calculation 
Calculates the amount of RPL inflation to be spent on core protocol development, by calculating the projected RPL inflation 1 year and multiplying by the percentage of inflation allocated to core protocol development.

```math
$$
ProtocolFunding = StartSupply * (InflationPerDay^{365}-1) * DevelopmentPercentage
$$
```

Where:
- $ProtocolFunding$ is the lump-sum payment of RPL to be paid for protocol development funding.
- $StartSupply$ is the RPL supply at the given start block. The core protocol team have been paid up to reward interval 28 (2024-10-24). So the start supply will be calculated from the block after interval 28. The supply at that block is expected to be `20831680.04404107 RPL`.
- $InflationPerDay$ is the standard RPL inflation day rate, queried from the RPL token contract (`getInflationIntervalRate`).
  https://etherscan.io/token/0xd33526068d116ce69f19a9ee46f0bd304f21a51f#readContract 
- $DevelopmentPercentage$ is set at `5%` of RPL inflation.

Calculated as:

```python
>>> supply = 20831680044041071829638396
for _ in range(365):
  supply *= 1000133680617113500
  supply //= 10**18
inflation = supply - 20831680044041071829638396
inflation  *  0.05 / 10**18
52079.2001101265
```

