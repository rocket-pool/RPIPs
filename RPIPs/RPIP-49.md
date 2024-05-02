---
rpip: 49
title: 2024 Tokenomics Rework Info
description: Provides an introduction and overview to the 2024 community tokenomics rework, its likely contents, and its current status.
author: Valdorff (@Valdorff), LongForWisdom (@LongForWisdom)
discussions-to: https://dao.rocketpool.net/tag/tokenomics-rework
status: Living
type: Informational
created: 2024-03-08
tags: tokenomics-2024
---

## Abstract
This informational RPIP provides an introduction and overview to the 2024 community tokenomics rework, its likely contents, and its current status. It exists to explain the overall tokenomics proposal as it's currently envisioned by contributing community members. This RPIP lists and briefly describes the rework's components, and links out to the RPIPs that specify those components in greater detail.

The tokenomics rework will likely be split into two protocol upgrades, Saturn I and Saturn II. This RPIP describes the components that are expected to be included within each.

This Informational RPIP and the tokenomics rework represent the best efforts of the Rocket Pool community contributors involved. Information within this RPIP should not be considered official word from the Rocket Pool core development team. 

## Contents

### [RPIP-43: Megapools](RPIP-43.md)

A Megapool is a single contract that can be used as an ethereum withdrawal address for multiple validators.
* This allows for much more gas-efficient usage of the Rocket Pool protocol for Node Operators.
* This allows for the application of node-level penalities. 
* Importantly, this Megapool specification allows Node Operators to create ETH-only validators. 

Additionally, Megapools are required to facilitate the bond curve changes described in RPIP-42.

### [RPIP-42: Bond curves](RPIP-42.md)

The changes to bond curves allow Node Operators to provide smaller ETH bonds in a manner which does not impact the security of the Rocket Pool protocol. 
* This gives Node Operators the option of dramatically increasing their capital efficiency.
* This allows the Rocket Pool Protocol to support a greater amount of rETH. 

In order for bonds to be lowered securely for the protocol, initial bonds for a node must be larger than the minimum bond. 

### [RPIP-46: Universal Adjustable Revenue Split](RPIP-46.md)

This change allows the ETH revenue income from borrowed ETH to be split between various targets. The four targets are:
1. The Node Operator borrowing the ETH.
2. Node Operators generally, proportional to their share of voting RPL.
3. The surplus share, used to capture value to the RPL token.
4. The rETH share, going to rETH holders. 

The split is both adjustable by the pDAO, and may adjust automatically according to various heuristics.

### RPL Value Capture

Some form of value capture method will be included in the tokenomics rework package. Three options are being actively debated:
* [RPL Burn](RPIP-45.md) - Use the surplus share to buy and burn RPL.
* [RPL Buy & LP](RPIP-50.md) - Use the surplus share to buy RPL and deposit it in a liquidity pool. 
* Direct the surplus share to Node Operators, proportional to their share of voting RPL.

### [RPIP-44: Forced exits](RPIP-44.md)

This change allows the Rocket Pool protocol to force-exit Node Operators under certain circumstances. It relies on [EIP-7002](https://eips.ethereum.org/EIPS/eip-7002) being adopted by the Ethereum Protocol. 
* This allows Node Operators to easily exit their validators.
* This allows the Rocket Pool Protocol to exit badly performing or malicious validators.

The ability to force-exit misbehaving validators is a requirement for RPIP-42. 

### [RPIP-47: Forced delegate upgrades](RPIP-47.md)

This change allows the Rocket Pool protocol to force-upgrade Node Operators minipool delegate contracts after a grace period has expired. 
* This reduces the compatibility debt incurred by the protocol as it is upgraded because it does not need to support all prior iterations. 
* This means that protocol governance can make changes that benefit the protocol as a whole even if the changes do not benefit all individual Node Operators. 

## Deployment Plan

The tokenomics reworks package will likely be split between two protocol upgrades: Saturn I and Saturn II.

### Saturn I

* [RPIP-43: Megapools](RPIP-43.md)
  * Including ETH-only validators.
* [RPIP-42: Bond curves](RPIP-42.md)
  * Framework
  * 4ETH minimum bond
* [RPIP-46: Universal Adjustable Revenue Split](RPIP-46.md)
  * All but heuristic adjustments.
* RPL Value Capture - [RPL Burn](RPIP-45.md) / [RPL Buy & LP](RPIP-50.md) / Increased share to voting Node Operators.
* [RPIP-47: Forced delegate upgrades](RPIP-47.md)

### Saturn II

* [RPIP-42: Bond curves](RPIP-42.md)
  * 1.5ETH minimum bond. 
* [RPIP-44: Forced exits](RPIP-44.md)
* [RPIP-46: Universal Adjustable Revenue Split](RPIP-46.md)
  * Heuristic adjustments.

## Current Status

### Estimated Process

The below is generally agreed to be the steps to be completed before we can consider this 'decided'.

1. Complete the first draft of the full proposal specifications. 
2. Seek feedback from highly engaged community members as to the draft specifications. **<-- We are approximately here.**
3. Create high-level explanations and informational material for the full proposal for consumption by less engaged community.
4. Make a concerted effort to gather feedback via the forum from less engaged community.
5. Update the proposal and specifications as needed taking into account community feedback.
6. Run a forum temperature check vote on the full proposal (bar value capture mechanism).
7. Run a snapshot vote on the full proposal (bar value capture mechanism).
7. Run a snapshot vote on the value capture mechanism once oustanding blockers are cleared.
9. Update the proposal to incorporate the value capture mechanism vote result.

### Outstanding Items

TBC

## Excluded Components
The below components have been discussed, but are not currently considered to be high enough priority to be included in the tokenomics rework plan.
* MEV penalty improvements
* rETH restitution from underperforming Node Operators
* Adjusting DAO portion of RPL inflation

## Further Links
* This proposal was presented at Rocket Pool's "Denver Lift Off" event by Valdorff and Samus - [Presentation](https://docs.google.com/presentation/d/12WRXuZktEtViwBWxFwm8OHpwpgoOpAF01859o0jGkiw), [Powerpoint Backup](../assets/rpip-49/On%20The%20Horizon%20(backup%20version).pptx), [Recorded Presentation](https://www.youtube.com/watch?v=nyqrilFtlrc&list=PLKzACASsJiuXc0v6kZambks4cPaSVbekf&index=4)
* Samus is maintaining a google sheet containing notes, feedback and TODOs which can be found [here](https://docs.google.com/spreadsheets/d/1qmGBCPAX-IqcFFjUzBib2Z_NKo_Yh5U00zKnGpyNak4). 

## Acknowledgements
The overall tokenomics package is based on the [early-March proposal from Valdorff](../assets/rpip-49/readme.md). The initial drafts have seen a significant improvement as a result of discussions with many people (thanks to 🏆samus🏆, 🏆sckuzzle, 🏆epineph, knoshua, uisce, langers, NonFungibleYokem, MountainB, luominx, ArtDemocrat and many others). This proposal was presented at Rocket Pool's "Denver Lift Off" event by Valdorff and Samus (see [the presentation](https://docs.google.com/presentation/d/12WRXuZktEtViwBWxFwm8OHpwpgoOpAF01859o0jGkiw) or its [powerpoint backup](../assets/rpip-49/On%20The%20Horizon%20(backup%20version).pptx)).

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
