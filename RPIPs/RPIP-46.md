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

Universal Adjustable Revenue Split (UARS) is motivated by the desire for increased flexibility for the Rocket Pool protocol as the Ethereum ecosystem evolves in the future. It's critical that the protocol can respond to the actions of other actors as effectively as possible and UARS helps facilitate this. It is also important that the protocol can balance the demand and supply of rETH to encourage sustainable and continuous growth on both sides of this equation. 

A fixed percentage of RPL inflation is currently being used to fund ongoing maintenance and development of the Rocket Pool protocol, a valuable RPL token means more bang for the same amount of inflation, and UARS supports multiple mechanisms that support RPL value. Competent and aligned governance will also be necessary as the protocol evolves and UARS facilitates this via directing a share of revenue to holders of vote-eligible RPL. 

This RPIP is part of a set of proposals motivated by a desire to rework Rocket Pool's tokenomics to ensure the protocol’s continued value, development, and longevity. For more details, see the supporting documentation [here](../tokenomics-explainers/001-why-rework.md). 

## Specification
### Universal Adjustable Revenue Split
This specification introduces the following pDAO protocol parameters:

| Name                                               | Type       | Initial Value |
|----------------------------------------------------|------------|---------------|
| `node_operator_commission_share`                   | pct        | `5`           |
| `node_operator_commission_share_council_adder`     | pct        | `0`           |
| `voter_share`                                      | pct        | `9`           |
| `max_node_operator_commission_share_council_adder` | pct        | `1`           |
| `allowlisted_controllers`                          | address [] | `[]`          |

1. There SHALL be the following defined revenues:
   1. `node_operator_commission_share + node_operator_commission_share_council_adder`: each NO receives this percentage of the commission from the borrowed ETH on validators they run. Unlike the remainder of the shares, this is _not_ a protocol revenue (ie, it is not socialized).
   2. `voter_share -  node_operator_commission_share_council_adder`: each NO receives a share of revenue based on the vote-eligible RPL staked to their megapool. The overall voter share of revenue is based on the setting, and each NO receives a proportion of that based on `vote_eligible_RPL_in_their_megapool / total_vote_eligible_RPL_in_megapools`.
2. `reth_commission` SHALL be defined as the sum of `node_operator_commission_share` and `voter_share`
3. `reth_share` SHALL be defined as `100% - reth_commission`
4. Distributions of revenue from borrowed ETH MUST respect the defined shares
   1. If shares change between claims, distributions MUST make an effort to account for the different values. For example, a distribution could use a duration-weighted average share. Approximations MAY be used where they significantly reduce complexity and/or costs.
   2. Legacy minipools are an exception and SHALL continue to support earlier distribution methodologies 
5. `node_operator_commission_share`, `node_operator_commission_share_council_adder`, and `voter_share` SHALL be updateable by any address in the `allowlisted_controllers` array
   1. This functionality SHALL not be used without a separate pDAO vote to enable a controller and add it to the list
6. The `node_operator_commission_share_council_adder` setting SHALL only allow values where:
   1. 0% ≤ `node_operator_commission_share_council_adder` ≤ `max_node_operator_commission_share_council_adder` 
   2. `node_operator_commission_share_council_adder` ≤ `voter_share`
7. The `node_operator_commission_share_council_adder` setting SHALL be controllable by the security council without requiring a delay
8. The security council SHOULD increment `node_operator_commission_share_council_adder` by 0.5% if the deposit pool is over half-full for the majority of a 2-week period with a constant `node_operator_commission_share + node_operator_commission_share_council_adder`
    1. The security council SHALL NOT otherwise change `node_operator_commission_share_council_adder`

### RPL issuance rewards
- The `proposed_method_share` specified in [RPIP-30](RPIP-30.md) SHALL no longer be used; instead a new `proposed_method_share` SHALL be defined as follows:
    - For one node, its `node_weight` divided by the sum of all `node_weight` across nodes
    - If staked RPL value in ETH is <=15% borrowed ETH, then:
    ```math
    $$
      node\_weight=100 * staked\_rpl\_value\_in\_eth
    $$
    ```
    - If staked RPL value in ETH is > 15% borrowed ETH, then:
    ```math
    $$
    node\_weight = (13.6137 + 2 * ln(100*\frac{staked\_rpl\_value\_in\_eth}{borrowed\_eth} - 13)) * borrowed\_eth
    $$
    ```
    - This value MAY be approximated if necessary
- All staked RPL SHALL be counted for this purpose (both "legacy staked RPL" and "megapool staked RPL" as defined in [RPIP-43](RPIP-43.md))
- The remainder of [RPIP-30](RPIP-30.md) SHALL remain in force

### Revenue share vote
Prior to the release of Saturn 1, a ranked-choice vote MUST be held to select a mechanism for revenue share to RPL
1. The choices SHALL include the state from Saturn 1
2. The choices SHOULD include [RPIP-45: RPL Burn](RPIP-45.md) and [RPIP-50: RPL LP](RPIP-50.md)
   1. The choices SHOULD specify a new share (eg, `buy_and_burn_share`) with an initial value
   2. The choices SHOULD specify a reduction of other shares (eg, `voter_share`) to balance the new share 
3. The choices MAY include novel options created for this vote
4. The selected mechanism MUST be implemented in Saturn 2
5. If the state from Saturn 1 is chosen, the entirety of the ["Implementing the revenue share vote"](#implementing-the-revenue-share-vote) section below SHALL be deleted

## Specification taking effect with Saturn 2
### RPL issuance rewards and inflation
1. Inflation settings SHALL be modified to retain inflation to the DAOs and eliminate inflation to NOs
   1. `rpl.inflation.interval.rate` SHALL be set to `1000040763630249500` (1.5% per year)
   2. Node operators (`rocketClaimNode`) allocation SHALL be set to 0%
   3. pDAO (`rocketClaimDAO`) allocation SHALL be set to 95%
   4. oDAO (`rocketClaimTrustedNode`) allocation SHALL be set to 5%
2. There SHALL be no RPL issuance rewards to NOs 

### Implementing the revenue share vote
For this section, we'll be writing `new_share`. When the revenue share vote is passed, that will define the share's name and this section SHALL be updated.

1. The following updates SHALL be made in the [Universal Adjustable Revenue Split](#universal-adjustable-revenue-split) section of the specification above, with the placeholder description filled in:
   1. There SHALL be the following defined shares with settings: `node_operator_commission_share`, `node_operator_commission_share_council_adder`, `voter_share`, `new_share`
   2. `node_operator_commission_share + node_operator_commission_share_council_adder`: each NO receives this percentage of the commission from the borrowed ETH on validators they run. Unlike the remainder of the shares, this is _not_ a protocol revenue (ie, it is not socialized).
   3. `voter_share`: each NO receives a share of revenue based on the vote-eligible RPL staked to their megapool. The overall voter share of revenue is based on the setting, and each NO receives a proportion of that based on `vote_eligible_RPL_in_their_megapool/total_vote_eligible_RPL_in_megapools`.
   4. `new_share -  node_operator_commission_share_council_adder`: this share of revenue is used to [PLACEHOLDER]
   5. `reth_commission` SHALL be defined as the sum of `node_operator_commission_share`, `voter_share`, and `new_share`
   6. `node_operator_commission_share`, `node_operator_commission_share_council_adder`, and `new_share`, SHALL be updateable by any address in the `allowlisted_controllers` array
2. `voter_share` SHALL no longer be a pDAO protocol parameter. the pDAO will not be able to vote changes to it and changes will rely on the method described below.
3. Updating `voter_share`:
   1. A permissionless function SHALL be available to update `voter_share`
   2. It MUST revert if it's been called within the last 45 days
   3. It MUST revert if the total RPL eligible to vote relative to the circulating supply of RPL is between `vote_eligible_target_min` and `vote_eligible_target_max` (inclusive)
      1. RPL eligible to vote includes both megapool staked RPL and legacy staked RPL
      2. The circulating supply of RPL is the total supply of RPL minus RPL owned by the protocol (eg, in treasury or an LP as a result of RPIP-50) 
   4. If <`vote_eligible_target_min` of total RPL is eligible to vote and the function succeeds:
      1. `voter_share` is increased to `voter_share * (1+voter_share_relative_step)`
      2. `new_share` is decreased by the difference between the old and new `voter_share`
         1. If this would reduce `new_share` below 0%, the function call MUST revert 
   5. If >`vote_eligible_target_max` of total RPL is eligible to vote and the function succeeds:
      1. `voter_share` is decreased to `voter_share / (1+voter_share_relative_step)`
      2. `new_share` is increased by the difference between the old and new `voter_share`
   6. Because this involves _voters_ modifying `voter_share`, there is an acknowledged conflict of interest here. As a result, changing this method of "Updating `voter_share`" SHALL require a supermajority vote with at least 75% of the vote in support of any change.
4. The pDAO SHALL be able to update `voter_share_relative_step`, `vote_eligible_target_min`, and `vote_eligible_target_max` by vote; however, it SHALL require a supermajority vote with at least 75% of the vote in support of any change.
5. The initial settings SHALL be:
   1. `voter_share_relative_step`: 15%
   2. `vote_eligible_target_min`: 55%
   3. `vote_eligible_target_max`: 65%

## Optional heuristics

<details>
	<summary>Click to expand optional heuristics</summary>
Once the revenue share vote concludes, one of the two sections below will be obsolete and should be deleted. If appropriate, `new_share` should be replaced with the proper name of the share.

### If revenue is shared between rETH, NOs, and vote-eligible RPL
This section reflects some of the thinking at the time this RPIP was drafted. These ideas are explicitly _not_ binding/enforceable, and they may freely change over time/context.

Some abstract guidelines:
- Consider `node_operator_commission_share` as a requirement to function. If this is not high enough to attract the supply we need, the protocol is non-functional.
- Finally, consider `voter_share`. RPL holders are incentivized to maximize something along the lines of `voter_share * rETH_TVL`. This means voters will generally have an incentive to make rETH holding attractive.

Some example concrete guidelines:
- If the NO queue is continuously over 500 deposits for 2 weeks and the trend is upwards, the pDAO should act to either increase rETH demand or decrease NO supply. This could use one or more of the following tactics:
  - Eg, rETH demand can be increased by spending more RPL on marketing or partner incentives; that RPL can be sourced by increasing RPL inflation. This is beneficial because it allows targeted intervention to spur rETH demand.
  - Eg, rETH demand can be increased by increasing `reth_share` alongside a counterbalancing decrease to `voter_share`
  - Eg, rETH demand can be increased by increasing `reth_share` _and_ NO supply can be simultaneously decreased by a counterbalancing decrease to `no_share` 
- If the NO queue is continuously over 1000 deposits for 4 weeks and the trend is upwards, the pDAO should take action to decrease the supply of NOs.
- When there are large changes to the system (eg, Saturn 2 release), do note that some volatility is expected and should be considered when acting
- If we are approaching the self-limits described in [RPIP-17](RPIP-17.md), the pDAO should act to limit one or both of rETH demand (via reducing RPL inflation spend on rETH demand and/or lower `reth_share`) or NO supply (via lower `no_share`). This would result in higher `voter_share` (or lower RPL inflation).


### If revenue includes a new_share beyond the above
This section reflects some of the thinking at the time this RPIP was drafted. These ideas are explicitly _not_ binding/enforceable, and they may freely change over time/context.
For this section, we'll be writing `new_share`.

Some abstract guidelines:
- Consider `node_operator_commission_share` as a requirement to function. If this is not high enough to attract the supply we need, the protocol is non-functional.
- Consider `voter_share` as a requirement to function. There is a method specified that's intended to attract governance security effectively, ["Implementing the revenue share vote"](#implementing-the-revenue-share-vote).
- Finally, consider `new_share`. RPL holders are incentivized to maximize something along the lines of `(new_share+voter_share) * rETH_TVL`. This means voters will generally have an incentive to make rETH holding attractive.

Some example concrete guidelines:
- If the NO queue is continuously over 500 deposits for 2 weeks and the trend is upwards, the pDAO should act to either increase rETH demand or decrease NO supply. This could use one or more of the following tactics:
  - Eg, rETH demand can be increased by spending more RPL on marketing or partner incentives; that RPL can be sourced by increasing RPL inflation. This is beneficial because it allows targeted intervention to spur rETH demand.
  - Eg, rETH demand can be increased by increasing `reth_share` alongside a counterbalancing decrease to `new_share`
  - Eg, NO supply can be decreased by reducing `no_share` alongside a counterbalancing increase to `new_share`
  - Eg, rETH demand can be increased by increasing `reth_share` _and_ NO supply can be simultaneously decreased by a counterbalancing decrease to `no_share`
- If the NO queue is continuously over 1000 deposits for 4 weeks and the trend is upwards, the pDAO should take action to decrease the supply of NOs.
- When there are large changes to the system (eg, Saturn 2 release), do note that some volatility is expected and should be considered when acting
- If we are approaching the self-limits described in [RPIP-17](RPIP-17.md), the pDAO should act to limit one or both of rETH demand (via reducing RPL inflation spend on rETH demand and/or lower `reth_share`) or NO supply (via lower `no_share`). This would result in higher `new_share` (or lower RPL inflation).
</details>

## Rationale
UARS is meant to enable the protocol to listen to the market and act effectively across the entire protocol.
A few details about the reasoning behind the spec:
- We provide for future automated controller contracts, but do not create any at this time. This is partly because we are fairly naive to the market, and partly in the interest of time to market for the next upgrade.
- The `node_operator_commission_share_council_adder` allows for much more rapidly tracking the market, especially when we first start UARS and may be quite far from the an appropriate `node_operator_commission_share + node_operator_commission_share_council_adder` value
  - Due to the low maximum for the adder, the pDAO would need to act to enable much growth in `node_operator_commission_share + node_operator_commission_share_council_adder`. For example, if the adder is at 1%, the pDAO could vote to set it to 0% and add 1% to `node_operator_commission_share`. This active pDAO participation ensures that this setting tracks closely to the will of the pDAO. 
- The [revenue share vote](#revenue-share-vote) is intended to allow the main body of the tokenomics to move forward, while allowing more time to get information about our options here before choosing a path

### RPL issuance rewards and inflation
As the core value capture is no longer based on a minimum RPL requirement, Saturn 1 removes the minimum to receive RPL issuance rewards. This "cliff" has been an extremely poor piece of UX and anecdotally led to many operators exiting validators. Note that the withdrawal limits are not updated; those had previously aligned with the end of the linear region, and they still do. With Saturn 2, RPL issuance rewards stop entirely. At this point, the bond curve should be creating sufficient attraction that we believe we can get to this desired end state more smoothly.

## Security Considerations
- `node_operator_commission_share_council_adder` is intended to be used in a particular way, but the security council may misuse it in other ways
  - This can be mitigated by setting `node_operator_commission_share + node_operator_commission_share_council_adder` to the desired value with the adder set to the max allowed value, thus preventing it from being set higher
  - The pDAO may also replace the security council if it misuses its power
- If there is a separate `new_share` going to all RPL, there is an acknowledged conflict of interest around `voters` controlling `voter_share`
  - There is an attempt to mitigate abuse by requiring a supermajority; nonetheless, that still depends on enough well-intentioned voters acting to defend the interests of other groups within the Rocket Pool community
- Attracting desirable vote-eligible share may not be trivial
  - While we have a heuristic to increase incentives for vote-eligible RPL, it may not move as fast as the market
  - RP allows for node operation with a separate RPL and ETH provider; this may limit the desired alignment of the voter
- Vote-eligibility may not be a strong proxy for "active voters"
  - The incentives describe only incentivize staking vote-eligible RPL
  - It will be important to supervise how much of the vote-eligible RPL is actually voting and/or delegating
    - There was some discussion around incentivizing voting more directly, but (a) they were complicated and (b) there's a fear that while voting can be incentivized, _informed/thoughtful_ voting cannot 

## Historic revenue share values
| Date                         | Share Settings                                           |
|------------------------------|----------------------------------------------------------|
| 2024-07-17<br>(ratified TBD) | `node_operator_commission_share`: 5%, `voter_share`: 9%  |

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
