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
- The protocol SHALL be able to penalize stake at the node level
- Minipool deposits SHALL be disabled
- Node Operators SHALL NOT be required to stake RPL in order to create validators within their megapool
- Node Operators with <`base_num` base validators SHALL be able to make a base validator within their megapool
  - A base validator SHALL have a `base_bond` bond from the Node Operator
  - If there is a node operator queue, base validators SHALL be given priority over satellite validators
- Node Operators with >=`base_num` base validators SHALL be able to make a satellite validator
  - A satellite validator SHALL have a `satellite_bond` bond from the Node Operator
- If a Node has satellite validators active while that Node has <`base_num` base validators active, funds SHALL NOT be withdrawable by the Node Operator
  - It is NOT RECOMMENDED to exit base validators unless all satellite validators have been exited
  - A user MAY exit satellite validaors to allow for fund withdrawal 
  - The protocol MAY or MAY NOT provide a path to upgrade a satellite validator to a base validator (to allow for fund withdrawal)
- The initial settings SHALL be:
  - `base_num`: 2
  - `base_bond`: 4 ETH
  - `satellite_bond`: 1.5 ETH


## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
