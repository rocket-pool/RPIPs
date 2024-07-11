---
rpip: 46
title: Universal Adjustable Revenue Split
description: Allow the revenue from borrowed ETH (aka, rETH commission) to be split between fixed targets; the relative split between targets is adjustable.
author: Valdorff (@Valdorff)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-08
requires: potentially rpip-45 or rpip-50
tags: tokenomics-2024, tokenomics-content
---

## Abstract
Currently, the commission determines the payout of revenue split between rETH and each specific minipool. Other parties, such as RPL, gain value indirectly. This proposal allows for splitting revenue between four initial parties: rETH (the main product), node operators (the decentralized operators actually staking), voters (a subset of operators that have vote power), and a surplus revenue mechanism.

This proposal also includes:
1. A setting controlled by the security council that can mildly increase NO commission -- this can be used to react to market conditions more quickly than the pDAO is able to act
2. For future use: an allowlist of controllers that may make changes to the settings, which potentially allows for automation

## Motivation

Universal Adjustable Revenue Split is motivated by the desire for increased flexibility for the Rocket Pool protocol as the Ethereum ecosystem evolves in the future. It's critical that the protocol can respond to the actions of other actors as effectively as possible and UARS helps facilitate this. It is also important that the protocol can balance the demand and supply of rETH to encourage sustainable and continuous growth on both sides of this equation. 

A fixed percentage of RPL inflation is currently being used to fund ongoing maintenance and development of the Rocket Pool protocol, a valuable RPL token means more bang for the same amount of inflation, and UARS supports multiple mechanisms that support RPL value. Competent and aligned governance will also be necessary as the protocol evolves and UARS facilitates this via directing a share of revenue to holders of vote-eligible RPL. 

This RPIP is part of a set of proposals motivated by a desire to rework Rocket Pool's tokenomics to ensure the protocol’s continued value, development, and longevity. For more details, see the supporting documentation [here](../tokenomics-explainers/001-why-rework). 

## Specification
### UARS
1. There SHALL be the following defined shares with settings: `node_operator_commission_share`, `node_operator_commission_share_council_adder`, `voter_share`, `surplus_share`
   1. `node_operator_commission_share + node_operator_commission_share_council_adder`: each NO receives this percentage of the commission from the borrowed ETH on validators they run. Unlike the remainder of the shares, this is _not_ a protocol revenue (ie, it is not socialized).
   2. `voter_share`: each NO receives a share of revenue based on the vote-eligible RPL staked to their megapool. The overall voter share of revenue is based on the setting, and each NO receives a proportion of that based on `vote_eligible_RPL_in_their_megapool/total_vote_eligible_RPL_in_megapools`.
   3. `surplus_share -  node_operator_commission_share_council_adder`: this share of revenue is used to distribute revenue beyond that used for protocol operation (such as the shares above)
2. `reth_commission` SHALL be defined as the sum of `node_operator_commission_share`, `voter_share`, and `surplus_share`
3. `reth_share` SHALL be defined as `100% - reth_commission`
4. Distributions of revenue from borrowed ETH MUST respect the defined shares
   1. If shares change between claims, distributions MUST make an effort to account for the different values. For example, a distribution could use a duration-weighted average share. Approximations MAY be used where they significantly reduce complexity and/or costs.
   2. Legacy minipools are an exception and SHALL continue to support earlier distribution methodologies 
5. `node_operator_commission_share`, `node_operator_commission_share_council_adder`, and `surplus_share`, SHALL be updateable by an address in the `allowlisted_controllers` array
   1. This functionality SHALL not be used without a separate pDAO vote to enable a controller and add it to the list
6. The `node_operator_commission_share_council_adder` setting SHALL only allow values where:
   1. 0% ≤ `node_operator_commission_share_council_adder` ≤ `max_node_operator_commission_share_council_adder` 
   2. `node_operator_commission_share_council_adder` ≤ `surplus_share`
7. The `node_operator_commission_share_council_adder` setting SHALL be controllable by the security council without requiring a delay
8. The security council SHOULD increment `node_operator_commission_share_council_adder` by 0.5% if the deposit pool is over half-full for the majority of a 2-week period with a constant `node_operator_commission_share + node_operator_commission_share_council_adder`
    1. The security council SHALL NOT otherwise change `node_operator_commission_share_council_adder`
9. `voter_share` SHALL NOT be controllable by pDAO vote and SHALL be initialized to 5%
10. The initial pDAO settings SHALL be:
    1. `node_operator_commission_share`: 3.5%
    2. `node_operator_commission_share_council_adder`: 0%
    3. `surplus_share`: 5.5%
    4. `max_node_operator_commission_share_council_adder`: 1%
    5. `allowlisted_controllers`: []

### Inflation
1. Inflation settings SHALL be modified to retain inflation to the DAOs and eliminate inflation to NOs
   1. `rpl.inflation.interval.rate` SHALL be set to `1000040763630249500` (1.5% per year)
   2. Node Operators (`rocketClaimNode`) allocation SHALL be set to 0%
   3. pDAO (`rocketClaimDAO`) allocation SHALL be set to 95%
   4. oDAO (`rocketClaimTrustedNode`) allocation SHALL be set to 5%

### Surplus revenue share vote
Prior to the release of Saturn 1, a ranked-choice vote MUST be held to select a mechanism for surplus revenue share
1. The choices MAY include [RPIP-45: RPL Burn](RPIP-45.md), [RPIP-50: RPL LP](RPIP-50.md), and using higher `voter_share`
2. If higher `voter_share` is selected:
   1. The entirety of the ["Specification taking effect with Saturn 2"](#specification-taking-effect-with-saturn-2) section below SHALL be deleted 
   2. `voter_share` MAY be updated by an address in the `allowlisted_controllers` array
   3. `node_operator_commission_share_council_adder` shall be subtracted from `voter_share` instead of `surplus_share`
   4. `voter_share` SHALL be a pDAO setting (and thus the specification item about it not being controllable by pDAO vote MUST also be removed)
3. The selected mechanism MAY be implemented in Saturn 1
   1. If not, the revenue SHALL be held in reserve until the mechanism is implemented. In such a case, once the mechanism is implemented, the revenue SHALL be distributed in the same amount of time it took to build up, or faster.
4. The selected mechanism MUST be implemented in Saturn 2
5. When implemented, `surplus_share` SHOULD be renamed to something more specific

## Specification taking effect with Saturn 2
### Updating `voter_share`:
The `voter_share` SHALL be adjustable, to target a share of voting eligible RPL (including 
megapool staked RPL and legacy staked RPL) relative to the total supply of RPL minus RPL 
owned by the protocol in its treasury or as a result of `surplus_share` buy backs. The voting
eligible RPL target is a range between `vote_eligible_target_min` and `vote_eligible_target_max`
1. Updating `voter_share`:
   1. A permissionless function SHALL be available to update `voter_share`
   2. It MUST revert if it's been called within the last 45 days
   3. It MUST revert if the total RPL eligible to vote is between `vote_eligible_target_min` and `vote_eligible_target_max` (inclusive)
   4. If <`vote_eligible_target_min` of total RPL is eligible to vote and the function succeeds:
      1. `voter_share` is increased to `voter_share * (1+voter_share_relative_step)`
      2. `surplus_share` is decreased by the difference between the old and new `voter_share`
         1. If this would reduce `surplus_share` below 0%, the function call MUST revert 
   5. If >`vote_eligible_target_max` of total RPL is eligible to vote and the function succeeds:
      1. `voter_share` is decreased to `voter_share / (1+voter_share_relative_step)`
      2. `surplus_share` is increased by the difference between the old and new `voter_share`
   6. Because this involves _voters_ modifying `voter_share`, there is an acknowledged conflict of interest here. As a result, changing this method of "Updating `voter_share`" SHALL require a supermajority vote with at least 75% of the vote in support of any change.
2. The pDAO SHALL be able to update `voter_share_relative_step`, `vote_eligible_target_min`, and `vote_eligible_target_max` by vote; however, it SHALL require a supermajority vote with at least 75% of the vote in support of any change.
3. The initial settings SHALL be:
   1. `voter_share_relative_step`: 15%
   2. `vote_eligible_target_min`: 55%
   3. `vote_eligible_target_max`: 65%

## Optional heuristics
This section reflects some of the thinking at the time this RPIP was drafted. These ideas are explicitly _not_ binding/enforceable, and they may freely change over time/context.

Some abstract guidelines:
- Consider `node_operator_commission_share` as a requirement to function. If this is not high enough to attract the supply we need, the protocol is non-functional.
- Consider `voter_share` as a requirement to function. There is a method specified that's intended to attract governance security effectively, ["Specification taking effect with Saturn 2"](#specification-taking-effect-with-saturn-2).
- Finally, consider `surplus_share`. RPL holders are incentivized to maximize something along the lines of `surplus_share * rETH_TVL`. This means voters will generally have an incentive to make rETH holding attractive.

Some example concrete guidelines:
- If the NO queue is continuously over 500 deposits for 2 weeks and the trend is upwards, the pDAO should act to either increase rETH demand or decrease NO supply. This could use one or more of the following tactics:
  - Eg, rETH demand can be increased by spending more RPL on marketing or partner incentives; that RPL can be sourced by increasing RPL inflation. This is beneficial because it allows targeted intervention to spur rETH demand.
  - Eg, rETH demand can be increased by increasing `reth_share` alongside a counterbalancing decrease to `surplus_share`
  - Eg, NO supply can be decreased by reducing `no_share` alongside a counterbalancing increase to `surplus_share`
- If the NO queue is continuously over 1000 deposits for 4 weeks and the trend is upwards, the pDAO should take action to decrease the supply of NOs.
- When there are large changes to the system (eg, Saturn 2 release), do note that some volatility is expected and should be considered when acting
- If we are approaching the self-limits described in [RPIP-17](RPIP-17.md), the pDAO should act to limit one or both of rETH demand (via reducing RPL inflation spend on rETH demand and/or lower `reth_share`) or NO supply (via lower `no_share`). This would result in higher `surplus_share` (or lower RPL inflation).

## Rationale
UARS is meant to enable the protocol to listen to the market and act effectively across the entire protocol.
A few details about the reasoning behind the spec:
- We provide for future automated controller contracts, but do not create any at this time. This is partly because we are fairly naive to the market, and partly in the interest of time to market for the next upgrade.
- The `node_operator_commission_share_council_adder` allows for much more rapidly tracking the market, especially when we first start UARS and may be quite far from the an appropriate `node_operator_commission_share + node_operator_commission_share_council_adder` value
  - Due to the low maximum for the adder, the pDAO would need to act to enable much growth in `node_operator_commission_share + node_operator_commission_share_council_adder`. For example, if the adder is at 1%, the pDAO could vote to set it to 0% and add 1% to `node_operator_commission_share`. This active pDAO participation ensures that this setting tracks closely to the will of the pDAO. 
- The [surplus revenue share vote](#surplus-revenue-share-vote) is intended to allow the main body of the tokenomics to move forward, while allowing more time to get information about our options here before choosing a path

## Security Considerations
- `node_operator_commission_share_council_adder` is intended to be used in a particular way, but the security council may misuse it in other ways
  - This can be mitigated by setting `node_operator_commission_share + node_operator_commission_share_council_adder` to the desired value with the adder set to the max allowed value, thus preventing it from being set higher
  - The pDAO may also replace the security council if it misuses its power
- There is an acknowledged conflict of interest around `voters` controlling `voter_share`
  - There is an attempt to mitigate abuse by requiring a supermajority; nonetheless, that still depends on enough well-intentioned voters acting to defend the interests of other groups within the Rocket Pool community
- Attracting desirable vote-eligible share may not be trivial
  - While we have a heuristic to increase incentives for vote-eligible RPL, it may not move as fast as the market
  - RP allows for node operation with a separate RPL and ETH provider; this may limit the desired alignment of the voter
- Vote-elgibility may not be a strong proxy for "active voters"
  - The incentives describe only incentivize staking vote-eligible RPL
  - It will be important to supervise how much of the vote-eligible RPL is actually voting and/or delegating
    - There was some discussion around incentivizing voting more directly, but (a) they were complicated and (b) there's a fear that while voting can be incentivized, _informed/thoughtful_ voting cannot 

## Historic revenue share values
| Date                         | Share Settings                                                                     |
|------------------------------|------------------------------------------------------------------------------------|
| 2024-03-08<br>(ratified TBD) | `node_operator_commission_share`: 3.5%, `voter_share`: 5%, `surplus_share`: 5.5%  |

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
