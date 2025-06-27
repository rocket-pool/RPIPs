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
    - give the Node Operator `credit` equal to the bonded value minus 1 ETH (which was deposited to beacon chain)
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

### Adding `pdao_share`
- Add a new protocol parameter to RPIP-46:

| Name        | Type | Initial Value | Guard Rails                 |
|-------------|------|---------------|-----------------------------|
| pdao_share  | pct  | 0             | reth_commission <= 100%     |

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

### Permissionless capital distribution
Before any subsections in the RPIP-43 specification, the following SHALL be added:

```md
This specification introduces the following pDAO protocol parameters:

| Name                            | Type    | Initial Value | Guard Rails        |
|---------------------------------|---------|---------------|--------------------|
| `notify_threshold`              | uint256 | `12 hours`    | \≥ 2 hours         |
| `late_notify_fine`              | uint256 | `0.05 ETH`    | ≤ 0.5 ETH          |
| `user.distribute.window.length` | uint256 | `7 days`      | \≥ 1 day; ≤30 days |
```

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
- There SHALL be a capital distribution process in the megapool
  - 
  - There SHALL be a permissionless `notify_exit` function, which proves the `withdrawable_epoch` and is intended to be called by a node operator per validator they exit
    - On notification, distribution of rewards SHALL be prevented after the earliest validator `withdrawal_epoch`, until all exiting validators have had `notify_final_balance` called
  - Any oDAO member SHALL be able to create an exit challenge if a node operator does not call `notify_exit` more than `notify_threshold` before `withdrawable_epoch`
    - There MAY be additional restrictions to which oDAO members can challenge (eg, not the oDAO member that most recently challenged)
    - There MAY be support for batching multiple exit challenges
    - Until an exit challenge is resolved, a node operator SHALL NOT be able to distribute rewards 
    - An exit challenge SHALL be resolved when either:
      - `notify_exit` is called successfully, in which case the validator is considered exiting; `debt` SHALL be increased by `late_notify_fine`
      - a proof that the validator in question is _not_ exiting is provided (eg, showing that `withdrawable_epoch` is `FAR_FUTURE_EPOCH`). In this case, the validator will not be considered exiting.
  - There SHALL be a `notify_final_balance` function, which can be called once exited validators are withdrawable. This function MUST:
    - Include a proof of the withdrawal balance for the validator (capital)
    - From the capital, send borrowed funds to the rETH contract
      - If the capital is insufficient to repay the protocol, the shortfall SHALL be added to `debt`
    - Use remaining capital to pay off `debt`, while the megapool has any
    - Hold remaining capital as unclaimed node operator funds
      - If called by the node operator, remaining capital SHOULD claim all unclaimed node operator funds
    - Be callable exclusively by the node operator starting at `withdrawable_epoch` and for `user.distribute.window.length` thereafter. After that exclusive period, this function SHALL be permissionless.
```

## Rationale

### Inflation share change supermajority
In the original tokenomics rework vote (<https://vote.rocketpool.net/#/proposal/0xb0e1c82fdf83f31de7b8f84767a092fddfb21abb71d81ea3aeec9acdcf43902d>), there is a change to inflation shares in a [section of RPIP-46](./RPIP-46.md#rpl-issuance-rewards-and-inflation). Per [a section of RPIP-25](./RPIP-25.md#updating-this-rpip), this required a supermajority of at least 75%. While there was a 99.4% outcome, it was an error not to explicitly call out the need for the supermajority.

If this RPIP passes over 75%, that will explicitly indicate that the inflation change has met its supermajority requirement.

Note: as this is procedural, there is no associated sentiment poll.

### Stake and dissolve
Making `stake` and `dissolve` permissionless aligns with the pDAO charter's values. Further, it is a simpler design than the earlier designs that required 2 or 3 transactions by the Node Operator. See [discord](https://discord.com/channels/405159462932971535/1215788197842255972/1353984953766907954) for detailed discussion that led to this design. Note that there is no on-chain reward for the permissionless call; for scope and flexibility reasons this was left off to allow for funding as desired without contract changes (as an example, there could be a GMC bounty that pays out based on 30th percentile gas cost in the last day).

Note: as this was previously noted in ["Still To Ratify Prior to Saturn 1](./RPIP-49.md#still-to-ratify-prior-to-saturn-1), there is no associated sentiment poll.

### Adding `pdao_share`
This change enables the protocol to direct ETH from rETH commission to the pDAO, establishing a flexible and native mechanism to fund protocol operations and growth. It does not specify which share `pdao_share` must come from, allowing future governance to reallocate as appropriate given current market conditions and protocol priorities.

Note: This was sentiment polled as summarized on the [forum](https://dao.rocketpool.net/t/protocol-funding-in-saturn-i-and-beyond/3555/12), with the key bit being that "there is strong consensus to at least build in a pdao share".

### Permissionless capital distribution
As the team worked on this feature, they determined that a notification of exit was required to prevent distribution of capital as rewards. The node operator should be responsible for doing this but the oDAO can be used as a fallback. The designed challenge system, aims to ensure node operators remain responsible but the protocol is secure.

The capital distribution function `notify_final_balance` is callable exclusively by the node operator within a window of time, to support arbitrage opportunities. After this period, it becomes permissionless to support rETH capital consolidation. 

There is also a correction to avoid using `minipool` namespaced variables.

Note: this is being added as an item the team "identifies that impact their ability to deliver this upgrade", per RPIP-49

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
