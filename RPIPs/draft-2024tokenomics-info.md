---
rpip:
title: 2024 Tokenomics Rework Info
description: Allow the revenue from borrowed ETH to be split different ways
author: Valdorff (@Valdorff)
discussions-to: TBD
status: Draft
type: Informational
created: 2024-03-08
---

## Intro
This Info RPIP exists to explain an overall tokenomics proposal, the RPIPs it has been split up into, the reasoning between including certain items now vs later, etc.

The overall package is based on, but not exactly the same as the [early-March proposal from Valdorff's github](../assets/rpip-2024tokenomics-info/readme.md). This proposal was a significant improvement based off of discussions with many people (thanks to 🏆samus🏆, sckuzzle, epineph, uisce, langers, NonFungibleYokem, MountainB, knoshua, luominx, ArtDemocrat and many others). This proposal was presented at Rocket Pool's "Denver Lift Off" event by Valdorff and Samus (see [the presentation](https://docs.google.com/presentation/d/12WRXuZktEtViwBWxFwm8OHpwpgoOpAF01859o0jGkiw) or its [powerpoint backup](../assets/rpip-2024tokenomics-info/On%20The%20Horizon%20(backup%20version).pptx)).

## Components currently being advocated for
1. [Bond curves rpip](draft-bond-curves.md) - this is perhaps the most critical component, as it unlocks dramatically higher capital efficiency. Note that the ability to penalize at the node level is also specified in this RPIP.
2. [Megapools rpip](draft-megapools.md) - this is needed for the bond curve changes, otherwise gas would be prohibitive gas for, eg, 1.5-ETH bond minipools.
3. [Forced exits rpip](draft-forced-exits.md) - this is needed for the bond curve changes, to enable node level penalties to be effective.
4. [Burn rpip](draft-burn.md) - this is the primary value capture mechanism being proposed. Note that the ability to create minipools without staking RPL is also specified in this RPIP.
5. [UVC rpip](draft-uvc.md) - this is the mechanism that allows for splitting revenue between various targets (including voters to keep our governance robust). Note that it also has some thinking about how to find an appropriate share for NOs, as well as some future-looking settings that are not intended to be used immediately.
6. [Remove operator-gated upgrades](draft-remove-operator-gated-upgrades.md) - this is desirable eventually. Since it reduces the number of things that need to be tested going forward, the thought is we should include it as quickly as possible.

1 requires 2 and 3.
4 and 5 require each other.

## Components being left for future consideration
- Interim solutions - we can better consider these _after_ we've established a goal and a timeline.
- MEV penalty improvements - this _is_ important, but the numbers we've seen previously suggest MEV theft has not been a pressing problem.
- rETH protection - this is moderate priority and significant complexity. While I wish to see it, I don't think it's worth risking timeline on the [components above](#components-currently-being-advocated-for).
- Reduce RPL inflation and remove RPL rewards - this would strongly push current NOs to switch to megapools following the new bond curve. This may not be immediately desirable. The settings to achieve this do not require smart contract changes, which also reduces the need for immediate advocacy.

## Knonw points that merit more discussion
- Right now voter share is split up amongst potential voters based on amount of RPL eligible to vote. Should we be doing something based on active governance or actual vote power instead of count of RPL?
  - Possible thought: require having a delegate set for eligibility
  - Possible thought: setting a delegate expires after a time period (eg, 1 year)
  - Possible thought: if a delegate has x% of vote power, prevent new delegations to them (revert with error)
- 3-transaction deposits for greater queue efficiency
- Cashing out credit as rETH
- More thinking about current NOs moving to megapools while still attracting new NOs

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
