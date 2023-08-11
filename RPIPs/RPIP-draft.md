---
rpip: draft
title: RPL Staking Rework
description: Modifies RPL staking rewards to better align with protocol goals
author: Valdorff (@Valdorff)
discussions-to: https://dao.rocketpool.net/t/rpl-staking-rework-proposal/2090
status: Draft
type: Protocol
category: Core
created: 2023-08-11
---

## Abstract
RPL inflation can be seen as protocol revenue that we steer to various places to achieve protocol
goals. This proposal looks to better align spending with protocol goals, specifically with the
portion of inflation (70%) that goes to Node Operators today.

**The primary goal of the proposal is to increase spend aligned with rETH supply creation.** This
means both rewarding based on minipool count and rewarding LEB8s over LEB16s (as they create more
rETH supply). Mechanically speaking, this means rewarding the minimum and near-minimum staked RPL
more, and basing rewards on borrowed ETH.

Secondarily, the proposal also aims to improve quality of life by avoiding locking people in far
from their desired asset allocation (which may lead to churn as folks exit to rebalance), retain
enough friction to avoid adding market volatility, and phase the new rules in slowly to minimize
market impacts.

## Rationale
**Please** see the proposal document on
[Valdorff's github](https://github.com/Valdorff/rp-thoughts/tree/main/rpl_staking), or its backup
printout [here](../assets/rpip-draft/rpl_staking_readme.pdf). It greatly expands on the context, 
reasoning, and impacts for this proposal.

As a very high level summary, the below pie charts show overall intended spend before and after this
proposal. Mechanically, the blue categories are RPL staked from 10-15% of borrowed ETH value
(incentivizing), RPL staked from 15-30% of borrowed ETH value (overshoot), and RPL staked above 30%
of borrowed ETH value (speculation).

| ![image](../assets/rpip-draft/overall_spend_pie_curr.png) | ![image](../assets/rpip-draft/overall_spend_pie_prop.png) |
|:---------------------------------------------------------:|:-------------------------------------:|

The proposal focuses a lot more spend on incentivizing minipool creation, while still spending a
significant amount on the speculative and hands-off categories of RPL staking (compare with Dev
spend, for example). The current outsized speculation category (larger than Dev, oDAO, IMC, GMC,
and Reserves combined) has been significantly reduced so that we can spend a lot more on achieving
the protocol goal of attracting minipool creation (and rETH supplying).

## Specification

Definition: X=1 is the first reward snapshot after (a) the end of the vote and (b) the
implementation of the new rewards rules. X=2 is the snapshot after that, etc.

- The new rewards rules SHALL be phased in such that, for reward snapshot X:
  - For periods X=1 to X=5: a nodes' share of rewards is
    `(X/6)*proposed_method_share + ((6-x)/6)*current_method_share`
  - For periods X>5, they SHALL  simply be `proposed_method_share`
  - `current_method_share` SHALL determine share of NO rewards among NOs according to the latest tree
    spec when the vote is passed
  - `proposed_method_share` SHALL be defined as:
    - For one node, its `node_weight` divided by the sum of all `node_weight` across nodes
    - If staked RPL value in ETH is <10% borrowed ETH
      - `node_weight=0`
    - If staked RPL value in ETH is (>= 10% borrowed ETH) and (<=15% borrowed ETH)
      - `node_weight=100 * staked_rpl_value_in_eth`
    - If staked RPL value in ETH is > 15% borrowed ETH
      - `node_weight = (13.6137 + 2 * ln(100 * (staked_rpl_value_in_eth / borrowed_eth) - 13)) * borrowed_eth`
      - This value MAY be approximated if necessary
- The next significant smart contract upgrades SHALL update the withdrawal process
  - Withdrawals SHALL be a 2-step process
    - A user MAY set some amount of RPL to "withdrawing"
    - Once RPL has been in the withdrawing state for 28 days, it MAY be withdrawn from the protocol
      - All "withdrawing" RPL MUST be withdrawn at one time
      - The 28-day value SHOULD be a pDAO-controlled setting that starts at 28 days
    - There SHALL only be a single time stored for "withdrawing" RPL; this means that if a user sets
      additional RPL to withdrawing after some RPL is already "withdrawing", they must wait for 28
      days after the later action
    - "Withdrawing" RPL SHALL NOT be eligible for RPL rewards or voting power
  - If this smart contract upgrade has not yet happened, the pDAO SHALL update the withdrawal
    threshold as follows:
    - Within the two weeks after X=3 rewards go out, set `node.per.minipool.stake.maximum` to 1.0
    - Within the two weeks after X=6 rewards go out, set `node.per.minipool.stake.maximum` to 0.6
- Except for not including "withdrawing" RPL, there SHALL NOT be any changes to voting power based
  on this RPIP

## Additional Considerations
A significant concern from some community members was that it may cause some folks to downgrade
their expectations of RPL and decide to sell. The author does not share this view and believes that
aligning spending with protocol goals improves what we should expect from both the protocol and the
token.

An analysis of possible fallout can be found in the `Which NOs are sensitive to RPL yield?` section
of the research document on
[Valdorff's github](https://github.com/Valdorff/rp-thoughts/blob/main/rpl_staking/research.md), or
its backup printout [here](../assets/rpip-draft/rpl_staking_research.pdf). The key conclusion is
that only up to ~4k ETH worth of RPL is sensitive to yield and in a range where they could improve
their yield by selling. Note that this does _not_ include the effect of RPL buying that is attracted
from (a) holders sensitive to yield in a range where they could improve their yield by buying or (b)
new joiners that are attracted by the greater incentives due to the now-higher spend on rETH supply.

## Acknowledgements
I'd like to explicitly acknowledge the many folks that participated in research discussion on this
idea. Please let me know if I missed you when scrolling through. In alphabetical order:

0xL2, 0xSheen, 5dayoldburrito, anisoptera.eth, astoneta.eth, bernard, bjobjo, Blackclaws, butta.eth,
Calurduran, Cayos, chessman, chris_marino, cowzow, d33t00, DagoDuck, David VI, Doublemint,
Dr Doofus, Epineph, Erowind, ewan.eth, feyo.eth, firegogo, Freek, FUINY7, fullwall, hackworth.eth,
halzen.eth, harpocryptes, Hollywood_Squares, ib1gymnast, Intelligent Investoor, invis, ivelin.eth,
james, Ken, knoshua, L3O, lilac, Ludi, luominx, lutro, mattkunke.eth, Ments, MetalSeed, mfilipe,
Mig, mikeylikesit, Myguy, NeverAnIsland, NickS, noahscape, NonFungibleYokem, notyou, occam,
philcares.eth, Pieter, pk3268, quant, rocket__1, RocketDoc.eth, rocknet, Rudes.eth, sadcuzz.eth,
Saint Rat, SANTINO33, sckuzzle, señor, slipstream, sneaky.eth, SNG, Snocones, socash, sonax,
StableScarab, takezo, Team Berebere, teamRick, techcapo, uisce, Wander, Waq, woodenship.eth,
wowsuchname.eth, Xero.eth, yeehaw0x, Yorick, ZQQLANDER

Some folks here chatted up a storm, reviewed models, and contributed their own models/essays. Some
folks came out of lurking just long enough to provide their perspective. Many were in between. All
are appreciated. It's stunning that we had 90 people participate in an early research thread.

<3 RP community.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
