---
rpip: 49
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

The overall package is based on, but not exactly the same as the [early-March proposal from Valdorff's github](../assets/rpip-49/readme.md). This proposal was a significant improvement based off of discussions with many people (thanks to 🏆samus🏆, sckuzzle, epineph, uisce, langers, NonFungibleYokem, MountainB, knoshua, luominx, ArtDemocrat and many others). This proposal was presented at Rocket Pool's "Denver Lift Off" event by Valdorff and Samus (see [the presentation](https://docs.google.com/presentation/d/12WRXuZktEtViwBWxFwm8OHpwpgoOpAF01859o0jGkiw) or its [powerpoint backup](../assets/rpip-49/On%20The%20Horizon%20(backup%20version).pptx)).

## Components currently being advocated for
1. [RPIP-42: Bond curves](RPIP-42.md) - this is perhaps the most critical component, as it unlocks dramatically higher capital efficiency. Note that this RPIP also includes (a) the ability to penalize at the node level and (b) the removal of the RPL staking requirement to create validators.
2. [RPIP-43: Megapools](RPIP-43.md) - this is needed for the bond curve changes, otherwise gas would be prohibitive for, eg, 1.5-ETH bond validators.
3. [RPIP-44: Forced exits](RPIP-44.md) - this is needed for the bond curve changes, to enable node level penalties to be effective.
4. [RPIP-45: RPL Burn](RPIP-45.md) - this is the primary value capture mechanism being proposed.
5. [RPIP-46: UVC](RPIP-46.md) - this is the mechanism that allows for splitting revenue between various targets (including voters to keep our governance robust). Note that it also has some thinking about how to find an appropriate share for NOs, as well as some future-looking settings that are not intended to be used immediately.
6. [RPIP-47: Forced delegate upgrades](RPIP-47.md) - this is desirable eventually. Since it reduces the number of things that need to be tested going forward, the thought is we should include it as quickly as possible.

## Topics that need feedback, discussion, or fleshing out
- [RPL Burn](RPIP-45.md) vs [RPL Buy & LP](RPIP-50.md)
  - Probably worth it to do some LP modeling to gain understanding, see [discord](https://discord.com/channels/405159462932971535/1215788197842255972/1224125945191989349)
- 3-transaction deposits for greater queue efficiency (see the 2nd bullet of [this section from the early-March proposal](../assets/rpip-49/readme_tier3.md#other-considerations>))
- The voter_share heuristic: as it's written, we would increase the share if less than 40% of RPL is vote-eligible and decrease the share if more than 85% of RPL is vote-eligible. There is a step of 10% written in.
- RPL inflation
  - The simplest possibility is to remove RPL rewards and the corresponding 3.5% of inflation
  - Depending on release schedule, it might be useful to continue paying RPL rewards to legacy minipools. This would slow down a migration of NOs, and thus allow uptake of new NOs.
  - Another possibility discussed is using RPL inflation to fulfill the role currently done by "voter share"
- Cashing out credit as rETH (including using this as a way to exit queue if "stuck")
- Unstaking RPL, right now this is only allowed (per RPIP-30) down to 15% borrowed ETH. It seems this should probably go to 0?? Needs discussion.
- Interim solutions (note: we can better consider these _after_ we've established a rough goal and a timeline)

## Topics not being addressed in this rework
- MEV penalty improvements - this _is_ important, but the numbers we've seen previously suggest MEV theft has not been a pressing problem.
- rETH protection - this is moderate priority and significant complexity. While I wish to see it, I don't think it's worth risking timeline on the [components above](#components-currently-being-advocated-for).
- Right now voter share is split up amongst potential voters based on amount of RPL eligible to vote. We could do something based on active governance or actual vote power instead of count of RPL?
  - Possible thought: require having a delegate set for eligibility
  - Possible thought: setting a delegate expires after a time period (eg, 1 year)
  - Possible thought: if a delegate has x% of vote power, prevent new delegations to them (revert with error)
  - Simplicity is being favored more than these; however, this may be something to revisit, depending on the actual practice of governance observed

## Potential deployment scenarios
We have some thought experiment scenarios. For all of these, the premise is that we'd do the governance for the whole Scenario before starting. We'd love to get team feedback here, especially on ballpark timelines.

In order to avoid messy conditionals in the rest of the RPIPs, we will be operating with "Scenario C" as the best guess.

### Scenario A (1 release):
- Release 1: Components 1-6 at once.

### Scenario B (3 releases):
- Release 1: Megapools, Forced delegate upgrades
  - Allows ETH-only participants
  - Allows NOs to move to Megapools for greater efficiency
  - Allow 4-ETH validators within Megapools for greater efficiency
- Release 2: RPL burn, UVC
  - New RPL value capture comes online
  - Revenue steering controls come online
- Release 3: Bond curves, Forced exits
  - Dramatically improve capital efficiency 

### Scenario C (2 releases; currently assumed for all RPIPs):
- Release 1: Megapools, Forced delegate upgrades, RPL burn, UVC
  - Allows ETH-only participants
  - Allows NOs to move to Megapools for greater efficiency
  - Allow 4-ETH validators within Megapools for greater efficiency
  - New RPL value capture comes online
  - Revenue steering controls come online
- Release 2: Bond curves, Forced exits
  - Dramatically improve capital efficiency 

### Scenario D (2 releases):
- Release 1: Megapools, Forced delegate upgrades
  - Allows ETH-only participants
  - Allows NOs to move to Megapools for greater efficiency
  - Allow 4-ETH validators within Megapools for greater efficiency
- Release 2: RPL burn, UVC, Bond curves, Forced exits
  - New RPL value capture comes online
  - Revenue steering controls come online
  - Dramatically improve capital efficiency

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
