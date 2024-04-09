---
rpip: #<to be assigned>
title: Rewards Tree Specification
description: Specifies how RPL Inflation and Smoothing Pool rewards are allocated to node operators
author: Ramana Kumar (@xrchz), Joe Clapis (@jcrtp)
discussions-to: https://github.com/rocket-pool/rocketpool-research/issues
status: Draft
type: Protocol
category: Core
created: 2024-04-09
requires: 1
---

## Abstract
This RPIP specifies the Rocket Pool rewards system (first introduced in [Redstone](https://medium.com/rocket-pool/rocket-pool-the-merge-redstone-601d9efd6b4)) for node operators, wherein ETH and RPL rewards are allocated at regular intervals to staking nodes.

## Motivation
Rocket Pool's allocation of rewards to node operators needs a clear specification that can be independently implemented and verified so that node operators know what to expect and have a basis for recourse in case of any misallocation.

The rewards tree specification has previously been developed in the external [Rocket Pool Research](https://github.com/rocket-pool/rocketpool-research/tree/master/Merkle%20Rewards%20System) repository. Development began before the RPIP process was created.
However, since this is a core protocol specification, its proper home is as an RPIP where it can be easily found and referred to with a clear status as an official definition of (part of) the Rocket Pool protocol.

## Specification
The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

TODO: Import the technical specification from [the research repository](https://github.com/rocket-pool/rocketpool-research/tree/master/Merkle%20Rewards%20System), possibly by copying the markdown (and image) files there as assets attached to this RPIP.

## Rationale
TODO

## Backwards Compatibility
N/A

## Test Cases
TODO: include some example tree roots for partial intervals?

## Reference Implementation
This specification is implemented by [Treegen](https://github.com/rocket-pool/treegen/tree/a92de2f75e8188c450653b01af5ab27f97a14ec3), which is the reference implementation. It is also implemented by [Rockettree](https://github.com/xrchz/rockettree/tree/ce9bca3d15ce490804ea8e31ebfd5a24723dc8cf).

## Security Considerations
Errors, inconsistencies, and omissions in this specification leave the protocol in a difficult position should there arise any disputes over the allocation of rewards.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
