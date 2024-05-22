---
rpip: #<to be assigned>
title: Node Distributor Balance Tracking
description: Guarantee that anyone can distribute ETH in Node Distributors.
discussions-to: <URL>
status: Draft
type: Protocol
category (*only required for Protocol ): Core
created: 2024-05-20
---


## Abstract
A proposal to guarantee that anyone can distribute ETH in Node Distributors (where execution layer rewards for people not in the smoothing pool go). If a withdrawal address is not able to accept the share belonging to the node operator, the value is stored in their balance and only the share belonging to rETH is sent. The node operator can then claim an outstanding balance at a later time.

## Motivation
Currently, anyone can call distribution on a Node Distributor. It attempts to send the node operator's share to their withdrawal address and if that fails the entire distribution including sending of rETH share fails. For example, a node operator could set their withdrawal address to a smart contract that doesn't accept ETH or only accepts it when they are personally initiating the distribution. A profit motive for node operators to do this exists when rETH is traded at a discount. Ensuring that everyone can distribute would help support the rETH peg through better arbitrage and better match expectation that anyone can distribute. 


## Specification
### Distribute
- If the withdrawal address cannot accept the node share, it MUST be added to their balance 
- The user share MUST be transferred to rETH regardless

### Claiming Outstanding ETH
- The node operator MUST be able to claim any outstanding balance


## Backwards Compatibility
The Node Distributor Delegate can be upgraded, there are no backwards compatibility concerns with this proposal.


## Security Considerations
The design attempts to keep changes minimal while achieving the stated goal. The same approach is already used with the Merkle Distributor.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
