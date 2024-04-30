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
tags: tokenomics-2024
---

## Abstract
This proposal drastically reduces the gas to add validators and distribute rewards for them. It does so by creating a single contract that can be used as the Ethereum withdrawal address for any number of validators. In addition, the contract will facilitate the application of node-level penalties, as opposed to the current state where penalties are assigned and processed at the minipool level.

## Specification
- A Node Operator SHALL be able to deploy a single megapool contract
    - This MAY be combined with the deposit of their first validator
- A Node Operator SHALL be able to add a validator to their existing megapool contract
- A Node Operator SHALL be able to exit one or more validator in their megapool contract
- A Node Operator SHALL be able to distribute rewards from all validator in their megapool at once
  - This MAY be temporarily blocked while validators are exiting
- RPL MAY be staked to a megapool
  - Validators MAY be created within a megapool regardless of RPL staked to the megapool
- A user MAY set any amount of RPL "Staked" to a megapool to "Unstaking"
- Once RPL has been "Unstaking" for `unstaking_period` days, it can be withdrawn (this setting is defined in [RPIP-30](RPIP-30.md))
- All RPL that was "Unstaking" MUST be withdrawn at one time
- There SHALL only be a single time stored for "Unstaking" RPL; this means that if a user sets
  additional RPL to "Unstaking" after some RPL is already "Unstaking", they must wait for
  `unstaking_period` before any is available to withdraw
  - All RPL that was already "Unstaking" for `unstaking_period` SHOULD be withdrawn when a user
    sets additional RPL to "Unstaking"

### Vote Eligibility
- All nodes are eligible to receive vote power.
- For nodes eligible to receive vote power, total RPL staked (to either a megapool or the node) SHALL contribute to vote power up to 150% of the value of total bonded ETH (from both megapools and legacy minipools).
  - Note that "Unstaking" RPL SHALL NOT be counted
  - If this RPIP is ratified, a note SHALL be added to the "Eligibility" section of [RPIP-4](./RPIP-4.md) saying that it has been superceded by this section

## Reference Implementation
See <https://github.com/rocket-pool/rocketpool-research/blob/master/Megapools/megapools.md>

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
