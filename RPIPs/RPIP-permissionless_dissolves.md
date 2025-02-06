---
rpip:
title: Permissionless Dissolves
description: Remove a current oDAO duty by making it permissionless
author: Valdorff (@Valdorff)
discussions-to: 
status: Draft
type: Protocol
category: Core
created: 2025-02-05
requires: 59
vote-to:
vote-date:
vote-result:
---

## Abstract
This RPIP supports a followup vote ratifying plans to tie up loose ends in the Saturn specification. It also provides a platform for community members to raise and discuss any as-yet-unnoticed problems in those plans.

## Specification
- The "`stake` Transaction" section of RPIP-59 SHALL be replaced with the following:
```md
#### `stake` Transaction
- `stake` SHALL stake the remaining 31 ETH to the beacon chain to make a complete validator
- After ETH is assigned to a validator, the Node Operator SHALL be able to call a function to dissolve that validator
- If `stake` has not been called within `time_before_dissolve` after the ETH was assigned to a validator, any user SHALL be able to call a function to dissolve that validator. The user calling this function SHALL receive `dissolve_reward`.
- When a validator is dissolved:
  - Unstaked balance SHALL be returned to the deposit pool
  - The bonded value SHALL be recoverable. This MAY require further action from the node operator. This MAY temporarily require additional ETH from the node operator.
```
- The parameter table in the `Deposit Mechanics Specification` section of RPIP-59 SHALL get a new parameter called `dissolve_reward` with an initial value of 0.01 ETH and a guardrail of ≤0.2 ETH

## Rationale
Per the pDAO charter, we prioritize permissionlessness and decentralization where possible. Per the oDAO charter, the oDAO seeks to minimize its role where possible. This seems to be a case where we can fairly painlessly move from oDAO reliance to relying on any user eventually acting.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).