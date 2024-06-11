---
rpip: #<to be assigned>
title: MEV penalty guardrail
description: This proposal introduces a limit on the total number of MEV penalties that the oDAO can apply to minipools in one week.
author: knoshua (@knoshua)
discussions-to: https://dao.rocketpool.net/t/rpip-mev-penalty-guardrail/3009
status: Draft
type: Protocol 
category (*only required for Protocol ): Core
created: 2024-05-22
---


## Abstract
This proposal introduces a global limit on the total number of MEV penalties that the oDAO can apply to minipools in one week. The limit is a protocol parameter controlled by the pDAO. The pDAO can't set it lower than a minimum value. The parameter can't be changed by the Security Council.

## Motivation
Currently, the oDAO can apply an unlimited number of penalties without any delay. This exposes node operators: In the worst case, a compromised oDAO can penalize 100% of the ETH that node operators have in minipools. For legitimate use of the penalty system, this is overpowered. The number of proposals in a time interval are limited and therefore also the number of stealing incidents in a time interval are limited. This proposal aims to reduce oDAO trust by introducing a sensible limit that doesn't interfere with legitimate use of the penalty system and removes unnecessary risk from illegitimate use.

## Specification

### Maximum Total Penalties protocol parameter
- There SHALL be a Maximum Total Penalties protocol parameter that the pDAO can vote to change
- Maximum Total Penalties SHALL initially be set to 2,500
- The pDAO SHALL NOT be able to set Maximum Total Penalties lower than 2,500
- The Security Council SHALL NOT be able to change Maximum Total Penalties


### Applying penalties
- The oDAO SHALL NOT be able to apply more than Maximum Total Penalties in 50,400 consecutive slots

## Rationale

### Sufficient for Legitimate Use
Given a slot time of 12 seconds, there are 50,400 slots in 7 days.
There are at most 50,400 blocks in 50,400 slots. At worst, every block that Rocket Pool validators propose is stolen. As of May 2024, Rocket Pool has a network share of ~2.5%. The probability that Rocket Pool validators get more than 1,370 proposals in 50,400 blocks is 0.1%. 2,500 is chosen as initial and minimum setting to keep this probability very low even after significant market share growth.

### Reduces Risk of Illegitimate Use Significantly
At 2,500 penalties per week and as of May 202 ~25,000 minipools the worst case impact is reduced to <2% of node operator ETH.


## Backwards Compatibility
There are no backwards compatibility concerns with this proposal.

## Security Considerations
Since the oDAO is also in control of contract upgrades, a compromised oDAO has the power to remove the guardrail introduced here. But contract upgrades are subject to a 7 day delay, which would give node operators an opportunity to react.

This RPIP assumes that the oDAO is applying penalties shortly after they occur. Retroactively applying penalties for a long time period might become limited. However, the retroactive approach is already problematic, since there is no guarantee that MEV stealers will stick around to be penalized.

This RPIP assumes that the oDAO applies one penalty per infraction. If for example we wanted to get rid of the two free initial strikes per minipool, this should best be done through contract changes and not by applying multiple penalties per infraction. 


## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
