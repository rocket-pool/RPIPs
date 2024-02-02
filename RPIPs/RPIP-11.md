---
rpip: 11
title: Incentive Management Committee Charter
description: Describes what the Incentives Management Committee is for and how it will execute
author: jasperthegovghost (@jasperthefriendlyghost), Valdorff (@Valdorff)
discussions-to: https://dao.rocketpool.net/t/pdao-liquidity-committee-and-budget-proposal/895
status: Obsolete
type: Meta
created: 2022-08-09
requires: 10
vote-to: https://vote.rocketpool.net/#/proposal/0x22a49c5384815358512fc8092da06f8f7e129fec6613a38f916ee8b17703014c
vote-date: 2022-08-25
vote-result: Passed
---

## \>\> NOTICE <<
This RPIP has been made obsolete by the passage of [RPIP-20](RPIP-20.md).

## Abstract
There is broad agreement on the need for liquidity incentives that @knoshua outlines in the
[Liquidity Incentives](https://dao.rocketpool.net/t/liquidity-incentives) forum post. We would like
to suggest an outline for an Incentives Management Committee (hereafter IMC) outlining guiding
principles and governance. Please see RPIP-10 for governance details that apply to all Management
Committees.

## Motivation
The Rocket Pool protocol has had various phases of growth in its history. These phases have been
characterized by an imbalance towards an abundance of Node Operators or an abundance of rETH demand.
Finding a harmonious balance was the goal of the initial commission system, however, upon abandoning
that the protocol lost its only variable way to influence rETH demand.

The protocol is billed as a Liquid Staking Solution and thus the protocol has an obligation to hold
certain values. These principles will help build confidence that Rocket Pool aims to truly be liquid.

## Specification

### Guiding Principles
- The IMC’s chief mission SHALL be to support low slippage exchange between ETH and rETH on
  major platforms where this trading occurs.
- The IMC SHALL expand access to rETH across major DeFi protocols.
- The IMC SHALL manage funds allocated to and/or rewarded to the pDAO for liquidity purposes.
  in accordance with any conditions those funds were received upon.
- The IMC SHOULD minimize costs and prioritize long-term capital-efficient strategies to build
  liquidity.
- The IMC SHOULD sustainably build rETH exchange volume.
- The IMC SHOULD support the oDAO rETH:ETH ratio as the focal point in designing liquidity
  incentives.
  - The IMC SHOULD NOT, however, add incentives that would serve _only_ to boost demand with
    marginal benefit from added liquidity.
- The IMC SHOULD be sensitive to market actions and adjust emissions accordingly to reasonably 
  maintain the aforementioned principles in adverse markets.

### Governance
In addition to the "Management Committee Governance" section of RPIP-10, the IMC will follow the
following:
- Initial whitelisted member addresses SHALL be nominated through an informal method on the forum
  and/or Discord.
- The initial nominated members SHALL be accepted by a separate snapshot vote from the one for this
  RPIP.
- The IMC Treasurer SHALL publish a budget detailing planned incentive spending one week before the 
  start of the next rewards period.
  - If the pDAO starts a vote between the release of the budget and the start of the next rewards 
  period which limits the IMC, the IMC SHALL NOT spend funds (to the extent possible) in the new
  rewards period until the vote has been resolved.
  - A vote SHALL be considered to "limit the IMC" if it: prevents or decreases disbursement of funds
  to the IMC, takes back existing funds from the IMC, or modifies the IMC membership.

## Rationale
The main goal is to establish a starting liquidity budget and provide for a quick initial state
where funds can be allocated and spent.

The community believes that it's important to start spending on liquidity incentives as quickly as
possible. The liquidity bucket amounts to 67.5k RPL, or 1,087 ETH at market prices, annually. For a
theoretical run of 1 reward period (28 days), this gives  ~83.6 ETH or ~140,200 USD to work with. In
light of parallel discussions about tokenomics changes, this amount serves as a conservative
starting point.

## Membership

Current and historic membership is recorded in [RPIP-36](RPIP-36.md#incentives-management-committee).

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
