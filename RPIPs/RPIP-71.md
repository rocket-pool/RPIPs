---
rpip: 71
title: rETH Withdrawal liquidity via EIP-7002
description: Enable Rocket Ether stakers to trigger validator exits to access protocol liquidity for unstaking from Rocket Pool.
author: Ramana (@xrchz)
discussions-to: <URL> TODO
status: Draft
type: Protocol
category: Core
created: 2025-05-14
requires (*optional): 44
---

## Abstract
Rocket ether is by designed fully backed by staked ether, so that pool stakers with Rocket Pool can be confident that (modulo a major slashing event) their funds are safe with the protocol and can in principle be retrieved. However, there is no direct mechanism by which pool stakers can signal that they wish to access ether locked in the protocol. As a result, they are left only with the slow indirect method of selling their rETH on the market leading to incentives for node operators to voluntarily exit their validators to capture a market discount for rETH. With the adoption of [EIP-7002](https://eips.ethereum.org/EIPS/eip-7002) by Ethereum, it is possible in principle for Rocket Pool to capture a direct signal from pool stakers that they wish to unstake their ether and automatically trigger appropriate validator exits to make this happen in a timely fashion. Delays for unstaking will then come only from factors truly outside of Rocket Pool's control (such as the beacon chain exit queue).

## Motivation
Rocket ether is an unnecessarily risky product for any investor who might want to have reliable and timely access to their capital, since it can remain locked up on the beacon chain by the permissionless node operator set of Rocket Pool until they choose to exit. This RPIP seeks to remedy this situation and thereby make Rocket ether a more attractive and competitive product in the marketplace of Liquid Staking Tokens.

## Specification
We leave technical implementation details to the discretion of the development team, as long as they conform to the following desiderata:

- Pool stakers MUST be able to signal that they wish to exit at the protocol rate.
- The delay to burn rETH after such a signal MUST be short and bounded except for factors outside of protocol control.
    - There MUST NOT be any required actions by node operators to ensure a short delay.
- The priority selection of which validators to exit to provide protocol liquidity for pool stakers MAY take the following factors into consideration:
    - RPL stake of the node that owns the validator.
    - Performance of the validator, as determined permissionlessly via a beacon chain balance proof.
    - Size of the node that owns the validator, in terms of total node operator-provided ETH.
    - Commission percentage for the validator.
- The mechanism SHOULD be implemented in a timely fashion:
    - This feature SHOULD be included in the Saturn 2 upgrade with the minimal delay to the development roadmap required to accommodate it.
    - The team is RECOMMENDED to solicit and incorporate paid or voluntary help from the community if it would speed things along.

## Rationale
The Rocket Pool DAO is too slow to come up with full specifications for this critical protocol feature, and meanwhile rETH is rapidly losing market share. We need to defer to an aligned and competent team to solve this problem for us.

## Security Considerations
Due consideration may need to be paid to any opportunities for gaming the exit system. Additionally, the incentives for node operators to continue staking with Rocket Pool should be considered (with the pool staker incentives given priority in case of conflict).

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
