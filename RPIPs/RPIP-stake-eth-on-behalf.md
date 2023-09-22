---
rpip: TO BE ASSIGNED
title: Stake ETH on behalf of node
description: Enables an address to provide ETH to a node, allowing the node to make deposits using the provided ETH.
author: Darren Langley (@langers), Kane Wallmann (@kanewallmann)
discussions-to: [TODO]
status: Draft
type: Protocol
category: Core
created: 2023-09-21
---

## Abstract
A proposal to enable an independent address to fund a registered node with ETH. Subsequently, the node can utilise this ETH to make deposits, without having direct access to the ETH.

## Rationale
This supports a variety of situations were the node operator is not directly providing the ETH:
- Enhanced security for node operators, as they can stake directly from their hardware wallet, eliminating the need to transfer funds to the node beforehand
- Staking as a Service providers where custody of funds are managed by a trusted custodian
- Protocol integrations where custody of funds are managed by smart contracts
- DAOs or organisations where custody of funds are managed by a treasury

While the primary aim of this feature is to facilitate single depositor scenarios, it's worth noting that multiple independent depositors can also leverage this capability by creating smart contracts layered on top.

## Other Related RPIPs 
- [**RPIP-3** - SaaS Provider Standard](./RPIP-3.md) 
- [**RPIP-7** - Withdrawal Address Escrow](./RPIP-7.md) 

## Specification
- As a withdrawal address, I MUST be able to fund the corresponding node with ETH that is accounted for separately from the node's funds. 
- As a node operator, I MUST NOT be able to withdraw the funds supplied by the withdrawal address.
- As a node operator, I MUST be able to fund new minipools using the withdrawal address supplied ETH.
- As a withdrawal address, I MUST be able to withdraw unused withdrawal address supplied ETH.
- As a withdrawal address, if I change my withdrawal address, the above MUST hold true for the new address only (the old address loses this ability).

## Backwards Compatibility
The specification as written is backwards compatible with the current deposit process. The behaviour is as current for node operators who do not use the new functionality. 

## Security Considerations
The specification operates under the assumption that the ETH owner is synonymous with the withdrawal address owner. This aligns with the existing trust assumptions within the protocol and serves as a protective measure to guarantee that only the rightful owner can access these funds.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).