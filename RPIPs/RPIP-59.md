---
rpip: 59
title: Deposit Mechanics
description: Describes the mechanics of Node Operator deposits and validator creation, including standard and express queues.
author: Valdorff (@Valdorff), knoshua (@knoshua)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-08
requires: 42, 43, 44
tags: tokenomics-2024, tokenomics-content
---

## Abstract

Defines the queue structures that govern validator deposits and the overall deposit process for Rocket Pool node operators. The standard and express queue structures give mild priority to existing Rocket Pool Node Operators and initial Node Operator deposits through a ticket system.

This proposal also changes the deposit mechanics: In case of a queue, the initial stake transaction happens only once ETH is assigned. This makes it possible to exit from the queue and receive ETH credit up until the validator is dequeued.

## Motivation

These changes are intended to strengthen the protocol by 1) rewarding existing NOs who have shown amazing loyalty to the protocol 2) minimizing governance churn from RPL staked NOs being stuck in queue without voting privileges 3) prioritizing initial deposits to allow easier onboarding of people trialing RP before deciding to migrate in full 4) maximizing Rocket Pool's major decentralization advantage over other LSTs - a huge set of small NOs.

This RPIP is part of a set of proposals motivated by a desire to rework Rocket Pool's tokenomics to ensure the protocol’s continued value, development, and longevity. For more details, see the supporting documentation [here](../tokenomics-explainers/001-why-rework). 

## Specification

### Deposit queue specification
ETH from the deposit pool SHALL be matched with validator deposits from queues as follows:
- There SHALL be a `standard_queue`
  - When adding a validator, users SHALL be able to place their deposit in the `standard_queue`  
- There SHALL be an `express_queue`
  - When adding a validator, users SHALL be able to place their deposit in the `express_queue` by spending one `express_queue_ticket`
- When matching ETH from the deposit pool to queued deposits:
  - First, ETH SHALL be matched to the oldest deposit in the `express_queue`; this is repeated `express_queue_rate` times
  - Next, ETH SHALL be matched to the oldest deposit in the `standard_queue`
  - This sequence SHALL be repeated indefinitely until both queues are empty
    - If one queue is empty, those matches SHALL be skipped
- Each node SHALL be provided `express_queue_tickets` equal to `express_queue_tickets_base_provision` (this includes newly created nodes)
- Each node SHALL be provided additional `express_queue_tickets` equal to `(bonded ETH in legacy minipools)/4` (this will always be zero for newly created nodes)
- The initial settings SHALL be:
  - `express_queue_rate`: 2
  - `express_queue_tickets_base_provision`: 2

### Deposit mechanics specification
- A node operator MUST take 2 actions to start a validator: `deposit` and `stake`

#### `deposit` Transaction
- `deposit` SHALL place the Node Operator ETH in the deposit pool (where it can be used in validators as needed) and place the validator in a queue as described [above](#deposit-queue-specification)
- The following values for the validator SHALL be stored on chain:
    - the public key of the validator
    - the BLS signature over the public key, the withdrawal credentials (the megapool address of the node operator), and the amount (1 ETH) as required by the deposit contract
    - the deposit message root as required by the deposit contract
- The transaction SHALL validate the provided values and revert if they do not match
- `deposit` SHALL assign deposits as described below 

#### Assigning ETH from the Deposit Pool
- ETH from the deposit pool SHALL be assigned the validator at the front of the queue by sending 32 ETH to the associated megapool contract
  - rETH mints SHALL assign `floor(ETH_deposit / 32)` validators
  - There MUST be a permissionless function to execute assignments
- The assignment SHALL execute the `Prestake` transaction, staking 1 ETH to the beacon chain using the values provided in the step above
- The assignment SHALL remove the validator from the queue 

#### `stake` Transaction
- `stake` SHALL revert unless at least `scrub_period` time has passed since ETH was assigned to the validator, to allow for validating the prestake
- If the beacon chain stake is invalid, the validator SHALL be scrubbed 
- `stake` SHALL stake the remaining 31 ETH to the beacon chain to make a complete validator
- If `stake` is not called within `time_before_dissolve` after the ETH was assigned, the validator SHALL be dissolved, returning the unstaked balance to the deposit pool
  - If a validator is dissolved the bonded value SHALL be recoverable. This MAY require further action from the node operator. This MAY temporarily require additional ETH from the node operator.

#### Exiting Queue
- Until ETH is assigned to a validator, it SHALL be possible to exit the queue and receive ETH `credit` for it

#### Social Assignments
- The deposit.assign.socialised.maximum setting SHALL be set to 0

#### Initial Settings
- The initial settings SHALL be:
  - `scrub_period`: 12 hours
  - `time_before_dissolve`: 2 weeks

## Rationale

### Deposit Queue
- The express queue is meant to favor (a) small NOs and (b) existing NOs. The end goal in both cases is to support multiple values enshrined in [RPIP-23](RPIP-23.md) (the pDAO charter): decentralization, protocol safety, and the health of the Ethereum network.
  - The `express_queue_tickets_base_provision` is enough to get started, and currently matches the length of `base_bond_array`
  - The tickets from `(bonded ETH in legacy minipools)/4` are enough to fully migrate to 4-ETH deposits during Saturn 1 using the express queue OR to partly migrate to 1.5-ETH deposits after Saturn 2
  - Tickets may be used or not at an NO's discretion and do not expire! This is because a Node Operator joining during a time when we don't have an NO queue is helping fill a protocol need, and it therefore doesn't make sense for us to require them to incur the additional cost of using up a ticket. This means they get to keep their express queue benefit for a later time if they wish.
- Validators with `base_bond` deposits are prioritized to promote decentralization; new or smaller Node Operators can get up to `base_bond_array.length` validators launched ahead of larger Node Operators adding `reduced_bond` validators.

### Deposit Mechanics
- `prestake` is moved back from `deposit` to assignment to allow for exiting of the queue
- to allow anyone to execute `prestake`, the validator specific data is stored 
- since adding `prestake` to assignments potentially  increases gas cost for rETH deposits, social assignments are deactivated. Node Operators will be able to assign to themselves when at the front of the queue. Additionally, the pDAO may fund keepers that execute assignments automatically. 

## Considerations
To avoid idle ETH and ensure the queue progressing smoothly, we recommend that the pDAO funds (for example through the GMC) development and running of assignment bots that assign ETH in the deposit pool to the queue at a reasonable gas price. 

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
