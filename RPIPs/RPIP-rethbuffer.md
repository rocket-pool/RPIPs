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

Currently, there are 2 different flows for ETH in the protocol: ETH from rewards and exits go to the rETH contract, while ETH from new rETH mints and new minipool deposits go to the deposit pool (in the latter case, after 1 ETH is put on the beacon chain). This RPIP proposes to switch ETH from rETH mints to the former flow, where they go to the rETH contract first. Note that excess ETH can go from the rETH contract to the deposit pool if there is more than a buffer threshold, currently set to 180 ETH.

## Specification
- ETH from new rETH mints SHALL go to the rETH contract 

## Rationale
We currently have a rETH discount. This represents an imbalance between supply and demand where supply is too high relative to demand (or, equivalently, demand is too low relative to supply). In this state, we should (a) strive to avoid creating additional supply and (b) allow for rETH burns to better reflect demand.

To measure the impact of the proposed solution, a [simple model](https://dune.com/queries/4660050) was used:
- From 2024-10-30 to the present, find every rETH mint
- Track how much ETH went into those, but cap the contribution of any one mint to 180 ETH (which is the size of the buffer before we overflow into the deposit pool)

As of 2025-02-03, we found that 5592 ETH has gone to the deposit pool, which could've instead gone to the rETH contract. Using 1-inch, we also estimated that there were ~7350 discounted ETH worth of rETH available in on-chain liquidity. 5592 is a highly significant portion of that total, so it seems likely that this change can have a material impact on the discount. Further, it also indicates that the pDAO is serious about prioritizing rETH.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).