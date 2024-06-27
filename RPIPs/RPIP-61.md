---
rpip: 61
title: Balance Submission Guardrail
description: This proposal limits how much the oDAO can change the rETH exhange rate.
author: knoshua (@knoshua)
discussions-to: https://dao.rocketpool.net/t/rpip-balance-submission-guardrail/3010
status: Review
type: Protocol
category (*only required for Protocol ): Core
created: 2024-05-27

---

## Abstract
This proposal limits how much the oDAO can change the rETH exhange rate (used for minting or burning rETH against ETH) in a single update. The limit is a protocol parameter controlled by the pDAO. The pDAO can't set it lower than a minimum value. The parameter can't be changed by the Security Council.
This proposal also limits how frequently updates can happen, using an existing protocol parameter that the pDAO controls. 

## Motivation
Currently, the oDAO can change rETH exchange rate arbitrarily within one block. This exposes rETH holders: In the worst case, a compromised oDAO can make rETH worthless for any holder and extract any ETH available. During normal operation, the rETH exchange rate doesn't need to change a lot. This proposal aims to reduce oDAO trust by introducing a sensible limit that doesn't interfere with legitimate use of balance updates and removes unnecessary risk from illegitimate use.

## Specification

### Maximum rETH Delta Protocol Parameter
- There SHALL be a Maximum rETH Delta protocol parameter that the pDAO can vote to change.
- Maximum rETH Delta SHALL initially be set to 2 percent.
- The pDAO SHALL NOT be able to set Maximum rETH Delta lower than 1 percent.
- The Security Council SHALL NOT be able to change Maximum rETH Delta.

### Network Balance Updates
- The oDAO SHALL NOT be able to be submit a Network Balance update if the time elapsed since the last Network Balance update is less than 95% of the Network Submit Balances Frequency.
- The oDAO SHALL NOT be able to change the rETH exchange rate by more than Maximum rETH Delta.
- If an update would lead to an rETH exchange rate change of more than Maximum rETH Delta, the oDAO SHALL submit an update of Maximum rETH Delta instead.

## Rationale
The pDAO can change the limit to a sufficiently high value to effectively disable this guardrail. The minimum guarantees that normal operation of the protocol can not be interfered with through a parameter change.

Limiting the change per update alone is not enough, since multiple updates in one block would be able to undermine it severely. Therefore update frequency is also limited.

A minimum for the Maximum rETH Delta parameter is necessary to make sure that regular updates are possible. I chose 1% as a round number that is large enough to allow for that, even with some volatility in updates because of big MEV blocks, and at the same time significantly reduce impact in the worst case scenario.



## Backwards Compatibility
There are no backwards compatibility concerns with this proposal.

## Security Considerations
Since the oDAO is also in control of contract upgrades, a compromised oDAO has the power to remove the guardrail introduced here. But contract upgrades are subject to a 7 day delay, which would give rETH holders an opportunity to react.

In case of many slashings on the beacon chain that lead to correlation penalties, it is possible that an update of more than 2 percent would be appropriate. Note that there would be ~18 days delay between initial slashing and application of the correlation penalty. With RPIP-33, the pDAO could change the Maximum rETH Delta parameter within 21 days.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/.)
