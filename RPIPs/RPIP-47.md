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
  - The pDAO MAY include a change to `fast_upgrade_seal_count` in a voted upgrade*
  - The pDAO MAY change `fast_upgrade_seal_count` via vote**
  - The security council SHALL use this power if and only if:
    - A vote specifically requested a fast upgrade
    - The vote passed with a supermajority of `fast_upgrade_supermajority` or more
    - The security council deems it acceptable from a security point of view
- The security council SHALL have a limited-use power to cancel an upgrade currently in the delay between passing and being executed
  - This power SHALL be usable if `veto_seal_count` > 0 
  - `veto_seal_count` SHALL be decremented by one upon using this power
  - The only way to change `veto_seal_count` MUST be via on-chain pDAO vote
  - The security council SHALL use this power if and only if:
    - The pDAO vote on-chain to grant a veto seal for a specific use
    - The vote passes with a supermajority of `veto_supermajority` or more
    - The security council deem that veto appropriate
- The initial settings SHALL be:
  - `default_upgrade_delay`: 3 weeks
  - `fast_upgrade_delay`: 3 days
  - `fast_upgrade_supermajority`: 75% of vote
  - `fast_upgrade_seal_count`: 1
  - `veto_seal_count`: 0
  - `veto_supermajority`: 66% of vote

### Notes
- `*` Example use: pDAO passes a vote by supermajority with a fast upgrade request; as part of the upgrade, `fast_upgrade_seal_count` is set to 1. The security council has 1 seal. They use it here and the value goes to zero; upon execution, their count is set back to 1 by the upgrade.
- `**` Example use: Due to a conflict of interest, the pDAO does not want to rely on the security council's judgement for an upcoming upgrade. They directly vote on-chain to set `fast_upgrade_seal_count` to 0.
- Note that vetoing requires a race against the clock. The pDAO must go through all stages of their on-chain voting process to grant a vote seal, and then the security council must use it within time. Vote phase times, default upgrade delay and any contract vote upgrade delays should all be considerted together when modifying.
- Note that the veto process is itself vulnerable to contract upgrades. However... such upgrades, if malicious, can be defended against by using a veto.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
