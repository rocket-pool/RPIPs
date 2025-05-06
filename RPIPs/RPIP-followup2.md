---
rpip:
title: Saturn Follow-up Vote No. 2
description: Follow-up vote to tie up loose ends in the Saturn specification
author: Valdorff (@Valdorff)
discussions-to:
status: Draft
type: Protocol
category: Core
created: 2025-05-05
requires: 25, 42, 44, 46, 59
vote-to:
vote-date:
vote-result:
tags: tokenomics-2024, tokenomics-content
---

## Abstract
This RPIP supports a follow-up vote ratifying plans to tie up loose ends in the Saturn specification. It also provides a platform for community members to raise and discuss any as-yet-unnoticed problems in those plans.

## Specification
- Ratify the inflation share change from the tokenomics rework vote as meeting supermajority requirements
- The "`stake` Transaction" section of RPIP-59 SHALL be replaced with the following:
```md
#### `stake` Transaction
- After ETH is assigned to a validator any user SHALL be able to call the `stake` function
- `stake` SHALL
    - validate the withdrawal credential is the megapool associated with the Node Operator that called `deposit` using a state proof (note that this requires waiting for the 1 ETH to create a validator on the beacon chain) (revert on failed validation)
    - stake the remaining 31 ETH to the appropriate validator to make a complete validator

#### `dissolve` Transaction
- After ETH is assigned to a validator any user SHALL be able to call the `dissolve` function
- `dissolve` SHALL:
    - validate one of the following (or revert on failed validation):
        - the withdrawal credential is _not_ the megapool associated with the Node Operator that called `deposit` using a state proof (note that this requires waiting for the 1 ETH to create a validator on the beacon chain)
        - `time_before_dissolve` has passed since the `deposit` call
    - return the unstaked balance of 31 ETH to the deposit pool
    - give the user calling this function `dissolve_reward`
    - give the Node Operator `credit` equal to the bonded value minus
      - 1 ETH (which was deposited to beacon chain)
      - `dissolve_reward`
```
- The following line of RPIP-44 ```Dissolved: the `stake` transaction for this validator wasn't executed in time (see [RPIP-44](RPIP-59.md))``` SHALL be replaced by ```Dissolved: the `stake` transaction for this validator was invalid or not executed in time (see [RPIP-59](RPIP-59.md))``` 
- `time_before_dissolve` in RPIP-59 SHALL update to an initial value of 28 days and a guardrail of ≥10 days
- The parameter table in the `Deposit Mechanics Specification` section of RPIP-59 SHALL get a new parameter called `dissolve_reward` with an initial value of `0.01 ETH` and guardrails of `≤0.2 ETH` and `≤(reduced_bond-1)`
- The `reduced_bond` parameter in RPIP-42 SHALL get a guardrail of `≥(dissolve_reward+1)`

## Rationale

### Inflation share change supermajority
In the original tokenomics rework vote (<https://vote.rocketpool.net/#/proposal/0xb0e1c82fdf83f31de7b8f84767a092fddfb21abb71d81ea3aeec9acdcf43902d>), there is a change to inflation shares in a [section of RPIP-46](./RPIP-46.md#rpl-issuance-rewards-and-inflation). Per [a section of RPIP-25](./RPIP-25.md#updating-this-rpip), this required a supermajority of at least 75%. While there was a 99.4% outcome, it was an error not to explicitly call out the need for the supermajority.

If this RPIP passes over 75%, that will explicitly indicate that the inflation change has met its supermajority requirement.

### Stake and dissolve
Making `stake` and `dissolve` permissionless aligns with the pDAO charter's values. Further, it is a simpler design than the earlier designs that required 2 or 3 transactions by the Node Operator. See [discord](https://discord.com/channels/405159462932971535/1215788197842255972/1353984953766907954) for detailed discussion that led to this design.


## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).