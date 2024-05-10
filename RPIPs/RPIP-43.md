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
requires: 30, eip-4788
tags: tokenomics-2024, tokenomics-content
---

## Abstract
This proposal drastically reduces the gas to add validators and distribute rewards for them. It does so by creating a single contract that can be used as the Ethereum withdrawal address for any number of validators. In addition, the contract will facilitate the application of node-level penalties, as opposed to the current state where penalties are assigned and processed at the minipool level. Note that this proposal also enables ETH-only Node operation.

## Specification

### Megapool Contracts

This specification introduces a type of smart contract called a "megapool" that
serves as the target of Beacon Chain withdrawal credentials for Rocket Pool
validators deposited after this RPIP is implemented.

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

This RPIP includes a rework of the treatment of RPL staked on Rocket Pool
nodes. We refer to RPL staked for a node prior to the implementation of this
RPIP as "legacy staked RPL", and to RPL staked according to the rules described
in this RPIP as "megapool staked RPL".

Megapool staked RPL (in the protocol, ie, before being withdrawn) can be in one
of two states:
- Staked: the RPL has been deposited and staked (locked) in the protocol, for a
          node with a megapool deployed
- Unstaking: the RPL has been marked as intended to be withdrawn

Rules specifying the movement of staked RPL are as follows:

- Legacy RPL staking (ie, increasing a node's legacy staked RPL balance) SHALL
  be disabled
- There is no staked RPL requirement for adding validators to a megapool
- A Node Operator MAY change the state of any amount of megapool staked RPL
  associated with their node from Staked to Unstaking at any time
    - The time at which Staked RPL was last changed to Unstaking RPL is called the
      "last unstake time" for the node
- Only Unstaking RPL can be withdrawn from the protocol, and only if the last
  unstake time for the node is at least `unstaking_period` (this parameter is
  defined in [RPIP-30](RPIP-30.md))
    - This does not have any impact on the extant rules governing the
      withdrawal of legacy staked RPL
- If any Unstaking RPL is withdrawn for a node, all Unstaking RPL for that node
  MUST be withdrawn at once
- If the last unstake time is at least `unstaking_period` when additional
  megapool staked RPL is having its state changed to Unstaking, the RPL already
  in the Unstaking state SHOULD be withdrawn first

### Vote Eligibility

- All nodes are eligible to receive vote power
- For nodes eligible to receive vote power, the sum of legacy staked RPL and
  megapool staked RPL in the Staked state SHALL contribute to vote power up to
  150% of the value of total bonded ETH (from both megapools and legacy
  minipools)
  - Note that Unstaking RPL SHALL NOT be counted
  - If this RPIP is ratified, a note SHALL be added to the "Eligibility" section of [RPIP-4](./RPIP-4.md) saying that it has been superseded by this section

## Reference Implementation
See <https://github.com/rocket-pool/rocketpool-research/blob/master/Megapools/megapools.md>

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
