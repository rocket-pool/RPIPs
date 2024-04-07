---
rpip: 42
title: Bond Curves
description: Allow smaller bonds to dramatically increase capital efficiency
author: Valdorff (@Valdorff)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-08
requires: rpip-megapools, rpip-forced-exits
---

## Abstract
This proposal dramatically increases the LTV used in the protocol (loan to value; the amount of ETH that can be borrowed per unit of bonded ETH). It does this safely by:
- Enabling node-level penalties to mitigate/discourage MEV theft
- Using forced exits as needed
- Starting with lower LTV at lower total bonded ETH to mitigate/discourage MEV theft
- Retaining sufficient bond per validator regardless of total stake to mitigate against slashing and abandonment 
- Providing increased capital efficiency with greater bond to encourage an NO to stake as large nodes instead of many small nodes

This work is based on prior work; a copy can be found [here](../assets/rpip-42/bond_curves.md).

## Specification
- The oDAO SHALL be able to penalize stake at the node level when a [Penalizable offense](#penalizable-offenses) is committed
- Legacy minipool deposits SHOULD be disabled
- Node Operators SHALL NOT be required to stake RPL in order to create validators within their megapool
- When Node Operators create validators:
  - If fewer than `base_num` validators already exist, the required `user_deposit` is `base_bond` per validator.
  - If at least `base_num` validators already exist, the required `user_deposit` is `reduced_bond` per validator.
- When Node Operators remove validators:
  - If greater than `base_num` validators exist prior to removing, the Node Operator share before penalties is `reduced_bond`.
  - If at most `base_num` validators exist prior to removing, the Node Operator share before penalties is `base_bond`.
- When a validator is added with a `base_bond` deposit, it SHALL receive priority treatment for any queues vis-à-vis validators added with a `reduced_bond` deposit.
- Bulk validator creation / removal functions SHALL behave the same as multiple individual transactions.
- It SHALL be possible to exit the node operator queue and receive ETH `credit` for it
- If an NO has more total bonded ETH in their megapool than would be necessary based on the current settings (eg, `reduced_bond` is reduced), it SHALL be possible to reduce their bonded ETH and receive ETH `credit` for it
- `credit` MUST be usable to create validators in a megapool
- If (a) the deposit pool is below 10% capacity or (b) `credit` is less than the `required_deposit` if the user were to create a new validator: `credit` MUST be usable to mint rETH to the NO's primary withdrawal address
- The initial settings SHALL be:
  - `base_num`: 2
  - `base_bond`: 4 ETH
  - `reduced_bond`: 4 ETH
- Once RPIP-44 is implemented, `reduced_bond` SHALL be updated to 1.5 ETH

## Penalizable offenses
This portion of the RPIP SHALL be considered Living. It may be updated by DAO vote.

| Offense   | Penalty              | Added      | Updated    |
|-----------|----------------------|------------|------------|
| MEV theft | theft size + 0.2 ETH | 2024-03-29 | 2024-03-29 |

## Rationale
- Bond sizes were chosen per [prior work](../assets/rpip-42/bond_curves.md).
  - `base_bond` is chosen to "sufficiently" dissuade MEV theft as a strategy
  - `reduced_bond` is chosen to "sufficiently" guard against slashing or abandonment risks
- Validators with `base_bond` deposits are prioritized to promote decentralization; new or smaller Node Operators can get up to `base_num` validators launched ahead of larger Node Operators adding `reduced_bond` validators.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
