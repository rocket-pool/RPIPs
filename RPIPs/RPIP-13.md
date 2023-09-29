---
rpip: 13
title: Supernodes (SaaS)
description: A native Staking-as-a-Service platform
author: Kane Wallmann (@kanewallmann)
discussions-to: TODO
status: Draft
type: Protocol
category: Core
created: 2023-09-01
---

## Abstract

The SaaS system is an addon to Rocket Pool to support the creation of Staking-as-a-Service products on top of the protocol. The SaaS system will
introduce the new concept of a "supernode" which is a special type of node operator that is controlled via a smart contract. Supernodes will
allow 4 entities to come together to fulfil the requirements of running a set of Rocket Pool minipools. Those are: 

- Supernode operator who is the controlling entity for the SaaS business
- RPL capital providers who provide the RPL collateral requirement
- ETH capital providers who provide the 16 ETH per minipool requirement
- Node operators who take care of the actual running of a validator

## Motivation

The purpose of introducing this addon is to increase the supply side (Node Operators) within the Rocket Pool protocol by offering a trust-minimised
platform from which SaaS providers can build on top of.

## Specification
TODO

## Rationale
TODO

## Backwards Compatibility
This design sits on top of the existing protocol and does not require any changes to the underlying protocol. therefore, there are no backwards
compatibility concerns. However, future changes to the protocol might need to consider what affect they could have on supernodes.

## Test Cases
TODO

## Reference Implementation
TODO

## Security Considerations
As an addon to the base layer, there are no additional security concerns regarding the protocol. As with any new smart contracts,
they should be audited by 2 major auditing firms, go through beta testing, and be added to the bug bounty program.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
