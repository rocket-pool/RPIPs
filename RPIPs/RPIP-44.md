---
rpip: 44
title: Integrating Execution Layer Triggerable Exits
description: Describes when EL triggerable exits will be used within RP
author: Valdorff (@Valdorff), Mike Leach (@VVander)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-05
requires: eip-7002
tags: tokenomics-2024
---

## Abstract
This proposal specifies when [execution layer triggerable exits as defined in EIP-7002](https://eips.ethereum.org/EIPS/eip-7002)  will be used by RP contracts. There are two enumertated use cases. The first is that the Node operator may freely request exits of their validators. The second is a keeper mechanism that allows and incentives any user to forcibly exit a Node Operator's validators if that Node Operator owes the protocol a threshold amount. This helps keep rETH performant and minimizes funds lost to theft.

There is an additional use case that is not addressed in this RPIP, which is to exit badly-performing (eg, abandoned) validators. It is likely the pDAO will wish to supersede this RPIP in order to include that functionality. However, as it requires modeling and is not a critical function to the work in [RPIP-49](RPIP-49.md), it is not being addressed at this time. For similar reasons, this RPIP does not include a keeper-bsed design to reward those that trigger forced exits. Please see a [historical version of RPIP-44](https://github.com/rocket-pool/RPIPs/blob/09d445accaa77f355acae1e943910ad0229a1d2e/RPIPs/RPIP-44.md) for initial but incomplete ideas to address abandonment and a keeper network.

## Specification
- An `exit` function SHALL be included in the Megapool delegate
  - This function SHALL take in an array of validator public keys
  - This function MAY be freely called by the Node Operator
  - This function MAY be called by any user if the Node has a `deficit` greater than `exit_deficit`
    - It MUST NOT be possible to exit more than the minimum number of validators needed to cover `deficit`
- `debt` is incurred via penalties, as enumerated in [RPIP-42](RPIP-42.md)
- The protocol SHALL use `rewards` and/or `credit` held by the NO to decrease `debt` by the same amount prior to taking action on exits
- The protocol SHOULD use staked RPL held by the NO to decrease `debt` by the corresponding amount prior to taking action on exits
- The initial settings SHALL be:
  - `exit_deficit`: 0.2 ETH

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
