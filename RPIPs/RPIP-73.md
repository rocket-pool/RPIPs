---
rpip: 73
title: rETH Protection From Underperforming Nodes
description: Force exit significantly underperforming validators to protect rETH from yield loss.
author: Samus (@orangesamus)
discussions-to: <URL> TODO
status: Draft
type: Protocol
category: Core
created: 2025-07-30
requires (*optional): 44
---

## Abstract
This proposal defines the conditions which Rocket Pool will use to force exit significantly underperforming megapool validators. There will be two types of issues that Rocket Pool will screen for: being mostly offline, and being online with poor performance. Megapool validators will be monitored to ensure they can be ejected once they cross a threshold of an unacceptable number of bad performance days within a period of time.

## Motivation
Rocket Pool node operators have been expected to perform well, since underperformance results in missed attestation penalties applied to their bonded stake, and causes a loss of rewards. Despite these incentives, some node operators significantly underperform with validators offline for extended periods of time (eg, abandoned validators). This underperformance reduces the relative value of rETH, as yield losses are socialized across all rETH holders. Historically, it has been impossible for Rocket Pool to force exit significantly underperforming validators, but with [EIP-7002](https://eips.ethereum.org/EIPS/eip-7002), it will be possible to protect rETH value by exiting significantly underperforming megapool validators. This RPIP proposes this functionality to improve rETH yield and provide the market with a more competitive liquid staking token.

## Specification
- This specification SHALL be implemented in Saturn 2. 
- There SHALL be an offline performance ejection system introduced as described below:
  - There SHALL be a protocol-wide parameter `offline_performance_percentage_threshold` that is initially set to 20%
  - There SHALL be a protocol-wide paramater `offline_performance_days_threshold` that is initially set to 14 days
  - There SHALL be a protocol-wide parameter `offline_performance_period` that is initially set to 20 days
  - There SHALL be a protocol-wide parameter `offline_performance_ejection_penalty` that is initially set to 0.03 ETH
  - Each megapool validator SHALL be assigned a `offline_performance_status` variable that is initially set to FALSE
  - A validator's `offline_performance_status` variable SHALL be set to TRUE under the condition described below: 
    - The validator had an `offline_performance_period` with `offline_performance_days_threshold` days that each met the criteria below:
      - The validator had `attestations_included/attestations_assigned < offline_performance_percentage_threshold` for that day
  - The `exit` function as described in [RPIP-44](RPIP-44.md), MUST be permissionlessly callable under the condition described below:
    - The megapool validator's `offline_performance_status` is set to TRUE
    - The `offline_performance_ejection_penalty` for that validator has been added to its associated megapool's `debt`
- There SHALL be a poor performance ejection system introduced as described below:
  - There SHALL be a protocol-wide parameter `poor_performance_percentage_threshold` that is initially set to 90%
  - There SHALL be a protocol-wide paramater `poor_performance_days_threshold` that is initially set to 30 days
  - There SHALL be a protocol-wide parameter `poor_performance_period` that is initially set to 100 days
  - There SHALL be a protocol-wide parameter `poor_performance_ejection_penalty` that is initially set to 0.05 ETH
  - Each megapool validator SHALL be assigned a `poor_performance_status` variable that is initially set to FALSE
  - A validator's `poor_performance_status` variable SHALL be set to TRUE under the condition described below: 
    - The validator had a `poor_performance_period` with `poor_performance_days_threshold` days that each met the criteria below:
      - The validator had `attestations_included/attestations_assigned < poor_performance_percentage_threshold` for that day
  - The `exit` function as described in [RPIP-44](RPIP-44.md), MUST be permissionlessly callable under the condition described below:
    - The megapool validator's `poor_performance_status` is set to TRUE
    - The `poor_performance_ejection_penalty` for that validator has been added to its associated megapool's `debt`

## Rationale
The implementation of rETH protection as proposed here was chosen as a starting point for improving rETH yields, by simply applying a flat penalty and ejecting underperformers (rather than a more complex idea like rETH restitution, where the exact cost of the underperformance is tracked and rETH is made whole for any penalties/lost rewards from underperformance). Ejecting validators rather than continuously penalizing was chosen, since this preserves the node operator's capital (eg, the node operator is deceased and goes offline, but it takes time for a beneficiary to determine how to recover the funds), and allows the pool stakers funds to be assigned to a better performing node operator (this is better for Ethereum liveness, better for other node operators to earn from properly stewarding the funds, and simpler for the protocol than tracking and penalizing for lost rewards).

An ejection system with unique criteria for underperformance was introduced, since [RPIP-43](RPIP-43.md)) and [RPIP-44](RPIP-44.md) only specify forced exits for `deficit >= exit_deficit`, and the goal with this RPIP is to eject systematic bad performers in a timely manner without overly penalizing them. Setting two different thresholds for being mostly offline vs poor performances gives operators time to resolve performance issues and stay lenient for things outside of the operator's control, while also reducing drag from sustained underperformance. The penalty is set to 0.04 ETH since this is expected to roughly account for lost rewards. The equation I used for this is: `borrowedETH*NetworkAPY*(1-Commission)*(1-averagePerformancePercentage)*performanceDaysThreshold/DaysPerYear = 30.5*0.03*0.86*1*14/365 = 0.03 ETH ; 30.5*0.03*0.86*0.8*30/365 = 0.05 ETH`. For the poor performance threshold, 90% was chosen based off of ArtDemocrat's previous research [here](https://dao.rocketpool.net/t/rapid-research-incubator-submission-reth-protection-through-rpl-rerouting-deflation/2599), and confirmation of similar results from a more up to date dashboard from Steely [here](https://rocketpool.steely-test.org). Performance was measured by attestations only to simplify the system rather than tracking the relative cost/weight of missed block proposals/sync committees, but this granularity could be accounted for in a future update.

## Security Considerations
The specifications still need further development but this initial version was written as a starting point. I expect the responsibility to monitor and update performance status's could be given to the oDAO to manage, and guardrails to avoid abuse of this power should be considered. It may also be worth considering a more dynamic performance threshold as there may be cases where the entire Ethereum network is underperforming the proposed threshold and it would be better in that scenario to avoid penalizations for this.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
