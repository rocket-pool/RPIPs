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
This proposal defines how Rocket Pool will use a strike system to force exit significantly underperforming megapool validators.

## Motivation
Rocket Pool node operators have been expected to perform well, since underperformance results in missed attestation penalties applied to their bonded stake, and causes a loss of rewards. Despite these incentives, some node operators significantly underperform with validators offline for extended periods of time (eg, abandoned validators). This underperformance reduces the relative value of rETH, as yield losses are socialized across all rETH holders. Historically, it has been impossible for Rocket Pool to force exit significantly underperforming validators, but with [EIP-7002](https://eips.ethereum.org/EIPS/eip-7002), it will be possible to protect rETH value by exiting significantly underperforming megapool validators. This RPIP proposes this functionality to improve rETH yield and provide the market with a more competitive liquid staking token.

## Specification
- This specification SHALL be implemented in Saturn 2. 
- There SHALL be a strike system introduced as described below:
  - Each megapool validator SHALL be assigned a `validator_strikes` variable that is initially set to 0
  - There SHALL be a protocol-wide paramater `strike_ejection_threshold` that is initially set to 3
  - There SHALL be a protocol-wide parameter `strike_ejection_penalty` that is initially set to 0.25 ETH
  - The `strike_ejection_penalty` SHALL be added to the `debt` of a megapool for each validator under the condition: `validator_strikes >= strike_ejection_threshold` 
  - The `exit` function as described in [RPIP-44](RPIP-44.md), MUST be permissionlessly callable under the condition: `validator_strikes >= strike_ejection_threshold`
  - There SHALL be a protocol-wide parameter `performance_threshold` that is initially set to 93%
  - A megapool validator SHALL increase its `validator_strikes` by 1 under the conditions described below:
    - The validator has been in the "Staked" state (as described in [RPIP-43](RPIP-43.md)), for the entirety of an interval (28 days, which corresponds to intervals in [RPIP-51](RPIP-51.md))
    - The validator has had `attestations_included/attestations_assigned < performance_threshold` for the interval
  - A megapool validator SHALL decrease its `validator_strikes` by 1 under the conditions described below:
    - The validator's `validator_strikes` > 0 at the start of the interval
    - The validator has had `attestations_included/attestations_assigned >= performance_threshold` for the interval


## Rationale
The implementation of rETH protection as proposed here was chosen as a starting point for improving rETH yields, by simply applying a flat penalty and ejecting underperformers (rather than a more complex idea like rETH restitution, where the exact cost of the underperformance is tracked and rETH is made whole for any penalties/lost rewards from underperformance). Ejecting validators rather than continuously penalizing was chosen, since this preserves the node operator's capital (eg, the node operator is deceased and goes offline, but it takes time for a beneficiary to determine how to recover the funds), and allows the pool stakers funds to be assigned to a better performing node operator (this is better for Ethereum liveness, better for other node operators to earn from properly stewarding the funds, and simpler for the protocol than tracking and penalizing for lost rewards).

A strike system was introduced, since [RPIP-43](RPIP-43.md)) and [RPIP-44](RPIP-44.md) only specify forced exits for `deficit >= exit_deficit`, and the goal with this RPIP is to eject systematic bad performers in a timely manner without overly penalizing them. Setting the threshold at 3 strikes gives operators time to resolve performance issues and stay lenient for things outside of the operator's control, while also reducing drag from sustained underperformance for several intervals. The penalty is set to 0.3 ETH since this is expected to roughly account for lost rewards with some additional buffer room. The equation I used for this is: `borrowedETH*NetworkAPY\*offlineIntervals/IntervalsPerYear = 30.5\*0.03\*3/13 = 0.21 ETH`. For the performance threshold, 93% was chosen based off of ArtDemocrat's previous research [here](https://dao.rocketpool.net/t/rapid-research-incubator-submission-reth-protection-through-rpl-rerouting-deflation/2599), and confirmation of similar results from a real time dashboard from Steely [here](https://rocketpool.steely-test.org). Performance was measured by attestations only to simplify the system rather than tracking the relative cost/weight of missed block proposals/sync committees, but this granularity could be accounted for in a future update.  The validator must be in "Staked" state for the entire interval to give node operators a chance to debug any initial issues, and ensure a strike isn't given due to poor performance combined with unlucky timing (eg, becoming "Staked" the day before an interval ends and being offline for just part of one day). The feature to decrease strikes through good performance is intended to give node operators a chance to prove they can fix any issues, and avoid permanent penalization.

## Security Considerations
The specifications still need further development but this initial version was written as a starting point. I expect the responsibility to apply strikes could be given to the oDAO to manage, and guardrails to avoid abuse of this power should be considered. It may also be worth considering a more dynamic performance threshold as there may be cases where the entire Ethereum network is underperforming the proposed threshold and it would be better in that scenario to avoid penalizations for this. As the specs are written, a malicious node operator could alternate months of underperformance and adequate performance and avoid ejection - but there is a lack of economic incentive to do this, and this would be a low impact to the protocol since at least half the time there is adequate performance. If this behaviour is observed, the "decreasing" strikes mechanism could be removed, or modified to require more than one consecutive interval of adequate performance.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
