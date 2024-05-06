---
rpip: 45
title: RPL Burn
description: Direct revenue surplus from borrowed ETH to buy and burn permissionlessly provided RPL.
author: Valdorff (@Valdorff)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-08
tags: tokenomics-2024, tokenomics-value-capture-option
---

## Abstract
This proposal establishes a contract that holds ETH from protocol revenue and allows for RPL to be burned in exchange for that protocol ETH. This provides direct utility to the RPL token.


## Specification
- A share of the revenue from borrowed ETH SHALL be directed to a smart contract (the burn contract) for this burn (see [RPIP-46](RPIP-46.md))
- Any user MAY call a function in the burn contract to get ETH from the burn contract in exchange for RPL
- Any RPL exchanged this way SHALL burned using the RPL token's `burn` or `burnFrom` functions
- The swap price SHALL be based on an on-chain oracle

## Implementation thoughts
There have been previous buy+burn mechanics susceptible to gaming. To that end, here's an example design that focuses on avoiding discontinuities by smoothly unlocking the funds available for exchange.

![burn.png](../assets/rpip-45/burn.png)

The way this works is that:
- New distributions go into the Filling bucket
- After one `bucket_period` has passed, upon the next distribution:
  - Move Unlocking to Unlocked
  - Move Filling to Unlocking
  - Increment bucket_block by one `bucket_period` (repeat as needed until `bucket_block` > `current_block`)
  - Put the distribution into filling
- The contract will have `Unlocked + Unlocking + Filling` ETH in it
- The contract will allow burning RPL against `Available = Unlocked + (Unlocking*UnlockingRatio)` ETH
  - `UnlockingRatio = 1 - ((bucket_block - current_block)/bucket_period)`
- The swap price will be based on a long Uniswap TWAP
  - For safety, the protocol should ensure we have enough protocol-owned liquidity in that Uniswap ETH/RPL pool
  - The swap price gives `rpl_to_eth_oracle_price * (1 + bonus_per_unlocked_eth * Available)` ETH per RPL
  - `bonus_per_unlocked_eth` should be set to 0 to start; if we see that certain market conditions cause undesirable ETH buildup, this number should be increased. For example, setting it to .00001 would pay out with a 1% bonus per 1000 Available ETH. This prevents a slowly gaining price from indefinitely accumulating ETH. It's unclear if this is necessary, but it seems prudent to include the tool. 

Let's visualize what being able to burn looks like in practice using real RPL price data and a simulated 12-hour TWAP.
![twap.png](../assets/rpip-45/twap.png)

We'd expect to burn all our available ETH at the beginning of a downturn (and thereby reduce its severity since that's not market sold). We'd also expect to continue burning against new distributions while the price is on a downward slope. On an upward slope, burning is not expected until after the top is reached and we turn around for a bit.

### Security
- The design intentionally presents a small surface area. Any benefit an attacker can accrue is limited by the fact that no user has a privileged position and that value is streamed in smoothly without any discontinuities.
- The cost to attack the oracle depends on:
  - TWAP duration - 12 hours is currently in use by the oDAO and seems quite reasonable
  - Liquidity depth in the pool - it is strongly recommended that the DAO have some full-range protocol-owned liquidity to set a high-enough floor cost for attacks
- The benefit to an attacker of burning is proportional to the time that they have unique access to a trade; since all users have the same access, this boils down to quick changes in accessible value in the burn contract
  - 1 day is proposed as the `bucket_period`


## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
