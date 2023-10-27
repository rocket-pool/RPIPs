---
rpip: 28
title: Deposits Under the Minimum
description: Allow minipool deposits that include an appropriate RPL stake, even when under the "minimum" RPL threshold 
author: Valdorff (@Valdorff)
discussions-to: https://dao.rocketpool.net/t/allow-minipool-deposits-while-under-min-rpl/2100
status: Review
type: Protocol
category: Core
created: 2023-08-11
---

## Abstract
This proposal removes an incentive to create sock-puppet nodes by adding the ability to do a
combined minipool deposit AND RPL stake. This would allow creating a new minipool even when under
the minimum.

## Motivation
Right now, if an NO falls below the minimum (value of RPL staked <10% of borrowed ETH), they can't 
add a new minipool to that node unless they stake enough to reach the minimum AND cover the new
minipool. Unless, of course, they make a new node. This means that an incentive currently exists to
make sock puppet nodes.

The proposed atomic method allows users that prioritize convenience or otherwise wish to remain on
one node an easy method to accomplish that goal, while still being able to add minipools. 

## Specification

- A function SHALL be added that allows an atomic RPL stake and minipool ETH deposit
- This function SHALL succeed if the staked RPL meets the amount that would be needed for this
  minipool on a fresh node

## Reference Implementation

- Add a new `RocketNodeDeposit.deposit_with_RPL` function that has the same arguments as
  `RocketNodeDeposit.deposit` plus a new `rpl_to_stake` argument 
- `RocketNodeDeposit.deposit` now calls `RocketNodeDeposit.deposit_with_RPL` with rpl_to_stake=0
- In `RocketNodeDeposit.deposit_with_RPL`
  - `require(msg.value == _bondAmount, "Invalid value");`
  - `deposited_with_enough_rpl = (rocketNetworkPrices.getRPLPrice() * rpl_to_stake > rocketDAOProtocolSettingsNode.getMinimumPerMinipoolStake() * (32-_bondAmount))`
  - Pass along `deposited_with_enough_rpl` as needed such that the deposit will succeed if EITHER:
    - `deposited_with_enough_rpl` is true
    - There's enough RPL to be above minimum once the new minipool is added (the current check)

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
