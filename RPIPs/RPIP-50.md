---
rpip: 50
title: RPL Buy & LP
description: Direct revenue surplus from borrowed ETH to buy 90:10 RPL/rETH liquidity pool shares. 
author: Valdorff (@Valdorff)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-22
tags: tokenomics-2024, tokenomics-value-capture-option
---

## Abstract
This proposal establishes a contract that holds ETH from protocol revenue and allows it to be used for single-asset deposits in a specified RPL/rETH pool. This serves as a form of market buy and provides significant liquidity depth for RPL buying/selling.

## Specification
- A share of revenue from borrowed ETH SHALL be directed to a smart contract for the purpose of this liquidity provision
- Once `lp_deposit_threshold` has been reached, any user MAY call a function to:
  - Convert the ETH to rETH via native mint if possible, or using a specified `backup_reth_pool` 
  - Execute a single-asset deposit of `lp_deposit_threshold` to the `target_pool`
  - The caller SHALL receive a gas refund based on precalculated gas and the BASEFEE opcode
  - If the function has been called in the last `lp_deposit_cooldown` blocks, it MUST revert 
- The initial settings SHALL be:
  - `backup_reth_pool`: `0x1E19CF2D73a72Ef1332C882F20534B6519Be0276`, a Balancer rETH/ETH metastable pool
  - `target_pool`: <ADDRESS TBD>, a Balancer weighted pool with 90% RPL, 10% rETH, and a 0.5% swap fee
  - `lp_deposit_threshold`: 10 ETH
  - `lp_deposit_cooldown`: 25
- The share of RPL owned by the DAO in this liquidity pool(s) SHALL NOT be counted for:
  - Voting quorum
  - "% of RPL is contributing vote power in RP" as used in [RPIP-46](RPIP-46.md)
- The contract MUST be upgradeable

## Thoughts
- `lp_deposit_threshold` should be set high enough to prevent an undue gas burden, low enough to minimize common arbitrage, and high enough to keep the contract balance quite low most of the time
  - It should be modified by governance if those criteria are not being met
- The pool's swap fee is set on the higher end to discourage thrashing and low-value arbitrage
- The deposit cooldown is set to prevent multiple distributions in quick succession purely to enable arbitrage
- The pool is most vulnerable to value extraction attacks when `lp_deposit_threshold` is large relative to the pool size (ie, it significantly moves the price). It is recommended that the pDAO place a large amount of protocol-owned liquidity in the pool to begin with. This liquidity can be removed once the "Buy & LP" process has built up size sufficiently. 
- Some resources for Balancer weighted pools: https://docs.balancer.fi/concepts/pools/weighted.html, https://docs.balancer.fi/reference/math/weighted-math.html, and https://balancer.fi/whitepaper.pdf. If using this solution, some math should be done to understand what size pool and threshold make sense to avoid significant value extraction.
- The upgradable contract allows flexibility such as, but not limited to:
  - A new pool could be targeted
  - A gradual migration from a previously targeted pool to the current target (eg, an "all-asset" withdrawal followed by an "all-asset" deposit plus a single-asset deposit); this approach could reduce the amount of idle ETH by moving to a pool with greater RPL weight
  - Gradually burning RPL from a pool (eg, a single-asset withdrawal followed by a burn); this approach could limit idle funds _and_ limit total protocol value buildup in the LP


## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
