---
rpip:
title: Prioritize rETH withdrawal buffer
description: Direct ETH from new mints to the rETH contract to prioritize withdrawal liquidity
author: Valdorff (@Valdorff)
discussions-to: 
status: Draft
type: Protocol
category: Core
created: 2025-02-03
requires: 
vote-to:
vote-date:
vote-result:
---

## Abstract
This RPIP proposes a small change to hopefully be included in Saturn 1.

Currently, there are 2 different flows for ETH in the protocol: ETH from rewards and exits go to the rETH contract, while ETH from new rETH mints and new minipool deposits go to the deposit pool (in the latter case, after 1 ETH is put on the beacon chain). This RPIP proposes to switch ETH from rETH mints to the former flow, where they go to the rETH contract first. Note that excess ETH can go from the rETH contract to the deposit pool if there is more than a buffer threshold, currently set to 1% of rETH TVL, or 5090 ETH.

## Specification
- ETH from new rETH mints SHALL go to the rETH contract 

## Rationale
We currently have a rETH discount. This represents an imbalance between supply and demand where supply is too high relative to demand (or, equivalently, demand is too low relative to supply). In this state, we should (a) strive to avoid creating additional supply and (b) allow for rETH burns to better reflect demand.

To measure the impact of the proposed solution, a [simple model](https://dune.com/queries/4660050) was used:
- From 2024-10-30 to the present, find every rETH mint
- Track how much ETH went into those, but cap the contribution of any one mint to 5090 ETH (which is the size of the buffer before we overflow into the deposit pool)
  - Note, we can find the buffer size as 1% of rETH TVL in ETH, which comes from rethTargetCollateral*getEthValue(totalSupply) using the following functions: https://etherscan.io/address/0x89682e5F9bf69C909FC5E21a06495ac35E3671Ab#readContract#F17, https://etherscan.io/address/0xae78736cd615f374d3085123a210448e74fc6393#readContract#F11 and https://etherscan.io/address/0xae78736cd615f374d3085123a210448e74fc6393#readContract#F5 

From 2024-10-30 to 2025-02-07, we found that 17,921 ETH has gone to the deposit pool, which could've instead gone to the rETH contract. Using 1-inch on 2025-02-03, we estimated that there were ~7350 discounted ETH worth of rETH available in on-chain liquidity. 17.9k may have been enough to avoid this depeg, or quite close. Further, voting on in favor of something like this also serves to communicate that the pDAO is serious about prioritizing rETH.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).