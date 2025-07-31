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
- This specification SHALL be implemented in Saturn 2. 
- Pool stakers MUST be able to signal that they wish to exit at the protocol rate.
- The delay to burn rETH after such a signal MUST be short and bounded except for factors outside of protocol control.
    - There MUST NOT be any required actions by node operators to ensure a short delay.
- Validators exited via this RPIP SHALL be randomly chosen.

## Rationale
Node Operator performance was ommitted from the selection criteria due to the necessity to comprehensively address this topic independently from the unique circumstances where withdrawal initiated exits are enacted. Other means, such as Node Operator defined commission, staked RPL exposure, or megapool size were ommitted to maintain simplicity.

## Security Considerations
Due consideration may need to be paid to any opportunities for gaming the exit system. Additionally, the incentives for node operators to continue staking with Rocket Pool should be considered (with the pool staker incentives given priority in case of conflict).

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
