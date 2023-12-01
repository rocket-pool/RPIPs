This expense funds the protocol development team for 12 months.

The amount will be paid in a lump-sum payment of: `49,420.55 RPL`, which is `5%` of RPL inflation over 12 months.

### Calculation 
Calculates the amount of RPL inflation to be spent on protocol development, by calculating the projected RPL inflation for 12 months and multiplying by the percentage of inflation allocated to protocol development.

`RPL_SUPPLY_START * (RPL_INFLATION_PER_DAY^365-1) * DEV_FUNDING_PERCENTAGE_OF_INFLATION`

```math
$$
P = S * (I^365-1) * D
$$
```

Where:

- `RPL_SUPPLY_START` is the RPL supply at the given start block. The core protocol team have been paid up to reward interval 14 (2023-09-28). So the start supply will be calculated from the block after interval 14 (`18232826`). 
- `RPL_INFLATION_PER_DAY` is the standard RPL inflation date rate that can be queried from the RPL token contract (`getInflationIntervalRate`).
  https://etherscan.io/token/0xd33526068d116ce69f19a9ee46f0bd304f21a51f#readContract 
- `DEV_FUNDING_PERCENTAGE_OF_INFLATION` is set at `5%` of RPL inflation.

Calculated as:

```python
>>> 19768220.1920667*(1.000133680617113500**365-1)*0.05
49420.55048018786
```

