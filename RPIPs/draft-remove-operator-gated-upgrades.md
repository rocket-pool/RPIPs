---
rpip:
title: Remove Operator-gated Upgrades
description: Allow for the protocol to make protocol-wide decisions rather than relying on per-operator upgrades
author: Valdorff (@Valdorff)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-08
---

## Abstract
Currently, Node Operators can choose to upgrade (or not) for their minipool delegate (the contract at the Ethereum withdrawal address that governs how funds are disbursed, among other things). This (a) means the protocol can only effect change in ways that are in the interest of individual NOs, and (b) makes it more challenging to design the protocol as it must remain backwards compatible with every past minipool delegate.

This proposal suggests removing that choice in the future. Instead, a time lock will provide time for users to initiate withdrawals in a case where they are unwilling to operate under a new incoming set of rules.

## Specification
- Contracts moving forward SHALL NOT require Node Operators to individually opt in to them
- All protocol upgrades SHALL have a `default_upgrade_delay` delay between when they are passed and when they are executed
- The security council SHALL have a limited-use power to use a shorter delay of `fast_upgrade_delay`
  - This power SHALL be usable if `fast_upgrade_seal_count` > 0 
  - `fast_upgrade_seal_count` SHALL be decremented by one upon using this power
  - The pDAO MAY change `fast_upgrade_seal_count` via vote*
  - The security council SHALL use this power if and only if:
    - A vote specifically requested a fast upgrade
    - The vote passed with a supermajority of `fast_upgrade_supermajority` or more
    - The security council deems it acceptable from a security point of view
- The initial settings SHALL be:
  - `default_upgrade_delay`: 3 weeks
  - `fast_upgrade_delay`: 3 days
  - `fast_upgrade_supermajority`: 75% of vote
  - `fast_upgrade_seal_count`: 1

*Example use: pDAO passes a vote by supermajority with a fast upgrade request; as part of the upgrade, `fast_upgrade_seal_count` is set to 1. The security council has 1 seal. They use it here and go to zero; upon execution, their count is set back to 1.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
