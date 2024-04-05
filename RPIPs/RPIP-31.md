---
rpip: 31
title: RPL Withdrawal Address
description: Adds an additional withdrawal address that facilitates splitting ETH bond and rewards from RPL bond and rewards
author: Darren Langley (@langers), Kane Wallmann (@kanewallmann)
discussions-to: https://dao.rocketpool.net/t/rpip-31-rpl-withdrawal-address/2251
status: Final 
type: Protocol
category: Core
created: 2023-09-05
vote-to: https://vote.rocketpool.net/#/proposal/0x6f36fa8ee8c077cc8a6bd813fdb261ccf5d84f8fac253a33b6ddebeea73afd40
vote-date: 2023-12-20
vote-result: Passed
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

### Defaults
- The RPL withdrawal address SHALL be initialized as unset for new node operators
- The RPL withdrawal address SHALL be initialized as unset for existing node operators
  
### Rewards & Withdrawn RPL
- If a node's RPL withdrawal address is set, all RPL rewards and withdrawn RPL will go to the RPL withdrawal address
- If a node's RPL withdrawal address is unset, all RPL rewards and withdrawn RPL will go to the primary withdrawal address

### Setting the Address
- As a node operator, I MUST be able to set an RPL withdrawal address so that I can direct my RPL bond/rewards
  - If a node's RPL withdrawal address is unset, the change MUST come from the node's primary withdrawal address
  - If a node's RPL withdrawal address is set, the change MUST come from the current RPL withdrawal address
- As a node operator, I MUST be able to confirm the RPL withdrawal address from the new target address before it takes effect, to ensure it is the correct address
- As the owner of the RPL withdrawal address, I MUST be able to unset my RPL withdrawal address

### Withdrawing RPL
- As the controller of the RPL withdrawn from a node, I MUST be able to call withdraw RPL as allowed by the protocol
  - If a node's RPL withdrawal address is unset, the call MUST come from one of: the node's primary withdrawal address, or the node's address
  - If a node's RPL withdrawal address is set, the call MUST come from the current RPL withdrawal address

### Claiming RPL rewards
- As the controller of the RPL for a node, I MUST be able to trigger a claim of RPL rewards
  - If a node's RPL withdrawal address is unset, the call MUST come from one of: the node's primary withdrawal address, or the node's address
  - If a node's RPL withdrawal address is set, the call MUST come from one of: the node's primary withdrawal address, the current RPL withdrawal address, or the node's address
- As the controller of the RPL for a node, I MUST be able to trigger a claim of RPL rewards and restake a portion
  - If a node's RPL withdrawal address is unset, the call MUST come from one of: the node's primary withdrawal address, or the node's address
  - If a node's RPL withdrawal address is set, the call MUST come from the current RPL withdrawal address
- *If a node operator is in the smoothing pool a claim of RPL will also distribute ETH rewards*

### Staking RPL
- As the controller of the RPL for a node, I MUST be able to stake additional RPL
  - If a node's RPL withdrawal address is unset, the call MUST come from the node's address, primary withdrawal address, or a specifically allowed address
  - If a node's RPL withdrawal address is set, the call MUST come from the current RPL withdrawal address, or a specifically allowed address
- As the controller of the RPL for a node, I MUST be able to control the list of specifically allowed RPL staking addresses
  - If a node's RPL withdrawal address is unset, the call MUST come from the node's address
  - If a node's RPL withdrawal address is set, the call MUST come from the current RPL withdrawal address

## Backwards Compatibility
The specification as written is backwards compatible with the current withdrawal address. The behaviour is as current for node operators who do not use the new functionality.

## Security Considerations
In a situation were the node operator is different to the RPL owner, the RPL owner must rely on the node operator to exit minipools to unlock their RPL stake. A future RPIP *may* address this limitation once execution layer withdrawals are implemented in Ethereum.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
