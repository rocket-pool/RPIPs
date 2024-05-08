---
rpip: 43
title: Megapools
description: A single contract per node operator that can be used as an Ethereum withdrawal address for multiple validators. Also allows for ETH-only node operation.
author: Valdorff (@Valdorff)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-05
requires: eip-4788
tags: tokenomics-2024, tokenomics-content
---

## Abstract
This proposal drastically reduces the gas to add validators and distribute rewards for them. It does so by creating a single contract that can be used as the Ethereum withdrawal address for any number of validators. In addition, the contract will facilitate the application of node-level penalties, as opposed to the current state where penalties are assigned and processed at the minipool level. Note that this proposal also enables ETH-only Node operation.

## Specification

### Megapool Contracts

This specification introduces a type of smart contract called a "megapool" that
handles Beacon Chain withdrawals for Rocket Pool validators deposited after
this RPIP is implemented.

- A Node Operator SHALL be able to deploy at most one megapool contract
    - This MAY be combined with the deposit of their first validator
- Megapool contracts MUST be upgradable using a proxy pattern; a megapool's
  proxy implementation contract SHALL be called the megapool's "delegate"
- Legacy minipool deposits (ie, deposits of validators with withdrawal
  credentials that are not a megapool) SHALL be disabled
    - This SHOULD be achieved in a way that prevents creation of minipools

### Validator Management

Megapools keep track of the set of validators associated with them and the
status of each validator. The possible statuses for validators associated with
megapools SHALL include at least the following:
  - Deposited: a Node Operator has added this validator to their megapool
  - Dissolved: this validator's withdrawal credentials have been discovered to
               be incorrect (ie, not this megapool)

Node operators can manage the set of validators in their megapool:

- A Node Operator SHALL be able to add new validators to their megapool
- A Node Operator SHALL be able to migrate existing validators to their megapool
- New and existing validators' withdrawal credentials MUST be checked to
  correctly refer to the megapool address, with the validator status set to
  Deposited or Dissolved accordingly
     - The check MAY occur after the validator is added initially as Deposited
- A Node Operator SHALL be able to remove exited validators from their megapool

### Funds Management

This subsection needs some work. I think it should say who can trigger
distribution of funds out of the megapool, whether those funds are pending
deposits, rewards, or remaining principal. This is a non-trivial question since
funds do not necessarily belong to the node operator. There could be a link out
to another RPIP with the details.

- A Node Operator SHALL be able to distribute rewards from all validators in their megapool at once
  - This MAY be temporarily blocked while validators are exiting or pending removal

### RPL Staking

This subsection also needs a little work: clarify who a "user" is, clarify
whether RPL "staked to a megapool" actually requires the RPL to be in the
megapool balance or whether that is unspecified (eg, could be stored in Rocket
Vault but accounted to the node), and preface with the two states (Staked and
Unstaking) that portions of the RPL can be in.

- RPL MAY be staked to a megapool
  - Validators MAY be added to a megapool regardless of RPL staked to the megapool (note that this means ETH-only node operation is possible)
- Legacy RPL staking (ie staking RPL to a node rather than to a megapool) SHALL be disabled
- A user MAY set any amount of RPL "Staked" to a megapool to "Unstaking"
- Once RPL has been "Unstaking" for `unstaking_period` days, it can be withdrawn (this setting is defined in [RPIP-30](RPIP-30.md))
- All RPL that was "Unstaking" MUST be withdrawn at one time
- There SHALL only be a single time stored for "Unstaking" RPL; this means that if a user sets
  additional RPL to "Unstaking" after some RPL is already "Unstaking", they must wait for
  `unstaking_period` before any is available to withdraw
  - All RPL that was already "Unstaking" for `unstaking_period` SHOULD be withdrawn when a user
    sets additional RPL to "Unstaking"

### Vote Eligibility

- All nodes are eligible to receive vote power
- Total RPL staked (to either a megapool or the node) SHALL contribute to vote power up to 150% of the value of total bonded ETH (from both megapools and legacy minipools)
  - Note that "Unstaking" RPL SHALL NOT be counted
  - If this RPIP is ratified, a note SHALL be added to the "Eligibility" section of [RPIP-4](./RPIP-4.md) saying that it has been superseded by this section

## Reference Implementation
See <https://github.com/rocket-pool/rocketpool-research/blob/master/Megapools/megapools.md>

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
