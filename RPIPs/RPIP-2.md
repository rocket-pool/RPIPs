---
rpip: 2
title: Rocket Pool Design Specification
status: Draft
type: Informational
author: Mike Leach (@Wander) <mike@bamboofin.tech>
discussions-to: https://dao.rocketpool.net/t/rpip-2-formalizing-protocol-changes/367
created: 2022-03-17
---
# Rocket Pool Design Specification

This document is intended as a high level overview of the Rocket Pool project.

## Rocket Pool Project

The Rocket Pool project includes:
- Protocol smart contracts 
- Other smart contracts, such as the standards and reference contracts created via RPRCs
- The Smart Node CLI
- Documentation such as RPIPs

### Protocol

The Rocket Pool core protocol is the set of smart contracts which are necessary for running a Rocket Pool node or swapping ETH for rETH via the deposit pool.

#### rETH
|Network|Address|
|---|---|
| Mainnet | 0xae78736Cd615f374D3085123A210448E74Fc6393 |
| Prater | 0x178E141a0E3b34152f73Ff610437A7bf9B83267A |

_[INSERT rETH CONTRACT DOCUMENTATION]_

#### RPL
|Network|Address|
|---|---|
|Mainnet|0xD33526068D116cE69F19A9ee46F0bd304F21A51f|
|Prater|0x5e932688e81a182e3de211db6544f98b8e4f89c7|

_[INSERT RPL CONTRACT DOCUMENTATION]_

#### OLD RPL
|Network|Address|
|---|---|
|Mainnet|0xb4efd85c19999d84251304bda99e90b92300bd93|
|Prater|0xD672E2F8eeDd0F7Eb020948299642e3c5E85F0A6|

_[INSERT OLD RPL CONTRACT DOCUMENTATION]_

#### Storage
|Network|Address|
|---|---|
|Mainnet|0x1d8f8f00cfa6758d7bE78336684788Fb0ee0Fa46|
|Prater|0xd8Cd47263414aFEca62d6e2a3917d6600abDceB3|

_[INSERT Storage CONTRACT DOCUMENTATION]_

#### [INSERT MORE CONTRACTS]

### Other Contracts

Standards and reference smart contracts which utilize the protocol are proposed via RPRCs and are not considered part of the core protocol.

##### RPIP-3 SaaS Reference Contract
|Network|Address|
|---|---|
|Mainnet|0x1234|
|Prater|0x1234|

_[INSERT Saas RPIP]_

### Smart Node CLI

The smart node is a reference client, written in golang, for interacting with the Rocket Pool protocol: 

- https://github.com/rocket-pool/smartnode-install
- https://github.com/rocket-pool/smartnode

### RPIPs

Rocket Pool Improvement Proposals RPIPs) are the process for documenting and proposing changes to the protocol. See [RPIP-1](./rpip-1.md) for more details.
