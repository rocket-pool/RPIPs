---
rpip: TO BE ASSIGNED
title: RPL Withdrawal Address
description: Adds an additional withdrawal address that facilitates splitting ETH bond and rewards from RPL bond and rewards
author: Darren Langley (@langers), Kane Wallmann (@kanewallmann)
discussions-to: [TODO]
status: Draft
type: Protocol
category: Core
created: 2023-09-05
---

## Abstract
A proposal to add an additional withdrawal address that is the target for RPL bond withdrawals and RPL reward claims. Essentially there will be two withdrawal addresses: one for ETH; and one for RPL. The RPL withdrawal address will default to the ETH withdrawal address. 

## Rationale
This facilitates the splitting of ETH bond/rewards and RPL bond/rewards, allowing different actors to supply each collateral type. This can be achieved either as a direct trusted relationship (i.e whale marriage) between parties, through a staking-as-a-service provider, or through a third-party staking protocol integration.  

The goal of the proposal is to support growth of the node operator set and to utilise RPL that is currently not being staked within the protocol. 

> There is a demand for parties to stake ETH with Rocket Pool without it necessary to take a position in RPL. There is also a demand from parties uninterested with running a Rocket Pool node to be able to loan unstaked RPL to other parties and earn yield on their loaned RPL.

https://dao.rocketpool.net/t/withdrawal-address-splitter-contract/563

## Other related RPIPs 
- [**RPIP-3** - SaaS Provider Standard](./RPIP-3.md) 
- [**RPIP-7** - Withdrawal Address Escrow](./RPIP-7.md) 

## Specification
- The withdrawal address that has existed to date SHALL be known as the primary withdrawal address
- If a node's RPL withdrawal address is set, all RPL rewards and withdrawn RPL will go to the RPL withdrawal address
- If a node's RPL withdrawal address is unset, all RPL rewards and withdrawn RPL will go to the primary withdrawal address
- As a node operator, I MUST be able to set an RPL withdrawal address so that I can direct my RPL bond/rewards
  - If a node's RPL withdrawal address is unset, the change MUST come from the node's primary withdrawal address
  - If a node's RPL withdrawal address is set, the change MUST come from the current RPL withdrawal address
- As a node operator, I MUST be able to confirm the RPL withdrawal address from the new target address before it takes effect, to ensure it is the correct address
- As the owner of the RPL withdrawal address, I MUST be able to unset my RPL withdrawal address
- As the controller of the RPL withdrawn from a node, I MUST be able to call `RocketNodeStaking.withdrawRPL()` to withdraw excess RPL
  - If a node's RPL withdrawal address is unset, the call MUST come from one of: the node's primary withdrawal address, or the node's address
  - If a node's RPL withdrawal address is set, the call MUST come from the current RPL withdrawal address
- The RPL withdrawal address SHALL be initialized as unset for new node operators
- The RPL withdrawal address SHALL be initialized as unset for existing node operators

## Backwards Compatibility
The specification as written is backwards compatible with the current withdrawal address. The behaviour is as current for node operators who do not use the new functionality. 

## Security Considerations
In a situation were the node operator is different to the RPL owner, the RPL owner must rely on the node operator to exit minipools to unlock their RPL stake. A future RPIP *may* address this limitation once execution layer withdrawals are implemented in Ethereum.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).