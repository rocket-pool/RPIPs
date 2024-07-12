---
rpip: 60
title: Protocol Upgrade Guardrails
description: Introduces a delay after protocol upgrades have been confirmed but prior to them coming into effect.
author: Valdorff (@Valdorff), LongForWisdom (@LongForWisdom)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-08
requires: 
tags: tokenomics-2024, tokenomics-content
---

## Abstract

Regularly scheduled protocol upgrades now have a delay between when they are passed, and when they are executed. Additionally the security council may veto contract upgrades. The veto power should not be used lightly, and misuse may result in replacement of the security council by the pDAO.

## Motivation

This RPIP is motivated by the desire to protect participants in Rocket Pool as best as possible from unfavorable upgrades. Participants should have time to exit the protocol if it changes in a way they do not agree with, _prior_ to those changes coming into effect. 

The security council veto comes out of a desire to have an last-resort layer of security against obviously harmful actions.

This RPIP is part of a set of proposals motivated by a desire to rework Rocket Pool's tokenomics to ensure the protocol’s continued value, development, and longevity. For more details, see the supporting documentation [here](../tokenomics-explainers/001-why-rework). 

## Specification

### Upgrade Delay
- All protocol upgrades SHALL have an `upgrade_delay` delay between when they are passed and when they are executed
- The `upgrade_delay` SHALL be a pDAO setting and its initial value SHALL be 3 weeks


### Security Council Veto
- The security council SHALL have the power to veto contract upgrades (which are proposed by the oDAO)
- Veto powers SHALL NOT be used lightly and SHALL be reserved for cases of vote manipulation, malicious action (eg, oDAO votes in a proposal against the pDAO governance), or contract upgrades that would result in clear damage to the Rocket Pool project
- When the veto power is exercised, the security council SHOULD immediately publish a Veto Explanation Document that describes why it was exercised
- Within 24 hours of the veto power being exercised, the security council MUST publish a Veto Explanation Document that describes why it was exercised
- If possible, the Veto Explanation Document SHOULD suggest alternative similar-but-non-damaging upgrades that could be considered
- If this veto power is abused, the pDAO SHOULD consider replacing the members of the security council

## Security Considerations
- The veto process is itself vulnerable to contract upgrades. However, if said upgrade is malicious, it can be defended against by using the veto process
- Emergency response actions should be taken directly by the security council (see [RPIP-33](RPIP-33.md)) rather than use the regular upgrade process

## Rationale
**Veto Explanation Document**  
The explanation document is a mechanism intended to help the pDAO hold the security council to account for their decision-making around vetos. It gives more information as to the reasoning behind a veto decision, and should help to inform pDAO decision-making post veto. In an ideal world the document accompanies the veto, the 24 hour grace period is intended to cover situations in which the security council must act within a limited time window.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
