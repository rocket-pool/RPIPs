---
rpip: 49
title: 2024 Tokenomics Rework Info
description: Allow the revenue from borrowed ETH to be split different ways
author: Valdorff (@Valdorff)
discussions-to: TBD
status: Draft
type: Informational
created: 2024-03-08
tags: tokenomics-2024
---

## Intro
This Info RPIP exists to explain an overall tokenomics proposal, the RPIPs it has been split up into, the reasoning between including certain items now vs later, etc.

The overall package is based on, but not exactly the same as the [early-March proposal from Valdorff's github](../assets/rpip-49/readme.md). This proposal was a significant improvement based off of discussions with many people (thanks to 🏆samus🏆, 🏆sckuzzle, 🏆epineph, knoshua, uisce, langers, NonFungibleYokem, MountainB, luominx, ArtDemocrat and many others). This proposal was presented at Rocket Pool's "Denver Lift Off" event by Valdorff and Samus (see [the presentation](https://docs.google.com/presentation/d/12WRXuZktEtViwBWxFwm8OHpwpgoOpAF01859o0jGkiw) or its [powerpoint backup](../assets/rpip-49/On%20The%20Horizon%20(backup%20version).pptx)).

## Components currently being advocated for
1. [RPIP-42: Bond curves](RPIP-42.md) - this is perhaps the most critical component, as it unlocks dramatically higher capital efficiency. Note that this RPIP also includes the ability to penalize at the node level.
2. [RPIP-43: Megapools](RPIP-43.md) - this is needed for the bond curve changes, otherwise gas would be prohibitive for, eg, 1.5-ETH bond validators. Note that this RPIP also includes the ability to create validators without staking RPL.
3. [RPIP-44: Forced exits](RPIP-44.md) - this is needed for the bond curve changes, to enable node level penalties to be effective.
4. Surplus revenue distribution: one of [RPIP-45: RPL Burn](RPIP-45.md), [RPIP-50: RPL LP](RPIP-50.md), or using higher `voter_share` (see [RPIP-46](RPIP-46.md)) - this is the proposed mechanism to distribute revenue beyond that used for protocol operation
5. [RPIP-46: Universal Adjustable Revenue Split](RPIP-46.md) - this is the mechanism that allows for splitting revenue between various targets (including voters to keep our governance robust). Note that it also has some thinking about how to find an appropriate share for NOs, as well as some future-looking settings that are not intended to be used immediately.
6. [RPIP-47: Forced delegate upgrades](RPIP-47.md) - this is desirable eventually. Since it reduces the number of things that need to be tested going forward, the thought is we should include it as quickly as possible.

## Topics that need feedback, discussion, or fleshing out
- Select a value capture method[RPL Burn](RPIP-45.md) vs [RPL Buy & LP](RPIP-50.md) vs higher `voter_share` (see [RPIP-46](RPIP-46.md))
  - Probably worth it to do some LP modeling to gain understanding, see [discord](https://discord.com/channels/405159462932971535/1215788197842255972/1224125945191989349)
  - Will need a snapshot, see [forum](https://dao.rocketpool.net/t/tokenomic-rework-vibe-check-surplus-revenue-redistribution/2912/11)
- 3-transaction deposits for greater queue efficiency (see the 2nd bullet of [this section from the early-March proposal](../assets/rpip-49/readme_tier3.md#other-considerations>))

## Deployment plan

## Saturn 1
Megapools, Forced delegate upgrades, Bond curves (framework), UARS, RPL value capture
  - Allows ETH-only participants
  - Allows NOs to move to Megapools for greater efficiency
  - Allow 4-ETH validators within Megapools for greater efficiency
  - UARS, except for voter_share targeting
  - RPL value capture online (burn, or LP, or higher voter_share); voter_share could also be an interim option until Saturn 2

## Saturn 2
Bond curves (low bond), Forced exits, UARS, RPL value capture
  - Bond curves: Dramatically improve capital efficiency with 1.5 ETH `reduced_bond`
  - UARS, in toto
  - RPL value capture update: if voter_share was used as an interim option, update to burn or LP 

Some of the other RPIPs will have specifications that take effect "with Saturn 2", and are designed to be spaced from Saturn 1.
These specifications should take effect:
- When Saturn 2 is released, and 4 months have passed since Saturn 1; or
- When Saturn 2 is released, and the pDAO votes to have a specification take effect (this requires a vote per specification or group of specifications)

## Topics that can be addressed after main votes, and before Saturn 2 dev
- [RPIP-44: Forced exits](RPIP-44.md) has been written very minimally. We should likely improve it to handle abandonment and/or bad performance. This requires thinking of good rules, a method for retrieving beacon chain balance, and likely an implementation for incentivized keepers.
- Interim solutions (note: we can better consider these _after_ we've established a rough goal and a timeline)

## Topics not being addressed in this rework
- DAO portion of inflation (30% of the current 5% per annum)
  - There were some discussions about whether we are better off continuing to fund things with RPL or ETH as we get to maturity. This issue isn't pressing. For now we'll continue using RPL inflation. We can discuss comfortably later and vote as desired.
- MEV penalty improvements - this _is_ important, but the numbers we've seen previously suggest MEV theft has not been a pressing problem.
- rETH restitution from underperforming Node Operators - this is moderate priority, significant complexity, and has some different variants possible (see [ArtDemocrat's original proposal](https://dao.rocketpool.net/t/rapid-research-incubator-submission-reth-protection-through-rpl-rerouting-deflation/2599) or [Val's tweak](https://dao.rocketpool.net/t/rapid-research-incubator-submission-reth-protection-through-rpl-rerouting-deflation/2599)). While I wish to see it, I don't think it's worth risking timeline on the [components above](#components-currently-being-advocated-for).
- Right now voter share is split up amongst potential voters based on amount of RPL eligible to vote. We could do something based on active governance or actual vote power instead of count of RPL?
  - Possible thought: require having a delegate set for eligibility
  - Possible thought: setting a delegate expires after a time period (eg, 1 year)
  - Possible thought: if a delegate has x% of vote power, prevent new delegations to them (revert with error)
  - Simplicity is being favored more than these; however, this may be something to revisit, depending on the actual practice of governance observed

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
