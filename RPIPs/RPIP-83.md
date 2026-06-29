---
rpip: 83
title: Increase Megapool Bond Requirements
description: Increase bond requirement for megapools and reroute rewards to top-up bond while below bond curve.
author: knoshua (@knoshua)
discussions-to: <URL>
status: Draft
type: Protocol
category (*only required for Protocol ): Core
created: 2025-05-31
---
## Abstract

Increase per validator bond requirement for megapools to 6 ETH. Use megapool rewards to top-up bond until back on bond curve.


## Motivation

Currently there is not enough rETH demand to allow existing node operators to switch to megapools with 4 ETH validators and a large share of the protocol remains in minipools. These do not contribute to `voter_share` and hurt RPL tokenomics. UARS also doesn't apply to minipools, so the ability to adjust commission remains limited. 

RPIP-71 proposes to prioritize minipools for liquidity exits. With limited rETH demand and the high efficiency of 4 ETH megapool validators, this could lead to a large share of node operators removed from the protocol and unable to rejoin. The large node operator set is a strength of rocket pool and if rETH demand improves long-term, keeping more node operators actively staking may be beneficial. 

## Specification

- `base_bond_array` SHALL be set to `[6, 12]` and `reduced_bond` SHALL be set to 6 ETH.
- When a node operator creates a validator, with `i` validators in the megapool prior to adding:
	- If `0 < i < base_bond_array.length`, the required `user_deposit` SHALL be changed to the minimum between `base_bond_array[i] - bas_bond_array[i-1]` and the bond curve specified in RPIP-42.
	- If  `i ≥ base_bond_array.length`, the required `user_deposit` SHALL be changed to the minimum between `reduced_bond` and the bond curve specified in RPIP-42.
- There SHALL be a `topUpBond` method that allows increasing `nodeBond` to match the current bond curve.
- When rewards are distributed for a megapool where the `nodeBond` is below the node bond requirement, the `nodeAmount` SHALL first be used to increase the `nodeBond` up to the bond requirement and associated ETH SHALL be send to rETH.

## Rationale

‎The bond requirement is increased to speed up the transition to megapools and keep more node operators in the protocol. At the level of rETH demand that is sufficient to allow a given amount of NO ETH to be completely on 6 ETH megapool validators, we'd still have:
- 63.2% of NO ETH in minipools and 42.2% of user ETH in minipools
- or 4 ETH megapool validators only and 38% of NO ETH unable to stake

‎Saturn 1 didn't plan for increasing the bond. to address this for megapools with existing 4 ETH validators, staking rewards are used to top up bond instead of being paid out. They still belong to the node operator. Voter share is still paid out to avoid complexity. It will take a long time (over 10 years) for this process to get a node back to thebond curve. Therefore, pDAO may want to consider bond level as exit criterion for megapools as well.
‎
### Time until 4 ETH validator bond is restored to 6 ETH from rewards

- assuming 2.75% solo staking APR, 5% NO_reward (3,7125% APR)
- not full compounding (bond goes up)
- assuming we can't get to `voter_share`
Somewhere between 11 and 13 years. Very slow, probably should consider exiting underbonded megapools first for liquidity as well.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
