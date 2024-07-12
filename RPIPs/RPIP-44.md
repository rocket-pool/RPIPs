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

## Motivation

Execution Layer Triggerable Exits are motivated by the need to combat MEV theft by malicious Rocket Pool validators. Currently, the protocol has limited recourse against such offending validators and the proposed functionality helps to change this. It's important to continue to pursue such improvements to ensure rETH delivers the advertised APY and competes favorably with other liquid staking tokens. 

A secondary motivation is to improve the Node Operator user experience, the ability to request exit of their validators via the protocol is an improvement to the current experience.

This RPIP is part of a set of proposals motivated by a desire to rework Rocket Pool's tokenomics to ensure the protocol’s continued value, development, and longevity. For more details, see the supporting documentation [here](../tokenomics-explainers/001-why-rework). 

## Specification

This specification extends the specification of megapools in
[RPIP-43](RPIP-43.md) with a facility for removal of validators from the
megapool to be done by parties other than the Node Operator.

Funds (ETH) associated with a megapool SHALL be accounted in (at least) the following categories:
- Received Funds (`received`): Funds provided that can be used towards validator deposits; sources include deposits from the Node Operator and ETH staked on behalf of the node ([RPIP-32](RPIP-32.md))
- Credits (`credit`): Funds already in the protocol that can be used towards validator deposits; sources include ETH already staked on the beacon chain for a migrating validator
- Deposits (`deposited`): Funds currently staked on the beacon chain for validators associated with this megapool
- Withdrawals (`withdrawn`): Funds received into the megapool via withdrawals from the beacon chain, including both principal and the node's share of rewards, NOT including pool stakers' share of the rewards
- Debt (`debt`): Penalties or shortfall from exited validators (see [RPIP-43](RPIP-43.md))
  
The following quantity, `deficit`, is derived from the categorised funds above:
```math
$$ \mathtt{deficit} = \mathtt{debt} - \mathtt{received} - \mathtt{credit} -  \mathtt{withdrawn} $$
```

We also introduce a protocol-wide parameter `exit_deficit`, with initial setting
```math
$$ \mathtt{exit\_deficit} = 0.2\;\mathrm{ETH} $$
```

The exit functionality for a megapool is specified as follows:
- A megapool contract SHALL include an `exit` function
  - This function SHALL accept one or more public keys for validators associated with this megapool
  - This function SHALL either remove the specified validators from the megapool or revert
      - If any of the validators are still active on the beacon chain a successful call of this
        function MUST also exit those validators from the beacon chain
  - The Node Operator SHALL be able to call this function (as implied by [RPIP-43](RPIP-43.md))
  - This function MUST be permissionlessly callable under the condition: `deficit >= exit_deficit`
      - It MUST NOT be possible for accounts other than the Node Operator to exit more validators than needed to reduce `deficit` below `exit_deficit`
- The protocol SHALL use the `withdrawn`, `credit`, and `received` balances to pay off `debt` prior to taking action on exits
- The protocol MAY use staked RPL to decrease `debt` by the corresponding amount prior to taking action on exits

## Rationale
The accounting categories are an attempt to simplify the tracking of value within a megapool, as well as calculating ownership.

The permissioned exit functionality is meant as a simple convenience. The permissionless exit functionality is meant to avoid ongoing losses to rETH in cases of negligence or maliciousness.

## Security Considerations
- A misbehaving oDAO gains the ability to force exit any validator by applying penalties to increase `deficit` beyond `exit_deficit`. Alongside a malicious protocol upgrade, this would allow for control of all principal.
  - The penalty application can be limited, eg, by using a maximum rate as suggested in [RPIP-58](RPIP-58.md); however, [RPIP-42](RPIP-42.md) introduces megapool-level penalties which can have arbitrary size. As a result, a new guardrail is included in RPIP-42 to limit penalty application.
  - The malicious upgrade risk can be reduced, eg, via security council veto as proposed in [RPIP-60](RPIP-60.md)
- If a bad actor gains control of a node operator key, they are now able to exit validators
  - This doesn't result in the loss of funds (assuming the NO has set a separate withdrawal address as recommended), so is not too problematic
  - In many cases, the validator keys are derived from the node operator key; insofar as that's true, the bad actor would already have this power
- On the positive side, rETH funds can be better protected by reclaiming funds sooner (vs waiting until the node operator opts to exit, if they ever do)

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
