---
rpip: 32
title: Stake ETH on behalf of node
description: Enables an address to provide ETH to a node, allowing the node to make deposits using the provided ETH.
author: Darren Langley (@langers), Kane Wallmann (@kanewallmann)
discussions-to: https://dao.rocketpool.net/t/rpip-32-stake-eth-on-behalf-of-node/2252
status: Final
type: Protocol
category: Core
created: 2023-09-21
vote-to: https://vote.rocketpool.net/#/proposal/0xae1f102e7953322f31e07ee7512dd1fa4f0e1f1f9cf20643689c35274ec41516
vote-date: 2023-12-20
vote-result: Passed
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
- [RPIP-TBD - RPL Withdrawal Address](./RPIP-31.md)
  - If the `RPL withdrawal address` RPIP is ratified, note that withdrawal address in this RPIP's Specification refers to the "primary withdrawal address". In this case, this line should be removed and the wording updated in the Specification.

## Specification
- As someone who wants to supply ETH to a node for staking, I MUST be able to fund a specific node with ETH from any address
- As someone who wants to supply ETH to a node for staking, I want the supplied ETH to be accounted for separately from the node's own funds
- As the owner of a node's withdrawal address, I MUST be able to withdraw unused supplied ETH
- As a node operator, I MUST NOT be able to withdraw the ETH funds supplied by another address
- As a node operator, I MUST be able to fund new minipools using the supplied ETH

## Backwards Compatibility
The specification as written is backwards compatible with the current deposit process. The behaviour is as current for node operators who do not use the new functionality.

## Security Considerations
The specification operates under the assumption that the ETH owner is synonymous with the withdrawal address owner. This aligns with the existing trust assumptions within the protocol and serves as a protective measure to guarantee that only the rightful owner can access these funds.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
