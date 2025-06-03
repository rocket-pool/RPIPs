---
rpip: 71
title: rETH Withdrawal liquidity via EIP-7002
description: Enable Rocket Ether stakers to trigger validator exits to access protocol liquidity for unstaking from Rocket Pool.
author: Ramana (@xrchz), Samus (@orangesamus)
discussions-to: <URL> TODO
status: Draft
type: Protocol
category: Core
created: 2025-05-14
requires (*optional): 44
---

## Abstract
Rocket ether is by design fully backed by staked ether, so that pool stakers with Rocket Pool can be confident that (modulo a major slashing event) their funds are safe with the protocol and can in principle be retrieved. However, there is no direct mechanism by which pool stakers can signal that they wish to access ether locked in the protocol. As a result, they are left only with the slow indirect method of selling their rETH on the market leading to incentives for node operators to voluntarily exit their validators to capture a market discount for rETH. With the adoption of [EIP-7002](https://eips.ethereum.org/EIPS/eip-7002) by Ethereum, it is possible in principle for Rocket Pool to capture a direct signal from pool stakers that they wish to unstake their ether and automatically trigger appropriate validator exits to make this happen in a timely fashion. Delays for unstaking will then come only from factors truly outside of Rocket Pool's control (such as the beacon chain exit queue).

## Motivation
Rocket ether is an unnecessarily risky product for any investor who might want to have reliable and timely access to their capital, since it can remain locked up on the beacon chain by the permissionless node operator set of Rocket Pool until they choose to exit. This RPIP seeks to remedy this situation and thereby make Rocket ether a more attractive and competitive product in the marketplace of Liquid Staking Tokens.

## Specification
We leave technical implementation details to the discretion of the development team, as long as they conform to the following desiderata:
- This specification SHALL be implemented in Saturn 2. 
- Pool stakers MUST be able to signal that they wish to exit at the protocol rate.
- The delay to burn rETH after such a signal MUST be short and bounded except for factors outside of protocol control.
    - There MUST NOT be any required actions by node operators to ensure a short delay.
- The next validator chosen to exit to provide protocol liquidity for pool stakers SHALL be associated with a megapool which meets the criteria below:
    - Definitions
        - `megapool_is_small_condition` SHALL be defined as `16 ETH`
        - For each megapool:
            - `megapool_rpl_eth_ratio` SHALL be defined as `megapool_staked_rpl / megapool_bonded_eth`.
                - `megapool_rpl_eth_ratio` MAY be rounded to the nearest integer value.
            - `megapool_is_small` SHALL be defined as `TRUE` if `megapool_bonded_eth <= megapool_is_small_condition`; `FALSE` otherwise.
    - Selection criteria
    1. The megapool with the smallest `megapool_rpl_eth_ratio` SHALL be chosen.
    2. If more than one megapool qualifies step 1, the megapool with `megapool_is_small = FALSE` SHALL be chosen.
    3. If more than one megapool qualifies step 2, the megapool SHALL be chosen randomly from this set. 
    4. If there is no megapool that qualifies step 2, the megapool with the largest `megapool_bonded_eth` value SHALL be chosen.
    5. If more than one megapool qualifies step 4, the megapool SHALL be chosen randomly from this set.
- If a validator SHALL be "chosen randomly" as referenced previously, the selection SHOULD include a seed that attempts to make the selection difficult for an actor to predict and/or bias.
- The oDAO MAY be trusted to implement the selection criteria previously defined, to decide which validators will be exited.

## Rationale
The Rocket Pool DAO is too slow to come up with full specifications for this critical protocol feature, and meanwhile rETH is rapidly losing market share. We need to defer to an aligned and competent team to solve this problem for us.

Staked RPL was chosen as the primary selection criteria since it provides a simple mechanism for Node Operators to signal their preference for avoiding involuntary ejections, further reward protocol aligned actors, and further incentivize governance in the protocol where even a small amount of staked RPL can significantly benefit the Node Operator.
Small megapools was chosen as the secondary selection critera since the health of Rocket Pool and Ethereum depend on decentralization and the contributions of independent actors, and the node operator experience of having an involuntarily ejected validator is increasingly worse the less bonded ETH you have. The value of 16 ETH was chosen for `megapool_is_small_condition` since it avoids incentivizing sock puppet behavior once bond curves are in place (as it requires node operators to sacrifice roughly half the difference in boosted APY they could earn by consolidating larger amounts of ETH to the same megapool). See the Security Considerations section in [RPIP-42](./RPIP-42.md) for further reference.

Node Operator performance was ommitted from the selection criteria due to the necessity to comprehensively address this topic independently from the unique circumstances where withdrawal initiated exits are enacted. Other means, such as Node Operator defined commission were ommitted to maintain simplicity and ensure clarity of messaging for the chosen selection criteria.

## Security Considerations
Due consideration may need to be paid to any opportunities for gaming the exit system. Additionally, the incentives for node operators to continue staking with Rocket Pool should be considered (with the pool staker incentives given priority in case of conflict).

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
