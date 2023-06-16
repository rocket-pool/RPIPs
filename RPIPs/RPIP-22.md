---
rpip: 22
title: Initial starting point "oDAO charter draft and more"
description: An initial draft version of 3 documents as shared by the oDAO
author: Valdorff (@Valdorff)
discussions-to: https://dao.rocketpool.net/t/odao-charter-draft-and-more/1832/2
status: Final
type: Informational
created: 2023-05-26
---

## Abstract
This document is meant to simply record the initial draft state as drafted by the oDAO and shared
on 2023-05-23.

The only changes from the document presented to the greater community are the header, this abstract,
no table of contents, and formatting with markdown.

## Intro - you should read this
Hello greater RP community!

We’ve been working hard on an oDAO charter. While we were doing this, we found it pretty necessary to think about the definition of the pDAO and inflation.

The following is essentially 3 documents:
- oDAO charter
  - Drafting is fairly far along, but we don’t want to formalize or finalize without community feedback
  - Once formalized and finalized into an RPIP, it’s intended to be voted on by oDAO node signatures
- pDAO charter
  - This is a rough skeleton for the community to flesh out into a fuller draft
  - Once formalized and finalized into an RPIP, it’s intended to be voted on via snapshot
- Inflation changes
  - This is a rough skeleton for the community to flesh out into a fuller draft
  - Once formalized and finalized into an RPIP (or maybe an RPIP plus an edit to RPIP-10), it’s intended to be voted on via snapshot

Why did the oDAO draft something for the pDAO side?
- We wanted to provide guidance about what would work well for the oDAO and team, to avoid potential pain points and resentment
- We wanted to provide a concrete starting point to a conversation – it’s very hard to discuss things that are totally abstract, which has been an issue in previous oDAO discussions
- There were interactions with the oDAO side. Inflation feeds into oDAO incentives which are related to duties. Duties have to be split up between the oDAO and pDAO, so their definitions significantly impact each other.
That said: those two documents are (purposely) rough skeletons. The community should take it from here, discuss (among themselves, with the team, and with the oDAO), make it more complete, and generally improve upon it before formalizing and finalizing.

The intent is to have an interlinked vote, such that support for each document can be clearly shown/seen, and the documents are only in force if all 3 votes pass.

##  oDAO Charter
### oDAO Values
1. The oDAO SHALL serve the RP community at large, which comprises rETH holders, NOs, and RPL holders, by upholding the values and responsibilities outlined in this charter
1. The oDAO SHALL NOT actively govern the protocol; they SHALL act as caretakers serving the RP community at large
    1. While the oDAO possesses the ability to veto (by refusing to apply an upgrade or settings change) veto powers SHALL be reserved for cases of changes that were not voted for by the pDAO, vote manipulation, malicious action, or proposals that would result in clear damage to the Rocket Pool project
        1. When a veto power is exercised, the oDAO SHALL publish a Veto Explanation Document that describes why this step was taken. If possible, the report SHOULD also suggest potential similar-but-non-damaging votes that could be considered.
    1. Except for the narrow veto circumstances described above, the oDAO SHALL execute the proposals that are ratified by a pDAO vote
1. The oDAO SHALL prioritize protocol safety
1. The oDAO SHALL minimize its role
    1. The oDAO SHOULD gradually and responsibly remove responsibilities where it is technically possible to do them in trustworthy and more decentralized means
    1. When no responsibilities remain, the oDAO SHALL support removal of the oDAO
1. The oDAO SHALL take on additional duties if required by the protocol
    1. Whenever possible, the oDAO SHOULD advocate for implementing duties without reliance on the oDAO and/or with an explicit plan to remove reliance on the oDAO
1. In addition to their explicitly defined duties, oDAO members SHOULD act as stewards for the protocol
1. The oDAO SHALL NOT abuse their position for personal gain
1. The oDAO SHALL actively govern themselves
    1. The oDAO SHALL select its membership
    1. The oDAO SHALL hold their membership accountable to the values listed here, especially as perceived by the RP community at large
    1. The oDAO SHOULD hold their membership accountable for performing the responsibilities listed here
    1. The oDAO SHOULD establish internal guidelines to select and remove members
    1. The oDAO SHOULD be transparent to the community about their self-governance outcomes
1. …

### Current oDAO Responsibilities
1. oDAO members MUST be informed voters on protocol update proposals
1. The oDAO SHOULD actively communicate with the RP community at large
    1. It is RECOMMENDED that the oDAO explain their votes, including the reasoning and due diligence behind them
    1. It is RECOMMENDED that the oDAO have forums where they can discuss with the RP community at large (examples could be live town hall meetings, quarterly Q&A forum threads, etc)
    1. The oDAO MAY or MAY NOT be available for day-to-day discussions (eg, on discord)
    1. Communication MAY be done at member or group level
1. oDAO members SHOULD effectively fulfill oracle duties, including:
    1. Balance updates
    1. RPL price updates
    1. Reward tree generation and submission
1. oDAO members SHOULD effectively fulfill support duties, including:
    1. Scrub checks for new minipools
    1. Scrub checks for bond reductions
    1. Scrub checks for solo migrations
    1. Marking validators as withdrawable (once the technical solution is provided)
1. oDAO members MAY propose settings changes for settings controlled by the oDAO, including:
    1. Proposal settings
    1. Scrub check settings
    1. oDAO membership settings
1. oDAO members SHOULD be informed voters on settings changes for settings controlled by the oDAO
1. oDAO members SHALL apply penalties within the framework provided by the pDAO
1. oDAO members SHALL perform the maintenance required to perform the aforementioned duties effectively; members SHOULD NOT be penalized for reasonable downtime incurred for maintenance
1. …

#### Changelog of oDAO responsibilities

| Date   | Changes |
|--------|---------|
| &nbsp; | &nbsp;  |

## pDAO Charter
### pDAO Values
1. The pDAO SHALL serve the RP community at large; this comprises rETH holders, NOs, and RPL holders
1. The pDAO SHALL be defined as Node Operators, with power based on effectively staked RPL
1. The pDAO SHALL actively govern the protocol in the interest of the RP community at large
1. pDAO members SHOULD either
    1. Be informed and engage in public discourse about governance, or;
    1. Delegate their voting power to someone that is informed and engaged
1. The pDAO SHALL prioritize protocol safety
1. The pDAO SHOULD prioritize decentralization
1. The pDAO SHOULD prioritize permissionlessness
1. …

## Inflation Changes
### High Level Context
1. Payments to the oDAO:
    1. Must cover ongoing expenses and initial expenses
    1. Should help keep members eager to serve in their role
    1. Should mitigate the chance of bribery
1. Currently, inflation is split up 70% to NOs, 15% to pDAO, 15% to oDAO
1. The Arbitrum security council was the most apt comparison based on powers and TVL in the protocol
    1. They are paid $60k/yr per seat (USD denominated)
    1. The oDAO have duties beyond protocol updates
    1. The oDAO take on price risk
    1. Other researched groups included the security multisigs for Aave, Balancer, Hop, Stakewise and Aura; as well as the oracle networks for Maker, Stakewise, and Lido.
1. We’re targeting
    1. The same rough ballpark as the Arbitrum security council, but a bit higher
    1. Potentially decrease pay over time (for simplicity) or as duties are removed
    1. When considering different options, money soon should be valued more than money later
        1. Partly due to the time value of money
        1. Partly because we’re actively working to eliminate duties/pay
1. In the suggestions below, there is math shown assuming a constant 18-seats oDAO. The pDAO SHOULD consider increasing inflation (and/or skipping otherwise planned reduction) in response to positive diversification of the oDAO.

### Suggestion 1 - reduce to 5% immediately, ramp to 2.5% over time
1. The oDAO share of inflation SHALL be set to 5%
1. For the next 20 reward periods, every 4 rewards periods 0.5% of inflation SHALL be removed from the oDAO share
    1. To be explicit, the oDAO share SHALL be: 5% for 4 periods, 4.5% for 4 periods, 4% for 4 periods, 3.5% for 4 periods, 3% for 4 periods, set to 2.5%
1. When the oDAO no longer has any duties, oDAO share of inflation SHALL be set to 0%
    1. Crucially, this includes contract upgrades; this may depend on ossifying a version of RP
1. …

Assuming constant $50 per RPL and 18 members:\
1st year rewards per member (13 periods): $118.2k\
2nd year rewards per member: $80.9k\
3rd year rewards per member: $73.8k

### Suggestion 2 - reduce to 8% immediately, ramp to 2% over time
1. The oDAO share of inflation SHALL be set to 8%
1. For the next 12 reward periods, every 2 rewards periods 1% of inflation SHALL be removed from the oDAO share
    1. To be explicit, the oDAO share SHALL be: 8% for 2 periods, 7% for 2 periods, 6% for 2 periods, 5% for 2 periods, 4% for 2 periods, 3% for 2 periods, set to 2%
1. When the oDAO no longer has any duties, oDAO share of inflation SHALL be set to 0%
    1. Crucially, this includes contract upgrades; this may depend on ossifying a version of RP
1. …

Assuming constant $50 per RPL and 18 members:\
1st year rewards per member (13 periods): $139.3k\
2nd year rewards per member: $56.2k\
3rd year rewards per member: $59.0k


### Protocol Development Funding
Dev team funding would be untenable if only coming from oDAO seats after these changes.\
As a result, we RECOMMEND an update to allocations. This MAY or MAY NOT look something like the following (some standalone, some updates to RPIP-10):
1. Increase the pDAO share of inflation
1. Decrease the IMC and GMC allocation percentages to counteract the increase in inflation
1. Increase the inflation share to the Reserve Treasury to the remainder
1. Remove “The pDAO SHALL NOT directly spend funds”, or make an explicit exception for the next bullet
1. Fund protocol development using long-term grants
    1. Maybe a year?
    1. Based on a rough roadmap, but with room to act independently in the best interest of the protocol
    1. Voted on directly by the pDAO instead of having a management committee as an intermediary

For context, some things that were purposely avoided:
1. Enshrining dev team funding. The team thought it was important that they could be replaced if the pDAO saw fit (I believe “if they were all just drinking piña coladas in Hawaii” came up); enshrining the team via direct inflation or something on that level is thus not desirable.
1. Damaging team flexibility. The team should be able to be agile and respond to current priorities as they see fit. An example of a problematic system would be using bounties for all dev; this would create pressure for them to hit specific milestones, even at the expense of quality or priorities that have since risen in importance.
1. Creating a “boss” sub-committee. The team are all doxed individuals working in web3 with a lot of reputation based on RP. If another group directs them (eg, the GMC came up in brainstorms), that group would have tremendous power, despite likely lower alignment.

Hopefully that helps explain how we came to “sparse high level pDAO direction” as a good method.
