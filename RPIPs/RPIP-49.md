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
1. [Bond curves](RPIP-42.md) - this is perhaps the most critical component, as it unlocks dramatically higher capital efficiency. Note that this RPIP also includes (a) the ability to penalize at the node level and (b) the removal of the RPL staking requirement to create minipools.
2. [Megapools](RPIP-43.md) - this is needed for the bond curve changes, otherwise gas would be prohibitive gas for, eg, 1.5-ETH bond minipools.
3. [Forced exits](RPIP-44.md) - this is needed for the bond curve changes, to enable node level penalties to be effective.
4. [RPL Burn](RPIP-45.md) - this is the primary value capture mechanism being proposed.
5. [UVC](RPIP-46.md) - this is the mechanism that allows for splitting revenue between various targets (including voters to keep our governance robust). Note that it also has some thinking about how to find an appropriate share for NOs, as well as some future-looking settings that are not intended to be used immediately.
6. [Remove operator-gated upgrades](RPIP-47.md) - this is desirable eventually. Since it reduces the number of things that need to be tested going forward, the thought is we should include it as quickly as possible.

1 requires 2 and 3.
4 and 5 require each other.

## Potential deployment scenarios
We have some thought experiment scenarios. For all of these, the premise is that we'd do the governance for the whole Scenario before starting.

We'd love to get team feedback here, especially on ballpark timelines.

### Scenario A (1 release):
- Release 1: Components 1-6 at once.

### Scenario B (3 releases):
- Release 1: Megapools, Remove operator-gated upgrades
  - Allows ETH-only participants
  - Allows NOs to migrate to 4-ETH minipools for greater efficiency
  - Allows NOs to move to Megapools for greater efficiency
    - Note that this implies very high earnings for ETH-only minipools until Release 2
- Release 2: RPL burn, UVC
  - New RPL value capture comes online
  - Revenue steering controls come online
- Release 3: Bond curves, Forced exits
  - Dramatically improve capital efficiency 

### Scenario C (2 releases):
- Release 1: Megapools, Remove operator-gated upgrades, RPL burn, UVC
  - Allows ETH-only participants
  - Allows NOs to migrate to 4-ETH minipools for greater efficiency
  - Allows NOs to move to Megapools for greater efficiency
  - New RPL value capture comes online
  - Revenue steering controls come online
- Release 2: Bond curves, Forced exits
  - Dramatically improve capital efficiency 

### Scenario D (2 releases):
- Release 1: Megapools, Remove operator-gated upgrades
  - Allows ETH-only participants
  - Allows NOs to migrate to 4-ETH minipools for greater efficiency
  - Allows NOs to move to Megapools for greater efficiency
    - Note that this implies very high earnings for ETH-only minipools until Release 2
- Release 2: RPL burn, UVC, Bond curves, Forced exits
  - New RPL value capture comes online
  - Revenue steering controls come online
  - Dramatically improve capital efficiency 


## Components being left for future consideration
- Interim solutions - we can better consider these _after_ we've established a goal and a timeline.
- MEV penalty improvements - this _is_ important, but the numbers we've seen previously suggest MEV theft has not been a pressing problem.
- rETH protection - this is moderate priority and significant complexity. While I wish to see it, I don't think it's worth risking timeline on the [components above](#components-currently-being-advocated-for).
- Right now voter share is split up amongst potential voters based on amount of RPL eligible to vote. We could do something based on active governance or actual vote power instead of count of RPL?
  - Possible thought: require having a delegate set for eligibility
  - Possible thought: setting a delegate expires after a time period (eg, 1 year)
  - Possible thought: if a delegate has x% of vote power, prevent new delegations to them (revert with error)
  - Simplicity is being favored more than these; however, this may be something to revisit, depending on the actual practice of governance observed

## Known points that merit more discussion
- [RPL Burn](RPIP-45.md) vs [RPL Buy & LP](RPIP-buy_lp.md)
- 3-transaction deposits for greater queue efficiency
- RPL inflation
  - The simplest possibility is to remove RPL rewards and the corresponding 3.5% of inflation
  - Depending on release schedule, it might be useful to continue paying RPL rewards to legacy minipools. This would slow down a migration of NOs, and thus allow uptake of new NOs.
  - Another possibility discussed is using RPL inflation to fulfill the role currently done by "voter share"
- Cashing out credit as rETH
- More thinking about current NOs moving to megapools while still attracting new NOs
  - One thought is to limit the current RPL rewards structure to legacy minipools. This could look like:
    - Only legacy minipools are counted for RPL rewards
    - We SHALL remove RPL rewards (and their inflation share) within one year of megapool release
    - We MAY remove RPL rewards (and their inflation share) earlier, to encourage legacy pool migration


## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
