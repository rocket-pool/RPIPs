---
rpip: 49
title: 2024 Tokenomics Rework Info
description: Provides an introduction and overview of the 2024 community tokenomics rework, its likely contents, and its current status.
author: Valdorff (@Valdorff), LongForWisdom (@LongForWisdom)
discussions-to: https://dao.rocketpool.net/tag/tokenomics-rework
status: Living
type: Informational
created: 2024-03-08
tags: tokenomics-2024
---

## Abstract
This informational RPIP provides an introduction and overview of the 2024 community tokenomics rework, its likely contents, and its current status. It exists to explain the overall tokenomics proposal as it's currently envisioned by contributing community members. This RPIP lists and briefly describes the rework's components, and links out to the RPIPs that specify those components in greater detail.

The tokenomics rework will likely be split into two protocol upgrades, Saturn 1 and Saturn 2. This RPIP describes the components that are expected to be included within each.

This Informational RPIP and the tokenomics rework represent the best efforts of the Rocket Pool community contributors involved. Information within this RPIP should not be considered official word from the Rocket Pool core development team. 

## Explainers

The following explainers are being maintained by the community to help make the rework as accessible as possible.

* [Part 1 - Why Rework Rocket Pool's Tokenomics?](/tokenomics-explainers/001-why-rework)
* [Part 2 - Introduction to the Tokenomics Rework](/tokenomics-explainers/002-rework-intro)
* [Part 3 - Foundation of the Tokenomics Rework](/tokenomics-explainers/003-rework-foundation)
* [Part 4 - Supporting Components of the Tokenomics Rework](/tokenomics-explainers/004-rework-support)
* [Part 5 - Glossary of Terms](/tokenomics-explainers/005-glossary)

## Contents

### [RPIP-43: Megapools](RPIP-43.md)

A Megapool is a single contract that can be used as an Ethereum withdrawal address for multiple validators.
* This allows for much more gas-efficient usage of the Rocket Pool protocol for Node Operators.
* This allows for the application of megapool-level penalties. 
* Importantly, this RPIP specifies that node operation no longer requires RPL staking (ETH-only node operation). 

Additionally, Megapools are required to facilitate the bond curve changes described in RPIP-42.

### [RPIP-42: Bond curves](RPIP-42.md)

The changes to bond curves allow Node Operators to provide smaller ETH bonds while maintaining the security of the Rocket Pool protocol. 
* This gives Node Operators the option of dramatically increasing their capital efficiency.
* This allows the Rocket Pool Protocol to support a greater amount of rETH. 

For bonds to be lowered securely for the protocol, an initial set of bonds for a node must be larger than the minimum bond. 

### [RPIP-46: Universal Adjustable Revenue Split](RPIP-46.md)

This change allows the ETH revenue income from borrowed ETH (aka, rETH commission) to be split between various targets. The four targets are:
1. The Node Operator responsible for the validator with the borrowed ETH.
2. Vote-eligible Node Operators, proportional to their share of vote-eligible RPL.
3. The surplus share, used to capture value to the RPL token.
4. The rETH share, going to rETH holders. 

Aspects of the split are managed by the pDAO, the security council, and automatically by a heuristic function. For full details of 'who can do what', please see the full RPIP. 

This RPIP also includes the reduction of RPL issuance from 5% to 1.5%, as RPL rewards to Node Operators have been replaced by UARS.

### RPL Value Capture

Some form of value capture method will be included in the tokenomics rework package. Three options are being actively debated:
* [RPL Burn](RPIP-45.md) - Use the surplus share to buy and burn RPL.
* [RPL Buy & LP](RPIP-50.md) - Use the surplus share to buy RPL and deposit it in a liquidity pool. 
* Direct the surplus share to vote-eligible Node Operators, proportional to their share of vote-eligible RPL.

For Saturn 1, the share will be directed based on vote-eligible RPL staked in megapools. There will be a vote prior to Saturn 1 going live to determine the preferred strategy starting at Saturn 2.

### [RPIP-44: Forced exits](RPIP-44.md)

This change, planned for Saturn 2, allows the Rocket Pool protocol to force-exit Node Operators under certain circumstances. It relies on the adoption of [EIP-7002](https://eips.ethereum.org/EIPS/eip-7002) by the Ethereum protocol. 
* This allows Node Operators to exit their validators easily.
* This allows the Rocket Pool protocol to exit malicious validators.

The ability to force-exit misbehaving validators is a requirement for [RPIP-44](RPIP-44.md).

### [RPIP-47: Forced delegate upgrades](RPIP-47.md)

This change allows the Rocket Pool protocol to force-upgrade Node Operators minipool delegate contracts after a grace period has expired. 
* This reduces the compatibility debt incurred by the protocol as it is upgraded because it does not need to support all prior iterations. 
* This means that protocol governance can make changes that benefit the protocol as a whole even if the changes do not benefit each individual Node Operator. 

### [RPIP-59: Deposit Mechanics](RPIP-59.md)

Here we describe the mechanics of Node Operator deposits and validator creation, including standard and express queues.
* This provides a faster queue to help small node operators get started.
* This provides a faster queue for existing node operators to migrate their minipools to megapools.
* This allows node operators to optionally exit the queue up until they point their validator is created.

### [RPIP-60: Protocol Upgrade Guardrails](RPIP-60.md)

This RPIP introduces a delay after protocol upgrades have been confirmed but prior to them coming into effect, and allows the security council to veto the upgrade during that delay. The veto is justified if the upgrade meets one of the following criteria:
* It was subject to vote manipulation
* It is a malicious action
* It causes clear damage to the Rocket Pool project

## Deployment Plan

The tokenomics rework package will likely be split between two protocol upgrades: Saturn 1 and Saturn 2.

### Saturn 1

* [RPIP-43: Megapools](RPIP-43.md)
  * Including ETH-only validators.
* [RPIP-42: Bond curves](RPIP-42.md)
  * Framework
  * 4ETH minimum bond
* [RPIP-59: Deposit Mechanics](RPIP-59.md)
  * Standard and express queues.
* [RPIP-46: Universal Adjustable Revenue Split](RPIP-46.md)
  * Splits between rETH, node operators, and vote-eligible staked RPL staked in megapools
  * RPL issuance rewards no longer have a minimum stake required
* RPL Value Capture - Increased share to voting Node Operators.
* [RPIP-47: Forced delegate upgrades](RPIP-47.md)
* [RPIP-60: Protocol Upgrade Guardrails](RPIP-60.md)

### Saturn 2

* [RPIP-42: Bond curves](RPIP-42.md)
  * 1.5ETH minimum bond. 
* [RPIP-44: Forced exits](RPIP-44.md)
* [RPIP-46: Universal Adjustable Revenue Split](RPIP-46.md)
  * Split may add a new share for all RPL, depending on outcome of Revenue Share vote
  * May add in a semi-automated system of controlling voter_share, depending on outcome of Revenue Share vote
  * RPL inflation reduced to 1.5%  (from 5%)
  * No more RPL issuance rewards
* RPL Value Capture based on vote outcome - [RPL Burn](RPIP-45.md) / [RPL Buy & LP](RPIP-50.md) / Increased share to vote-eligible RPL staked in megapools

## Current Status
Last Updated: July 16th

Current efforts are primarily focused on:
1. Confirming the Core Team is happy with RPIP specification contents and making minor adjustments as needed.
2. Polishing the RPIP specifications ready for vote.

A maintained list of open tasks is available via Google sheet [here](https://docs.google.com/spreadsheets/d/1KDTeFnNl3XLFO37upti6NbT2p2GDYJ4GKH4aJ51gQZA/edit?pli=1#gid=725857744). 

### Still To Ratify Prior to Saturn 1
These items are to be considered flexible until ratified explicitly, rather than ratified alongside the tokenomics rework package as a whole. 

* **Saturn 2 Surplus Share Strategy** - Decide and ratify how to manage surplus share in Saturn 2 (we want to decide this before Saturn 1 launches to avoid the incumbent advantage for the voter_share option.)
  * **Vote Eligible RPL Targets** - Decide and ratify vote-eligible RPL targets if voter_share is not chosen as the surplus share strategy. 
* **Deposit Strategy** - Decide whether to take a 2TX or 3TX deposit strategy for validator creation and ratify choice.
* **Penalty System** - Research, draft and ratify a penalty system. 
* **When the security council should change node operator commission** - the proposal delegates a limited ability for the security council to make UARS changes and directs them to do so based on a specific metric. When and how the council should act is still an open area of discussion.

Any other significant issues the development team identifies that impact their ability to deliver this upgrade may also be added to this list. The development team should strive to minimize this, communicate clearly if it occurs and keep as close to the spirit of the existing contents when deviation is needed. 

### Estimated Process

The below is generally agreed to be the steps to be completed before we can consider this 'decided'.

1. **Done** - Complete the first draft of the full proposal specifications. 
2. **Done** - Seek feedback from technically skilled or highly engaged community members on the draft specifications. 
3. **Done** - Create high-level explanations and informational material for the full proposal for consumption by the wider community.
4. **Done** - Make a concerted effort to gather feedback via the forum from the wider community.
5. **Active** - Update the proposal and specifications as needed taking into account wider community feedback.
6. Run a forum temperature check vote on the rework package (bar the 'still-to-ratify' list above).
7. Run a snapshot vote on the rework package as Living RPIPs, acknowledging the existence of the 'still-to-ratify' list above.
8. Run one or more snapshot vote(s) as blockers are cleared from the 'still-to-ratify' list. The end state will include no remaining blockers and the status of the rework RPIPs set to Final.

## Excluded Components
The below components have been discussed, but are not currently considered high enough priority to be included in the tokenomics rework plan.
* MEV penalty improvements
* rETH restitution from underperforming Node Operators
* Adjusting the DAO's portion of RPL inflation

## Further Links
* This proposal was presented at Rocket Pool's "Denver Lift Off" event by Valdorff and Samus - [Presentation](https://docs.google.com/presentation/d/12WRXuZktEtViwBWxFwm8OHpwpgoOpAF01859o0jGkiw), [Powerpoint Backup](../assets/rpip-49/On%20The%20Horizon%20(backup%20version).pptx), [Recorded Presentation](https://www.youtube.com/watch?v=nyqrilFtlrc&list=PLKzACASsJiuXc0v6kZambks4cPaSVbekf&index=4)
* A now-retired Google sheet containing notes, feedback, and TODOs which can be found [here](https://docs.google.com/spreadsheets/d/1qmGBCPAX-IqcFFjUzBib2Z_NKo_Yh5U00zKnGpyNak4). 
* A [tokenomics Q&A video](https://www.youtube.com/watch?v=p-Q6fQsVBTY), kindly hosted by the Rocket Fuel podcast.

## Acknowledgements
The tokenomics package is based on the [early-March proposal from Valdorff](../assets/rpip-49/readme.md). The initial drafts have seen a significant improvement as a result of discussions with many people (thanks to 🏆samus🏆, 🏆sckuzzle, 🏆epineph, 🏆LongForWisdom, 🏆knoshua, uisce, langers, NonFungibleYokem, MountainB, luominx, ArtDemocrat, and many others). 

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
