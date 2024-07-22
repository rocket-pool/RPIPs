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

Regularly scheduled protocol upgrades now have a delay between when they are passed, and when they are executed. Additionally, the security council may veto contract upgrades. The veto power should not be used lightly, and misuse may result in the replacement of the security council by the pDAO.

## Motivation

This RPIP is motivated by the desire to protect participants in Rocket Pool as best as possible from unfavorable upgrades. Participants should have time to exit the protocol if it changes in a way they do not agree with, _prior_ to those changes coming into effect. 

The security council veto comes from a desire to have a last-resort layer of security against obviously harmful actions.

This RPIP is part of a set of proposals motivated by a desire to rework Rocket Pool's tokenomics to ensure the protocol’s continued value, development, and longevity. For more details, see the supporting documentation [here](../tokenomics-explainers/001-why-rework.md). 

## Specification

This specification introduces the following pDAO protocol parameters:
| Name                                                   | Type  | Initial Value |
|--------------------------------------------------------|-------|---------------|
| `upgrade_delay`                                        | Weeks | `1`           |
| `rocketDAOProtocolSettingsSecurity.upgradeveto.quorum` | pct   | `33`          |


### Upgrade Delay
- All protocol upgrades SHALL have an `upgrade_delay` delay between when they are passed and when they are executed


### Security Council Veto
- The security council SHALL have the power to veto contract upgrades (which are proposed by the oDAO)
  - In order to veto a contract upgrade, a quorum of `rocketDAOProtocolSettingsSecurity.upgradeveto.quorum` of the security council MUST be met or exceeded
- Veto powers SHALL NOT be used lightly and SHALL be reserved for cases of vote manipulation, malicious action (eg, oDAO votes in a proposal against the pDAO governance), or contract upgrades that would result in clear damage to the Rocket Pool project
- When the veto power is exercised, the security council SHOULD immediately publish a Veto Explanation Document that describes why it was exercised
- Within 24 hours of the veto power being exercised, the security council MUST publish a Veto Explanation Document that describes why it was exercised
- If possible, the Veto Explanation Document SHOULD suggest alternative similar-but-non-damaging upgrades that could be considered
- If this veto power is abused, the pDAO SHOULD discuss replacing the members of the security council

## Rationale
**Veto Explanation Document**  
The explanation document is a mechanism intended to help the pDAO hold the security council to account for their decision-making around vetos. It gives more information as to the reasoning behind a veto decision and should help to inform pDAO decision-making post-veto. In an ideal world, the document accompanies the veto. The 24-hour grace period is intended to cover situations in which the security council must act within a limited time window.

**Upgrade Delay Setting**  
Due to the considerations around slowing down hotfixes [described above](#security-considerations), the initial upgrade delay setting is attempting to be the smallest that allows "enough" time for participants to exit the system ahead of a proposal if they deem that appropriate.

Any upgrade will have `proposal.vote.delay.time` (currently 1 week) and `upgrade_delay` (initially set to 1 week). This is likely enough time to exit validators and withdraw ETH (barring an extreme exit queue). In normal circumstances, there will be more time available to exit. There is a leadup period to the sentiment poll (currently at least 1 week), there is a pDAO vote period (currently at least 2 weeks), there's often dev time (which can be large), there's a human action required to raise the oDAO proposal, and there are many human actions to place oDAO votes after `proposal.vote.delay.time`. These delays should provide significant breathing room to allow participants to exit their ETH positions ahead of a proposal, and may even provide enough time to exit RPL positions despite the 28-day unstaking period. RPL exiting was not heavily prioritized because it's unclear how a severely damaging protocol change wouldn't be priced in by 28 days anyhow.

## Security Considerations

### Upgrade Delay
- Emergency response settings changes should be taken directly by the security council (see [RPIP-33](RPIP-33.md)) rather than use the regular governance process
- The fastest possible upgrade speed is significantly slowed from the previous state. It used to take `proposal.vote.delay.time` (currently 1 week) and will now take an additional `upgrade_delay` (initially set to 1 week). This means that important hotfixes could take longer. It may also mean that emergency pauses (eg, disabling rETH deposits) could last longer until a hotfix takes effect.
- The fastest possible upgrade speed is now faster than the RPL `unstaking_period` defined in [RPIP-30](RPIP-30.md), and there are edge cases where it may be faster than the time it takes to exit ETH (if the validator exit queue is very full)

### Security Council Veto
- The security council veto means it would require both a rogue oDAO and a rogue security council to pass a corrupt proposal
- The security council veto means that a rogue security council could block a valid proposal
  - This is mitigated by the pDAO's ability to replace the security council. While a valid proposal could be blocked once, it could not be blocked indefinitely
- The veto process can itself be modified by contract upgrades. However, if a corrupt proposal tries to change the veto process, that can be defended against by using the veto process.
- The security council veto means that if pDAO governance were captured, protocol upgrades could be stopped at will (by installing a compromised security council). The oDAO would be unable to pass any contract upgrades (without the approval of the compromised security council), and the only way back would be to recapture pDAO governance. While this means there is a real tradeoff, this "inability to upgrade" is seen as less problematic than the "ability to pass any upgrade" that a rogue oDAO would have without the veto.
- The quorum for this veto is lower than for security council proposals (controlled by `rocketDAOProtocolSettingsSecurity.members.quorum`). This is intended to provide additional resilience to, eg, a rogue oDAO that attempts to bribe a security council into not vetoing a corrupt proposal (as fewer bribe-resistant members are needed). This sort of interaction doesn't exist for normal security council proposals, which is what leads to a different desired threshold. 

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
