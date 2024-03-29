---
rpip: 46
title: Universal variable commission
description: Allow the revenue from borrowed ETH to be split different ways
author: Valdorff (@Valdorff)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-08
requires: rpip-burn
---

## Abstract
Currently, commission determines the payout of revenue from split between rETH and each specific minipool. Other parties, such as RPL, gain value indirectly. This proposal allows for splitting revenue between four initial parties: reth (the main product), node operators (the decentralized operators actually staking), voters (a subset of operators that have vote power), and rpl burn.

This proposal also includes a small set of items for potential future use:
- Seals that the security council can use to increase the NO share -- this is being used to find a reasonable setting based on the actual market.
- Shares for the oDAO and pDAO -- if RP grows significantly, we may be able to eliminate RPL inflation to pay these parties and instead use a share of revenue.
- An allowlist of controllers that may make changes to the settings, which allows for potential automation in the future

## Specification
- There SHALL be the following defined shares with settings: `node_operator_commission_share`, `voter_share`, `rpl_burn_share`, `pdao_treasury_share`, `odao_share`
  - `node_operator_commission_share`: each NO receives this percentage of commission from the borrowed ETH on validators they run. Unlike the remainder of the shares, this is _not_ a protocol revenue (ie, it is not socialized).
  - `voter_share`: each NO receives a share of revenue based on their vote-eligible staked RPL. The overall voter share of revenue is based on the setting, and each NO receives a proportion of that based on `vote_eligible_RPL_on_node/total_vote_eligible_RPL`.
  - `rpl_burn_share`: this share of revenue is used to buy and burn RPL per [RPIP-45](RPIP-45.md).
  - `pdao_treasury_share`: this share of revenue is sent to the pDAO treasury in the vault
  - `odao_share`: this share of revenue is sent to a splitter contract that anyone can distribute to oDAO members 
- `reth_commission` SHALL be defined as the sum of all defined shares that have settings
- `reth_share` SHALL be defined as `100% - reth_commission`
- Distributions of revenue from borrowed ETH MUST respect the defined shares
  - Legacy minipools are an exception and MAY continue to support earlier distribution methodologies 
- These settings MAY be updated by pDAO vote
- These settings MAY be updated by an address in the `allowlisted_controllers` array
  - This functionality SHALL not be used without a separate pDAO vote to enable a controller and add it to the list
- The security council SHALL have a limited-use power to increase the `node_operator_commission_share` by `increase_no_share_seal_increment` and decrease the `rpl_burn_share` by the same amount
  - This power SHALL be usable if `increase_no_share_seal_count` > 0
  - `increase_no_share_seal_count` SHALL be decremented by one upon using this power
  - The pDAO MAY change `increase_no_share_seal_count` via vote
- The initial settings SHALL be:
  - `node_operator_commission_share`: 2.5%
  - `voter_share`: 1%
  - `rpl_burn_share`: 10.5%
  - `pdao_treasury_share`: 0%
  - `odao_share`: 0%
  - `increase_no_share_seal_increment`: 0.5%
  - `increase_no_share_seal_count`: 6
  - `allowlisted_controllers`: []

### Specified heuristics
- The security council SHOULD use their `increase_no_share_seal_increment` power if:
  - `increase_no_share_seal_count` > 0  
  - The security council deems that protocol growth is actively hindered by node operator supply; they may use information such as deposit pool state, rETH premium, etc to make this determination
- The pDAO SHALL modify voter_share as follows:
  - If <40% of RPL is contributing vote power in RP, the pDAO SHALL initiate a vote to increase `voter_share` with the aim of getting above 40%
    - It is SUGGESTED that `rpl_burn_share` is decreased by the same magnitude
  - If >85% of RPL is contributing vote power in RP, the pDAO SHALL initiate a vote to decrease `voter_share` with the aim of getting below 85%
    - It is SUGGESTED that `rpl_burn_share` is increased by the same magnitude
  - If 40-85% of RPL is contributing vote power in RP, the pDAO MUST NOT change `voter_share` 
  - Because this involves _voters_ modifying `voter_share`, there is an acknowledged conflict of interest here. As a result, changing the voter_share heuristic SHALL require a supermajority vote with at least 75% of the vote in support of any change.

## Optional heuristics
This section reflects some of the thinking at the time this RPIP was drafted. These ideas are explicitly _not_ binding/enforceable, and they may freely change over time/context.

- Consider `node_operator_commission_share` as a requirement to function. If this is not high enough to attract the supply we need, the protocol is non-functional. At the same time, if we have significantly more supply than needed, we may freely decrease it. Offset the increase/decrease with a commensurate decrease/increase of `rpl_burn_share`.
- Consider `voter_share` as a requirement to function. If this is not high enough to attract governance security, the protocol is endangered. This has a [specified heuristic](#specified-heuristics) above to follow. Offset the increase/decrease with a commensurate decrease/increase of `rpl_burn_share`.
- Finally, consider `rpl_burn_share`. 
  - If rETH demand has been robust enough to reach desired rETH TVL, `rpl_burn_share` can be increased (without other settings changes, this means at the cost of `reth_share`). This is one way to fulfill the soft limits described in [RPIP-17](./RPIP-17.md).
  - If below desired rETH TVL due to lack of rETH demand, `rpl_burn_share` can be decreased. The surplus can either be directed to `reth_share` (without other settings changes) or to `pdao_treasury_share` (with a commensurate increase). Here voters should be considering how to most efficiently generate rETH demand given a fixed amount of funds.

The premise here is that voters (who are all NOs and RPL holders) can operate fairly selfishly. They are strongly incentivized to keep node operation and rETH holding attractive -- these lead to strong TVL and thus capture a lot of value to RPL.

## Historic revenue share values
| Date                         | Share Settings                                                                                                       |
|------------------------------|----------------------------------------------------------------------------------------------------------------------|
| 2024-03-08<br>(ratified TBD) | `node_operator_commission_share`: 2.5%, `voter_share`: 1%, `rpl_burn_share`: 10.5%, `pdao_treasury_share`: 0%, `odao_share`: 0% |

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
