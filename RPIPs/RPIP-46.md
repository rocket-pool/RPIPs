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
Currently, the commission determines the payout of revenue split between rETH and each specific minipool. Other parties, such as RPL, gain value indirectly. This proposal allows for splitting revenue between four initial parties: reth (the main product), node operators (the decentralized operators actually staking), voters (a subset of operators that have vote power), and a surplus revenue mechanism.

This proposal also includes a small set of items for potential future use:
- Seals that the security council can use to increase the NO share -- this is being used to find a reasonable setting based on the actual market.
- An allowlist of controllers that may make changes to the settings, which allows for potential automation in the future

## Specification
1. Inflation settings SHALL be modified to retain inflation to the DAOs and eliminate inflation to NOs
   1. `rpl.inflation.interval.rate` SHALL be set to `1000040763630249500` (1.5% per year)
   2. Node Operators (`rocketClaimNode`) allocation SHALL be set to 0%
   3. pDAO (`rocketClaimDAO`) allocation SHALL be set to 95%
   4. oDAO (`rocketClaimTrustedNode`) allocation SHALL be set to 5%
2. There SHALL be the following defined shares with settings: `node_operator_commission_share`, `voter_share`, `surplus_share`
   1. `node_operator_commission_share`: each NO receives this percentage of the commission from the borrowed ETH on validators they run. Unlike the remainder of the shares, this is _not_ a protocol revenue (ie, it is not socialized).
   2. `voter_share`: each NO receives a share of revenue based on the vote-eligible RPL staked to their megapool. The overall voter share of revenue is based on the setting, and each NO receives a proportion of that based on `vote_eligible_RPL_in_their_megapool/total_vote_eligible_RPL_in_megapools`.
   3. `surplus_share`: this share of revenue is used to distribute revenue beyond that used for protocol operation (such as the shares above)
3. `reth_commission` SHALL be defined as the sum of all defined shares that have settings
4. `reth_share` SHALL be defined as `100% - reth_commission`
5. Distributions of revenue from borrowed ETH MUST respect the defined shares
   1. If shares change between claims, distributions MUST make an effort to account for the different values. For example, a distribution could use a duration-weighted average share. Approximations MAY be used where they significantly reduce complexity and/or costs.
   2. Legacy minipools are an exception and MAY continue to support earlier distribution methodologies 
6. `node_operator_commission_share`, `surplus_share`, `increase_no_share_seal_increment`, `increase_no_share_seal_count`, and `allowlisted_controllers` MAY be updated by pDAO vote
7. `node_operator_commission_share` and `surplus_share`, MAY be updated by an address in the `allowlisted_controllers` array
   1. This functionality SHALL not be used without a separate pDAO vote to enable a controller and add it to the list
8. The security council SHALL have a limited-use power to increase the `node_operator_commission_share` by `increase_no_share_seal_increment` and decrease the `surplus_share` by the same amount
   1. This power SHALL be usable if `increase_no_share_seal_count` > 0
   2. `increase_no_share_seal_count` SHALL be decremented by one upon using this power
   3. This power SHOULD be used if the deposit pool is over half-full for the majority of a 2-week period at the current `node_operator_commission_share`
   4. The pDAO MAY change `increase_no_share_seal_count` via vote
9. The initial settings SHALL be:
   1. `node_operator_commission_share`: 3.5%
   2. `voter_share`: 5%
   3. `surplus_share`: 5.5%
   4. `increase_no_share_seal_increment`: 0.5%
   5. `increase_no_share_seal_count`: 6
   6. `allowlisted_controllers`: []
10. Prior to the release of Saturn 1, a ranked-choice vote MUST be held to select a mechanism for surplus revenue share
    1. The choices MAY include [RPIP-45: RPL Burn](RPIP-45.md), [RPIP-50: RPL LP](RPIP-50.md), or using higher `voter_share`
    2. If higher `voter_share` is selected, the entirety of the ["Specification taking effect with Saturn 2"](#specification-taking-effect-with-saturn-2) section below SHALL be deleted
    3. The selected mechanism MAY be implemented in Saturn 1
       1. If not, the revenue SHALL be held in reserve until the mechanism is implemented. In such a case, once the mechanism is implemented, the revenue SHALL be distributed in the same amount of time it took to build up, or faster.
    4. The selected mechanism MUST be implemented in Saturn 2
    5. When implemented, `surplus_share` SHOULD be renamed to something more specific

## Specification taking effect with Saturn 2
1. Updating `voter_share`:
   1. A new function SHALL be available to update `voter_share`, which MAY be called by anyone
   2. It MUST revert if it's been called within the last 45 days
   3. It MUST revert if the total RPL eligible to vote is between `voter_share_target_min` and `voter_share_target_max` (inclusive)
   4. If <`voter_share_target_min` of total RPL is eligible to vote and the function succeeds:
      1. `voter_share` is increased to `voter_share * (1+voter_share_relative_step)`
      2. `surplus_share` is decreased by the difference between the old and new `voter_share`
         1. If this would reduce `surplus_share` below 0%, the function call MUST revert 
   5. If >`voter_share_target_max` of total RPL is eligible to vote and the function succeeds:
      1. `voter_share` is decreased to `voter_share / (1+voter_share_relative_step)`
      2. `surplus_share` is increased by the difference between the old and new `voter_share`
   6. Because this involves _voters_ modifying `voter_share`, there is an acknowledged conflict of interest here. As a result, changing this method of "Updating `voter_share`" SHALL require a supermajority vote with at least 75% of the vote in support of any change.
2. `voter_share_relative_step`, `voter_share_target_min`, and `voter_share_target_max` MAY be updated by pDAO vote; however, it SHALL require a supermajority vote with at least 75% of the vote in support of any change.
3. The initial settings SHALL be:
   1. `voter_share_relative_step`: 15%
   2. `voter_share_target_min`: 55%
   3. `voter_share_target_max`: 65%

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
- If `increase_no_share_seal_count` reaches 1, the pDAO should (a) consider adding seals, (b) consider stepping up `no_share` themselves, and (c) consider decreasing `reth_share`
- When there are large changes to the system (eg, Saturn 2 release), do note that some volatility is expected and should be considered when acting
- If we are approaching the self-limits described in [RPIP-17](RPIP-17.md), the pDAO should act to limit one or both of rETH demand (via reducing RPL inflation spend on rETH demand and/or lower `reth_share`) or NO supply (via lower `no_share`). This would result in higher `surplus_share` (or lower RPL inflation).

## Historic revenue share values
| Date                         | Share Settings                                                                     |
|------------------------------|------------------------------------------------------------------------------------|
| 2024-03-08<br>(ratified TBD) | `node_operator_commission_share`: 3.5%, `voter_share`: 5%, `surplus_share`: 5.5%  |

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
