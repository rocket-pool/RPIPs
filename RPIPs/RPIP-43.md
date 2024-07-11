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
requires: 30, 46, eip-4788
tags: tokenomics-2024, tokenomics-content
---

## Abstract
This proposal drastically reduces the gas to add validators and distribute rewards for them. It does so by creating a single contract that can be used as the Ethereum withdrawal address for any number of validators. In addition, the contract will facilitate the application of megapool-level penalties, as opposed to the current state where penalties are assigned and processed at the minipool level. Note that this proposal also enables ETH-only Node operation.

## Motivation

Megapools are motivated by several desires; to lower ongoing costs, to lower the barrier to entry, and to improve penalty handling across what would currently be multiple minipools. Both ongoing and upfront costs are reduced by Megapools due to the efficiency savings achieved by using a single smart contract over multiple minipool contracts. The accounting and processing of penalties is easier to handle with a single contract. 

ETH-only node operation is motivated by a desire to support growth in the demand for rETH and further lower the barrier to entry to Rocket Pool.

This RPIP is part of a set of proposals motivated by a desire to rework Rocket Pool's tokenomics to ensure the protocol’s continued value, development, and longevity. For more details, see the supporting documentation [here](../tokenomics-explainers/001-why-rework). 

## Specification

### Megapool Contracts

This specification introduces a type of smart contract called a "megapool" that
SHALL serve as the target of Beacon Chain withdrawal credentials for Rocket Pool
validators deposited after this RPIP is implemented.

- A Node Operator SHALL be able to deploy at most one megapool contract
    - This MAY be combined with the deposit of their first validator
- Megapool contracts MUST be upgradable using a proxy pattern; a megapool's
  proxy implementation contract SHALL be called the megapool's "delegate"
- Legacy minipool deposits (ie, deposits of validators with withdrawal
  credentials that are not a megapool) SHALL be disabled
- Solo migration into legacy minipools (ie, promotion of vacant minipools)
  SHALL be disabled; this MAY be achieved by disabling creation of vacant
  minipools

### Validator Management

Megapools keep track of the set of validators associated with them and the
status of each validator. The possible statuses for validators associated with
megapools SHALL include at least the following:
  - Prestaked: a Node Operator has added this validator to their megapool, but
               the validator has not yet had its full activation balance
               deposited to the beacon chain
  - Staked:    this validator has been added to the megapool and is in the
               `pending_queued` or later state on the beacon chain
  - Dissolved: this validator's withdrawal credentials have been discovered to
               be incorrect (ie, not this megapool)

Node operators can manage the set of validators in their megapool:

- A Node Operator SHALL be able to add new validators to their megapool
- A Node Operator MAY be able to migrate existing validators to their megapool
     - Note that this would involve updating the validators' BLS withdrawal
       credentials to the megapool address
- New and existing validators' withdrawal credentials MUST be checked to
  correctly refer to the megapool address, with the validator status set to
  Dissolved if the check fails
     - The check MAY occur after the validator is added initially as Prestaked
- A Node Operator SHALL be able to remove exited validators from their megapool

### `debt` Variable
- There SHALL be a `debt` variable that is initially set to 0
- The oDAO SHALL be able to increase `debt` to apply penalties (see [RPIP-42](./RPIP-42.md/#penalizable-offenses)) by majority vote
  - If this causes the megapool to meet the criteria for a non-Node Operator initiated exit (see [RPIP-44](RPIP-44.md)), it is RECOMMENDED that an exit is initiated by the same transaction 
- There SHALL be a function to pay off `debt` with ETH. The ETH SHALL be sent to the rETH contract and the `debt` SHALL be reduced by the amount


### Funds Management

In the following, "capital" refers to funds supplied for staking by either the
node operator or pool stakers, including node operator staked RPL. "Rewards"
refers to additional funds generated or obtained via staking, including
consensus rewards, execution rewards, and RPL inflation. The split of rewards
into shares is defined in [RPIP-46](RPIP-46.md).

- This RPIP assumes the continued use of separate 32 Ether validators. Support for
  [EIP-7251](https://eips.ethereum.org/EIPS/eip-7251) 'Max EB' validators will require additional specification and development.
- The megapool SHALL track the total capital ownership of the validator(s) between
  the node operator and the rETH stakers.
- The megapool MUST keep track of the capital funds present in the contract.
  The megapool MUST assume that any other funds present are rewards.
- A Node Operator SHALL be able to distribute rewards from all validators in their
  megapool at once.
- Rewards distribution MAY be temporarily blocked while validators are exiting or
  pending removal.
- There SHALL be a reward distribution function in the megapool
  - For this section, we define `borrowed_portion` as the megapool's `borrowed_eth / (bonded_eth + borrowed_eth)`
  - When called, `reth_share * borrowed_portion` of rewards SHALL be sent to the rETH contract
  - When called, `voter_share * borrowed_portion` of rewards SHALL be sent to a merkle rewards
    distributor contract
  - When called, `surplus_share * borrowed_portion` of rewards SHALL be sent to the appropriate
    surplus disposition contract
  - If `debt` exists when called, the remaining rewards SHALL first be used to pay off `debt`
  - When called, any remaining rewards SHALL then be held in the megapool as unclaimed node operator funds 
  - This function SHALL be permissionless
  - If called by the node operator, this function SHOULD claim all unclaimed node operator funds
- There SHALL be a capital distribution function in the megapool
  - When called, capital borrowed from the protocol that has been released from
    exited validators SHALL be sent to the rETH contract.
    - If the capital is insufficient to repay the protocol, the shortfall SHALL be added to `debt`
  - When called while the megapool has `debt`, the remaining capital from exited validators SHALL first be used to pay off `debt`
  - When called, the remaining capital SHALL then be held in the megapool as unclaimed node operator funds
  - This function SHALL support permissionless use, with additional restrictions:
    - A permissionless `startUserDistribute` function SHALL record the time when called, as long as there is no previously recorded time _or_ the previously recorded time was longer than `minipool.user.distribute.window.start + minipool.user.distribute.window.length` ago
    - The capital distribution function may be called permissionlessly if `minipool.user.distribute.window.start` has passed since the recorded `startUserDistribute` call, but `minipool.user.distribute.window.start + minipool.user.distribute.window.length` has not yet passed since the recorded `startUserDistribute` call
  - If called by the node operator
    - This function SHOULD claim all unclaimed node operator funds
    - This function SHALL be immediately callable without delay
- There SHALL be a function to claim unclaimed node operator funds; when called
  - If `debt` exists, unclaimed node operator funds SHALL first be used to pay off `debt`
  - Then any remaining unclaimed node operator funds SHALL be transferred to the node operator's
    withdrawal address
  - Unclaimed node operator funds SHALL be set to 0
- A Node Operator SHALL be able to withdraw unclaimed node operator funds to
  their withdrawal address
- A Node Operator MAY be able to use unclaimed node operator funds for redeposit
  to the beacon chain.
- Newly deposited capital that is awaiting deposit to the beacon chain SHALL be
  excluded from capital distribution, but rather be subject to separate functions
  for `prestake` and `stake` transactions.
- If participating in the smoothing pool, the smoothing pool address SHALL be the only allowed fee recipient.
- If not participating in the smoothing pool, the allowed fee recipients SHALL be: the megapool's address, the smoothing pool's address (note that this donates EL rewards to other Node Operators),
  or the rETH token's address (note that this donates EL rewards to rETH holders).

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
- A Node Operator SHALL be able to change the state of any amount of megapool staked RPL
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
- RPIP editors SHALL modify the eligibility section of [RPIP-4](RPIP-4.md) to match the logic described in this section, and include links to both this RPIP, and the legacy version of RPIP-4.

## Rationale
Megapools are a critical upgrade as we use lower ETH-bond validators because gas costs would otherwise become an increasing drag on revenue.
A few details about the reasoning behind the spec:
- Legacy minipools are disabled because (a) uniformity going forward is desirable and (b) they do not participate in the pooled revenue share described in [RPIP-46](RPIP-46.md).
- The two step unstaking process with an unstaking period recreates the intent in [RPIP-30](RPIP-30.md) within the megapool structure.
- If ETH is sent to a megapool, the spec will count it similarly to any other reward - this makes it quite easy to handle as we don't need to track incoming amounts etc, just the balances.

## Security considerations
- There is a process for permissionlessly distributing funds that won't go to the NO so that funds are never "stuck". Importantly, this distribution does not distribute the NO's share, but rather accounts for it for later claiming. This prevents potential attacks where the withdrawal address causes transactions to fail on an attempted distribution. 
- Both megapool staked RPL and legacy staked RPL are eligible for vote. This is particularly critical while most vote comes from legacy staked RPL. If only megapool staked RPL were counted, there would be a vulnerable window with very low total vote power available.

## Reference Implementation
See <https://github.com/rocket-pool/rocketpool-research/blob/master/Megapools/megapools.md>

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
