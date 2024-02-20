---
rpip: 20
title: Updated Incentive Management Committee Charter
description: Supersedes RPIP-11; describes what the Incentives Management Committee is for and how it will execute
author: Valdorff (@Valdorff)
discussions-to: https://dao.rocketpool.net/t/add-an-rpl-related-goal-to-imc-charter/1656/
status: Final
type: Meta
created: 2023-04-17
requires: 10
vote-to: https://vote.rocketpool.net/#/proposal/0x78fb23da34837170bb182f21fb57e4a023f7c52c1379751e824a348467f96a65
vote-date: 2023-05-16
vote-result: Passed
---

## Abstract
There is broad agreement on the need for liquidity incentives that @knoshua outlines in the
[Liquidity Incentives](https://dao.rocketpool.net/t/liquidity-incentives) forum post. We would like
to suggest an outline for an Incentives Management Committee (hereafter IMC) outlining guiding
principles and governance. Please see [RPIP-10](https://rpips.rocketpool.net/RPIPs/RPIP-10) for governance details that apply to all Management
Committees.

## Motivation
The Rocket Pool protocol has had various phases of growth in its history. These phases have been
characterized by an imbalance towards an abundance of Node Operators or an abundance of rETH demand.
Finding a harmonious balance was the goal of the initial commission system, however, upon abandoning
that the protocol lost its only variable way to influence rETH demand.

The protocol is billed as a Liquid Staking Solution and thus the protocol has an obligation to hold
certain values. These principles will help build confidence that Rocket Pool aims to truly be liquid.

In addition to the initial goals captured in [RPIP-11](https://rpips.rocketpool.net/RPIPs/RPIP-11), this update captures a secondary need for RPL
liquidity, especially for security.

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
- As a lesser goal, the IMC SHOULD support RPL/ETH TWAP oracle safety
- As a lesser goal, the IMC MAY act to improve accessibility of RPL; efforts for this goal SHOULD
  NOT exceed 10% of resources
- The IMC SHOULD be sensitive to market actions and adjust emissions accordingly to reasonably
  maintain the aforementioned principles in adverse markets.

### Governance
In addition to the "Management Committee Governance" section of RPIP-10, the IMC will follow the
following:
- Initial allowlisted member addresses SHALL be nominated through an informal method on the forum
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
