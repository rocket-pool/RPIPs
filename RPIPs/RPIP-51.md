---
rpip: 51
title: Current Rewards Tree Specification
description: Specifies how RPL Inflation and Smoothing Pool rewards are allocated to node operators
author: Ramana Kumar (@xrchz), Joe Clapis (@jcrtp)
discussions-to: https://github.com/rocket-pool/rocketpool-research/issues
status: Final
type: Informational
created: 2024-04-09
---

## Abstract
This RPIP describes the current state of the specification of the Rocket Pool rewards system (first introduced in [Redstone](https://medium.com/rocket-pool/rocket-pool-the-merge-redstone-601d9efd6b4)) for node operators, wherein ETH and RPL rewards are allocated at regular intervals to staking nodes.

## Motivation
Rocket Pool's allocation of rewards to node operators needs a clear specification that can be independently implemented and verified so that node operators know what to expect and are able to verify reward allocations.

The rewards tree specification is currently developed in the external [Rocket Pool Research](https://github.com/rocket-pool/rocketpool-research/tree/master/Merkle%20Rewards%20System) repository. Development there began before the RPIP process was created. However, since this is a core protocol specification, its proper home is as an RPIP where it can be easily found and referred to with a clear status as an official definition of (part of) the Rocket Pool protocol. This RPIP is an informational document describing the status quo, with the intention of following up with a future pDAO-ratified RPIP (which may or may not include changes).

## Specification
The technical specification (v8 in particular) taken from the external [research repository](https://github.com/rocket-pool/rocketpool-research/tree/master/Merkle%20Rewards%20System) is attached as the following assets:

* [Merkle Tree Specification](../assets/rpip-51/merkle-tree-spec.md)
* [Rewards Calculation Specification](../assets/rpip-51/rewards-calculation-spec.md)

## Rationale
This specification describes what is actually implemented and used by Rocket Pool as of April 2024 (that is, for rewards interval 21).

## Reference Implementation
This specification (Rewards Calculation v8) is implemented by [Treegen](https://github.com/rocket-pool/treegen/tree/a92de2f75e8188c450653b01af5ab27f97a14ec3), which is the reference implementation. It is also implemented by [Rockettree](https://github.com/xrchz/rockettree/tree/ce9bca3d15ce490804ea8e31ebfd5a24723dc8cf).

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
