---
rpip: 72
title: Saturn Follow-up Vote No. 2
description: Follow-up vote to tie up loose ends in the Saturn specification
author: Valdorff (@Valdorff), ShfRyn (@ShfRyn)
discussions-to:
status: Review
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
This is broken down into a few sub-categories, to make it easier to match to the related portion within the [rationale section](#rationale).

### Inflation share change supermajority
- Ratify the inflation share change from the tokenomics rework vote as meeting supermajority requirements

### Stake and dissolve
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

- The following line of RPIP-44:
```md
Dissolved: the `stake` transaction for this validator wasn't executed in time (see [RPIP-44](RPIP-59.md))
```
SHALL be replaced by:
```md
Dissolved: the `stake` transaction for this validator was invalid or not executed in time (see [RPIP-59](RPIP-59.md))
```

- `time_before_dissolve` in RPIP-59 SHALL update to an initial value of 28 days and a guardrail of ≥10 days

- The parameter table in the `Deposit Mechanics Specification` section of RPIP-59 SHALL get a new parameter called `dissolve_reward` with an initial value of `0.01 ETH` and guardrails of `≤0.2 ETH` and `≤(reduced_bond-1)`

- The `reduced_bond` parameter in RPIP-42 SHALL get a guardrail of `≥(dissolve_reward+1)`

### Adding `pdao_share`
- Add a new protocol parameter to RPIP-46:

| Name        | Type | Initial Value | Guard Rails                  |
|-------------|------|----------------|------------------------------|
| pdao_share  | pct  | 0              | reth_commission <= 100%     |

- Update the defined revenue shares in RPIP-46 to include:
```md
- pdao_share:
  a portion of commission revenue directed to the pDAO treasury for protocol use.

reth_commission SHALL be defined as the sum of node_operator_commission_share, voter_share, and pdao_share.
```

- Update the list of updateable parameters in RPIP-46:
```md
node_operator_commission_share, node_operator_commission_share_council_adder, voter_share, and pdao_share
SHALL be updateable by any address in the allowlisted_controllers array.
```

- Add the following rationale to RPIP-46:
```md
A pdao_share is introduced to enable a direct stream of ETH to the pDAO treasury. This allows the protocol
to fund operations, grants, or other community-approved initiatives using rETH commission revenue.
This share is initially 0% but may be increased in the future based on governance needs.
```

### Permissionless distribution refinement
The following section of RPIP-43
```md
- There SHALL be a capital distribution function in the megapool
  - When called, capital borrowed from the protocol that has been released from
    exited validators SHALL be sent to the rETH contract.
    - If the capital is insufficient to repay the protocol, the shortfall SHALL be added to `debt`
  - When called while the megapool has `debt`, the remaining capital from exited validators SHALL first be used to pay off `debt`
  - When called, the remaining capital SHALL then be held in the megapool as unclaimed node operator funds
  - This function SHALL support permissionless use, with additional restrictions:
    - A permissionless `startUserDistribute` function SHALL record the time when called, as long as there is no previously recorded time _or_ the previously recorded time was longer than `minipool.user.distribute.window.start + minipool.user.distribute.window.length` ago
    - The capital distribution function may be called permissionlessly if `minipool.user.distribute.window.start` has passed since the recorded `startUserDistribute` call, but `minipool.user.distribute.window.start + minipool.user.distribute.window.length` has not yet passed since the recorded `startUserDistribute` call
```
SHALL be replaced by
```md
- There SHALL be a capital distribution function in the megapool
  - When called, capital borrowed from the protocol that has been released from
    exited validators SHALL be sent to the rETH contract.
    - If the capital is insufficient to repay the protocol, the shortfall SHALL be added to `debt`
    - There SHALL be a permissionless `notify_exit` function that validates an exiting validator, increments the amount of capital that will be released, and updates the epoch when it will be released if necessary (based on withdrawable epoch)
  - When called while the megapool has `debt`, the remaining capital from exited validators SHALL first be used to pay off `debt`
  - When called, the remaining capital SHALL then be held in the megapool as unclaimed node operator funds
  - This function SHALL support permissionless use, after a small delay `user.distribute.window.length`
    - This variable SHALL be initialized to 7 days
```

## Rationale

### Inflation share change supermajority
In the original tokenomics rework vote (<https://vote.rocketpool.net/#/proposal/0xb0e1c82fdf83f31de7b8f84767a092fddfb21abb71d81ea3aeec9acdcf43902d>), there is a change to inflation shares in a [section of RPIP-46](./RPIP-46.md#rpl-issuance-rewards-and-inflation). Per [a section of RPIP-25](./RPIP-25.md#updating-this-rpip), this required a supermajority of at least 75%. While there was a 99.4% outcome, it was an error not to explicitly call out the need for the supermajority.

If this RPIP passes over 75%, that will explicitly indicate that the inflation change has met its supermajority requirement.

Note: as this is procedural, there is no associated sentiment poll.

### Stake and dissolve
Making `stake` and `dissolve` permissionless aligns with the pDAO charter's values. Further, it is a simpler design than the earlier designs that required 2 or 3 transactions by the Node Operator. See [discord](https://discord.com/channels/405159462932971535/1215788197842255972/1353984953766907954) for detailed discussion that led to this design.

Note: as this was previously noted in ["Still To Ratify Prior to Saturn 1](./RPIP-49.md#still-to-ratify-prior-to-saturn-1), there is no associated sentiment poll.

### Adding `pdao_share`
This change enables the protocol to direct ETH from rETH commission to the pDAO, establishing a flexible and native mechanism to fund protocol operations and growth. It does not specify which share `pdao_share` must come from, allowing future governance to reallocate as appropriate given current market conditions and protocol priorities.

Note: This was sentiment polled as summarized on the [forum](https://dao.rocketpool.net/t/protocol-funding-in-saturn-i-and-beyond/3555/12), with the key bit being that "there is strong consensus to at least build in a pdao share".

### Permissionless distribution refinement
As the team worked on this feature, they determined that there was no good way to avoid an oDAO duty to notify about all exiting validators in a timely manner.
Given that notification, it was possible to get to the permissionless distribute with a single transaction to improve UX.
There is also a correction to avoid using `minipool` namespaced variables.

Note: this is being added as an item the team "identifies that impact their ability to deliver this upgrade", per RPIP-49

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
