---
rpip:
title: Bond Curves
description: Dramatically increase capital efficiency by allowing smaller bonds, especially as an NO has more total bonded ETH on their node
author: Valdorff (@Valdorff)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-08
---

## Abstract
This proposal dramatically increases the LTV used in the protocol (loan to value; the amount of ETH that can be borrowed per unit of bonded ETH). It does this safely by:
- Enabling node-level penalties to mitigate/discourage MEV theft
- Using forced exits as needed
- Starting with lower LTV at lower total bonded ETH to mitigate/discourage MEV theft
- Retaining sufficient bond per validator regardless of total stake to mitigate against slashing and abandonment 
- Providing capital efficiency with greater bond to encourage an NO to stake as large nodes instead of many small nodes

## Specification
- The protocol SHALL be able to penalize stake at the node level
- Node Operators with <`trunk_pool_num` trunk pools SHALL be able to make a trunk pool
  - A trunk pool SHALL have a `trunk_pool_bond` bond from the Node Operator
- Node Operators with >=`trunk_pool_num` trunk pools SHALL be able to make a branch pool
  - A branch pool SHALL have a `branch_pool_bond` bond from the Node Operator
- If a Node has branch pools active while that Node has <`trunk_pool_num` trunk pools active, funds SHALL NOT be withdrawable by the Node Operator
  - It is NOT RECOMMENDED to exit trunk pools unless all branch pools have been exited
  - A user MAY exit branch pools to allow for fund withdrawal 
  - The protocol MAY or MAY NOT provide a path to upgrade a branch pool to a trunk pool (to allow for fund withdrawal)
- The initial settings SHALL be:
  - `trunk_pool_num`: 2
  - `trunk_pool_bond`: 4 ETH
  - `branch_pool_bond`: 1.5 ETH

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
