---
rpip: 60
title: Protocol Upgrade Guardrails
description: Introduces a delay after protocol upgrades have been confirmed but prior to them coming into effect.
author: Valdorff (@Valdorff)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-08
requires: 
tags: tokenomics-2024, tokenomics-content
---

## Abstract

Regularly scheduled protocol upgrades now have a delay between when they are passed, and when they are executed. 

The security council may trigger a faster upgrade with a shorter delay, but there are limitations on when they may use this power. This security council power has a maximum number of uses that may be set by the pDAO. The security council cannot use the fast upgrade power if they have no remaining uses, and each use removes one seal. Seals may be restored via pDAO vote. 

Additionally the security council may veto contract upgrades. The veto power should not be used lightly, and misuse may result in replacement of the security council by the pDAO.

## Motivation

This RPIP is motivated by the desire to protect participants in Rocket Pool as best as possible from unfavorable upgrades. Participants should have time to exit the protocol if it changes in a way they do not agree with, _prior_ to those changes coming into effect. 

Secondly, we want to ensure that the protocol has the procedures to act in an accelerated fashion when the situation calls for it, but that this upgrade path has strict limitations and is not misused. The security council is well placed to manage accelerated upgrades but must remain accountable to the pDAO.

This RPIP is part of a set of proposals motivated by a desire to rework Rocket Pool's tokenomics to ensure the protocol’s continued value, development, and longevity. For more details, see the supporting documentation [here](../tokenomics-explainers/001-why-rework). 

## Specification

### Protocol upgrades
- All protocol upgrades SHALL have a `default_upgrade_delay` delay between when they are passed and when they are executed
- The security council SHALL have a limited-use power to use a shorter delay of `fast_upgrade_delay`
  - This power SHALL be usable if `fast_upgrade_seal_count` > 0 
  - `fast_upgrade_seal_count` SHALL be decremented by one upon using this power
  - The pDAO SHALL be able to include a change to `fast_upgrade_seal_count` in a voted upgrade*
  - The pDAO SHALL be able to change `fast_upgrade_seal_count` via vote**
  - The security council SHALL use this power if and only if:
    - A vote specifically requested a fast upgrade
    - The vote passed with a supermajority of `fast_upgrade_supermajority` or more
    - The security council deems it acceptable from a security point of view
- The security council SHALL have the power to veto contract upgrades (which are proposed by the oDAO)
  - Veto powers SHALL NOT be used lightly and SHALL be reserved for cases of vote manipulation, malicious action (eg, oDAO votes in a proposal against the pDAO governance), or contract upgrades that would result in clear damage to the Rocket Pool project
  - When this veto power is exercised, the security council MUST publish a Veto Explanation Document that describes why this step was taken; if possible, the report SHOULD also suggest potential similar-but-non-damaging votes that could be considered
  - If this veto power is abused, the pDAO SHOULD consider replacing the security council
- The initial settings SHALL be:
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
