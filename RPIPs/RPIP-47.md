---
rpip: 47
title: Enable Forced Delegate Upgrades
description: Allows protocol governance to force upgrade Node Operators after a Rocket Pool protocol upgrade takes place, and a grace period has expired. 
author: Valdorff (@Valdorff)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-08
requires: 43
tags: tokenomics-2024, tokenomics-content
---

## Abstract
Currently, Node Operators can choose to upgrade (or not) for their minipool delegate (the contract at the Ethereum withdrawal address that governs how funds are disbursed, among other things). This (a) means the protocol can only effect change in ways that are in the interest of individual NOs, and (b) makes it more challenging to design the protocol as it must remain backwards compatible with every past minipool delegate.

This proposal suggests limiting that upgrade choice in the future. Users will be able to opt in to upgrade or use an "old" delegate for a defined period after a newer version. However, once the defined period has passed, they will no longer use the old delegate. To ensure timely availability of protocol funds, after the defined period, anyone may change the megapool's delegate to a minimal 'recovery delegate' that is only capable of exiting validators and distributing funds. 

## Motivation

Supporting many iterations of the same basic functionality as the protocol evolves makes protocol development slower, more risky, and more expensive, with no real limit on the degree to which this debt can accumulate. The ability to force upgrades allows the protocol to avoid this tax on efficiency and remain responsive to future development needs.

Additionally, the pDAO must have the ability to effect change in ways that benefit the protocol as a whole, but that may not benefit every individual node operator in isolation. What benefits individuals in the short term can lead to ruin in the long term for both that individual and the collective. The pDAO is unable to protect against this outcome without the ability to force upgrades or exits. 

This RPIP is part of a set of proposals motivated by a desire to rework Rocket Pool's tokenomics to ensure the protocol’s continued value, development, and longevity. For more details, see the supporting documentation [here](../tokenomics-explainers/001-why-rework). 

## Specification
### Megapool delegates
- `Megapool delegate` contracts SHALL have an expiration block (ie, execution layer block)
- When a new `megapool delegate` is released, the contract upgrade SHALL include:
  - Setting the new `megapool delegate`'s expiration block to "no expiration"
  - Setting the previous `megapool delegate`'s expiration block to occur `delegate_upgrade_buffer` after the upgrade
- A node operator SHALL be able to upgrade their active `megapool delegate` to the latest `megapool delegate` at any time (including after the active `megapool delegate`'s expiration block)
- After the active `megapool delegate`'s expiration block:
  - The active `megapool delegate` SHALL NOT be usable by the node operator
  - The node operator SHALL NOT be able to claim any rewards
- The initial setting of `delegate_upgrade_buffer` SHALL be 4 months.

### Megapool recovery delegate
- There SHALL be a single `megapool recovery delegate` contract that is used by all megapools in the protocol
- The `megapool recovery delegate` for a megapool SHALL NOT be usable by anyone if the current block is less than or equal to the active `megapool delegate`'s expiration block
- The `megapool recovery delegate` for a megapool SHALL be usable by any address if the current block is greater than the active `megapool delegate`'s expiration block
- The `megapool recovery delegate` SHALL have the following capabilities:
  - Force exiting ALL validators under the megapool
    - In the Saturn 1 release, this MAY be a non-functional stub
  - Removal of any pending validators from the Rocket Pool queue
  - Processing of voluntary exit proofs
  - Collection of any penalties or deficits
  - Final distribution of rewards and capital 
- The `megapool recovery delegate` SHOULD be updated only rarely, and only to support changes to the Ethereum execution layer exit process, or major changes in the Rocket Pool protocol. Simultaneous upgrades to both the `megapool recovery delegate` and the latest `megapool delegate` SHOULD be avoided if possible.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
