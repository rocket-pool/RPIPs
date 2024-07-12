---
rpip: 47
title: Enable Forced Delegate Upgrades
description: Allows protocol governance to force upgrade Node Operators after a Rocket Pool protocol upgrade takes place, and a grace period has expired. 
author: Valdorff (@Valdorff), LongForWisdom(@LongForWisdom)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-08
requires: 43
tags: tokenomics-2024, tokenomics-content
---

## Abstract
Currently, Node Operators can choose to upgrade (or not) for their minipool delegate (the contract at the Ethereum withdrawal address that governs how funds are disbursed, among other things). This (a) means the protocol can only effect change in ways that are in the interest of individual NOs, and (b) makes it more challenging to design the protocol as it must remain backward compatible with every past minipool delegate.

This proposal suggests limiting that upgrade choice in the future. Users can opt-in to upgrade or use an "old" delegate for a defined period after a newer version is released. Once the defined period has passed, users will no longer be able to use the old delegate, and they may be permissionlessly upgraded to the latest version. 

## Motivation

Supporting many iterations of the same basic functionality as the protocol evolves makes protocol development slower, more risky, and more expensive, with no real limit on the degree to which this debt can accumulate. The ability to force upgrades allows the protocol to avoid this tax on efficiency and remain responsive to future development needs.

Additionally, the pDAO must have the ability to effect change in ways that benefit the protocol as a whole, but that may not benefit every individual node operator in isolation. What benefits individuals in the short term can lead to ruin in the long term for both that individual and the collective. The pDAO is unable to protect against this outcome without the ability to force upgrades. 

This RPIP is part of a set of proposals motivated by a desire to rework Rocket Pool's tokenomics to ensure the protocol’s continued value, development, and longevity. For more details, see the supporting documentation [here](../tokenomics-explainers/001-why-rework). 

## Specification

### Upgrade Buffer Enforcement
- Each `Megapool delegate` contract SHALL have an execution layer `expiration_block` variable which should initialize to "no expiration"
- Each `Megapool delegate` contract SHALL have a non-modifiable `upgrade_buffer` value
  - The `Megapool delegate`'s `upgrade_buffer` SHALL be 120 days
- The only permitted modification to a `Megapool delegate`'s `expiration_block` variable SHALL be:
  - Any protocol contract may set its value to now + its `upgrade_buffer`
  - Any protocol contract may set its value to "no expiration"

### Upgrade
- When a new `megapool delegate` is released, a protocol contract SHOULD update all previous `megapool delegate`s `expiration_block`'s to now + their `upgrade_buffer`s
- A node operator SHALL be able to upgrade their active `megapool delegate` to the latest `megapool delegate` at any time
- Functionality MAY be provided to allow a Node Operator to automatically upgrade to the latest `megapool delegate`
- After an active `megapool delegate`'s expiration block has passed:
  - Functions in the expired megapool delegate SHALL revert when not called by a protocol contract
  - No capital SHALL be distributed to the withdrawal address(es)
  - The expired `megapool delegate` SHALL be permissionlessly upgradeable to the latest `megapool delegate`

## Rationale

**Direct Upgrade versus Recovery Delegate**  
Initially, this specification included a 'recovery delegate' that would provide basic functionality for a node operator. In place of forcing an upgrade to the latest version, validators on expiring delegates would be shifted to the recovery delegate. This was dropped primarily due to scope and implementation complexity reasons.

**Next Version versus Latest Version**  
Some debate covered whether the upgrade should push validators on expiring delegates to the _latest_ delegate, or the _next_ delegate. The latest was chosen to allow the skipping of bugged delegates to a fixed version and to improve UX (no need to cycle through to get to latest.)

## Security Considerations
- Part of the risk mitigation here depends on some users upgrading their megapool delegate well before the expiration block to build up "Lindy"
  - If this doesn't happen naturally, it may be possible to incentivize early adopters with GMC funds

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
