---
rpip: draft
title: Deposits Under the Minimum
description: Allow minipool deposits that include an appropriate RPL stake, even when under the "minimum" RPL threshold 
author: Valdorff (@Valdorff)
discussions-to: 
status: Draft
type: Protocol
category: Core
created: 2023-08-11
---

## Abstract

Right now, if an NO falls below the minimum (value of RPL staked <10% of borrowed ETH), they add a
new minipool to that node unless they stake enough to reach the minimum AND cover the new minipool.
Unless, of course, they make a new node. This means that we're creating an incentive to make Sybil
nodes.

This proposal suggests adding the ability to do a combined minipool deposit AND RPL stake. This
would allow creating a new minipool even when under the minimum and remove the incentive to create
Sybil nodes. Because creating a Sybil node is an inconvenience, we can require _slightly_ more RPL
to be staked, such that the NO moves towards the minimum overall.

## Specification

- There SHALL be a function that allows an atomic RPL stake and minipool ETH deposit
- This function SHALL succeed if the staked RPL is 110% of the amount needed for this minipool
  - Note that this means the minipool MAY be created even if the overall node's RPL stake is below
    the minimum needed (eg, for RPL rewards) 

## Reference Implementation

- Add a new `RocketNodeDeposit.deposit_with_RPL` function that has the same arguments as
  `RocketNodeDeposit.deposit` plus a new `rpl_to_stake` argument 
- `RocketNodeDeposit.deposit` now calls `RocketNodeDeposit.deposit_with_RPL` with rpl_to_stake=0
- In `RocketNodeDeposit.deposit_with_RPL`
  - `require(msg.value == _bondAmount, "Invalid value");`
  - `force_minimum_met == (rocketNetworkPrices.getRPLPrice() * rpl_to_stake > rocketDAOProtocolSettingsNode.getForceMinimumPerMinipoolStake() * (32-_bondAmount))`
  - Pass along rpl_requirement_met as needed such that the deposit will succeed if EITHER:
    - `force_minimum_met` is true
    - There's enough RPL to be above minimum once the new minipool is added (the current check)
- Implement `rocketDAOProtocolSettingsNode.getForceMinimumPerMinipoolStake()` as
  - Start `node.per.minipool.stake.force_minimum` at 0.11 (10% more than `node.per.minipool.stake.minimum`)

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
