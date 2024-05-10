---
rpip: 44
title: Integrating Execution Layer Triggerable Exits
description: Describes how execution layer triggerable exits (per EIP-7002) will be used within Rocket Pool.
author: Valdorff (@Valdorff), Mike Leach (@VVander)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-05
requires: 42, 43, eip-7002
tags: tokenomics-2024, tokenomics-content
---

## Abstract
This proposal specifies when [execution layer triggerable exits as defined in EIP-7002](https://eips.ethereum.org/EIPS/eip-7002) will be used by RP contracts. There are two enumerated use cases. The first is that the Node operator may freely request exits of their validators. The second is a keeper mechanism that allows and incentivizes any user to forcibly exit a Node Operator's validators if that Node Operator owes the protocol a threshold amount. This helps keep rETH performant and minimizes funds lost to theft.

There is at least one additional use case not addressed in this RPIP, which is to exit badly-performing (eg, abandoned) validators. It is likely the pDAO will wish to supersede this RPIP to include that functionality. However, as it requires modeling and is not a critical component of the tokenomics rework, it is not being addressed at this time. For similar reasons, this RPIP does not include a keeper-based design to reward those that trigger forced exits. Please see a [historical version of RPIP-44](https://github.com/rocket-pool/RPIPs/blob/09d445accaa77f355acae1e943910ad0229a1d2e/RPIPs/RPIP-44.md) for initial but incomplete ideas to address abandonment and a keeper network.

## Specification

This specification extends the specification of megapools in
[RPIP-43](RPIP-43.md) with a facility for removal of validators from the
megapool to be done by parties other than the Node Operator.

Funds (ETH) associated with a megapool SHALL be accounted in (at least) the following categories:
- Credits (`credit`): Funds that can be used towards validator deposits; sources include but are not limited to deposits from the Node Operator, ETH staked on behalf of the node ([RPIP-32](RPIP-32.md)), and ETH already staked on the beacon chain for a migrating validator
- Deposits (`deposited`): Funds initially received as credits that are currently staked on the beacon chain for validators associated with this megapool
- Withdrawals (`withdrawn`): Funds received into the megapool via withdrawals from the beacon chain, including both principal and the node's share of rewards, NOT including pool stakers' share of the rewards
- Penalties (`debt`): Funds lost to penalties as enumerated in [RPIP-42](RPIP-42.md), or repaid from other balances as specified in this RPIP

The following quantity, `deficit`, is derived from the categorised funds above:
```math
$$ \mathtt{deficit} = \mathtt{credit} + \mathtt{withdrawn} - \mathtt{debt} $$
```

We also introduce a protocol-wide parameter `exit_deficit`, with initial setting
```math
$$ \mathtt{exit\_deficit} = 0.2\;\mathrm{ETH} $$
```

The exit functionality for a megapool is specified as follows:
- A megapool contract SHALL include an `exit` function
  - This function SHALL accept one or more public keys for validators associated with this megapool that have exited the beacon chain
  - This function SHALL either remove the specified validators from the megapool or revert
  - This function MAY be freely called by the Node Operator (as implied by [RPIP-43](RPIP-43.md))
  - This function MAY be called by any account under the condition: `deficit >= exit_deficit`
      - It MUST NOT be possible for accounts other than the Node Operator to exit more validators than needed to reduce `deficit` below `exit_deficit`
- The protocol SHALL use the `withdrawn` and `credit` balances to decrease `debt` prior to taking action on exits
- The protocol SHOULD use staked RPL to decrease `debt` by the corresponding amount prior to taking action on exits

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
