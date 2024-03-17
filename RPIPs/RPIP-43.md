---
rpip: 43
title: Megapools
description: A single contract per node operator that can be used as an ethereum withdrawal address for multiple validators
author: Valdorff (@Valdorff)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-05
requires: eip-4788
---

## Abstract
This proposal drastically reduces the gas to add validators and distribute rewards for them. It does so by creating a single contract that can be used as the Ethereum withdrawal address for any number of validators. In addition, the contract will facilitate the application of node-level penalties, as opposed to the current state where penalties are assigned and processed at the minipool level.

## Specification
- A Node Operator SHALL be able to deploy a single megapool contract
    - This MAY be combined with the deposit of their first minipool
- A Node Operator SHALL be able to add a minipool to their existing megapool contract
- A Node Operator SHALL be able to exit one or more minipools in their megapool contract
- A Node Operator SHALL be able to distribute rewards from all minipools in their megapool at once
  - This MAY be temporarily blocked while minipools are exiting

## Reference Implementation
See <https://github.com/rocket-pool/rocketpool-research/blob/master/Megapools/megapools.md>

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
