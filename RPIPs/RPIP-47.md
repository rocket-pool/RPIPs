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

This proposal suggests limiting that upgrade choice in the future. Users will be able to opt in to upgrade or use an "old" delegate for a defined period after a newer version. However, once the defined period has passed, they will no longer use the old delegate.

## Specification
- Megapool delegate contracts SHALL have an expiration block (ie, execution layer block)
- When a new megapool delegate is released, the contract upgrade SHALL include:
  - Setting the new megapool delegate's expiration block to "no expiration"
  - Setting the previous delegate's expiration block to occur `delegate_upgrade_buffer` after the upgrade
- Interactions with a megapool delegate after its expiration block SHALL revert
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
- The security council SHALL have the power to veto contract upgrades 
  - Veto powers SHALL NOT be used lightly and SHALL be reserved for cases of vote manipulation, malicious action (eg, oDAO votes in a proposal against the pDAO governance), or contract upgrades that would result in clear damage to the Rocket Pool project
  - When this veto power is exercised, the security council MUST publish a Veto Explanation Document that describes why this step was taken; if possible, the report SHOULD also suggest potential similar-but-non-damaging votes that could be considered
  - If this veto power is abused, the pDAO SHOULD consider replacing the security council
- The initial settings SHALL be:
  - `delegate_upgrade_buffer`: 4 months 
  - `default_upgrade_delay`: 3 weeks
  - `fast_upgrade_delay`: 3 days
  - `fast_upgrade_supermajority`: 75% of vote
  - `fast_upgrade_seal_count`: 1

### Notes
- `*` Example use: pDAO passes a vote by supermajority with a fast upgrade request; as part of the upgrade, `fast_upgrade_seal_count` is set to 1. The security council has 1 seal. They use it here and the value goes to zero; upon execution, their count is set back to 1 by the upgrade.
- `**` Example use: Due to a conflict of interest, the pDAO does not want to rely on the security council's judgment for an upcoming upgrade. They directly vote on-chain to set `fast_upgrade_seal_count` to 0.
- Note that the veto process is itself vulnerable to contract upgrades. However... such upgrades, if malicious, can be defended against by using a veto.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
