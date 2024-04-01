---
rpip: 44
title: Integrating Execution Layer Triggerable Exits
description: Describes when EL triggerable exits will be used within RP
author: Valdorff (@Valdorff)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-05
requires: eip-7002
---

## Abstract
This proposal specifies when Execution Layer triggerable exits will be used by RP contracts. They will be used to provide protocol safety, as well as NO safety and convenience.

## Specification
- A Node Operator SHALL be able to request a validator exit from the Execution Layer
- Any user SHALL be able to exit an NO's minipool when an NO incurs deficit greater than a specified `exit_deficit` setting
  - `deficit` is `balance_deficit + penalties - expected_exit_balance`
    - `balance_deficit = 32-balance`; this represents the amount of validator principal lost due to Ethereum penalties
    - `penalties`; this represents the amount of ETH owed to the protocol for reasons defined elsewhere (eg, due to MEV theft or underperformance)
    - `expected_credit_from_exit`; this represents the amount of ETH the NO expects to receive as their share from validator exits that have begun but not yet completed
  - The initial `exit_deficit` setting SHALL be 0.5 ETH
  - The protocol SHOULD use `rewards` and/or `credit` held by the NO to decrease `debt` by the same amount prior to taking action on exits
- The procotol SHALL update `expected_credit_from_exit` when exits are begun or completed 
- The protocol SHOULD incentivize users to trigger exits when conditions are met

## Reference Implementation
TODO

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
