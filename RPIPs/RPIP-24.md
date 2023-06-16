---
rpip: 24
title: oDAO Charter
description: Describes what the oDAO is for and how it will operate
author: Valdorff (@Valdorff)
discussions-to: https://dao.rocketpool.net/t/odao-charter-draft-and-more/1832
status: Draft
type: Meta
created: 2023-06-02
---

## Abstract
There is broad agreement that the oDAO needs to be more clearly defined in terms of what they are
expected to do, and how they are expected to do it. This document is intended to be a foundational
definition document that will serve (a) the oDAO itself in answering "what's the right thing to do"
in a given situation, and (b) the greater community in understanding "what should we expect" and
"what shouldn't we expect" from the oDAO and its membership.

## Specification

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
1. Each oDAO member SHALL openly communicate information about their signers
    1. This MUST include if there is one signer or multiple
    1. This MUST note any close relationships with other oDAO seats. This includes but is not limited to signers able to sign for more than one oDAO seat, and particularly close relationships to signers for other oDAO seats (familial, employee/employer, etc).
    1. This MAY include a high level bio introducing the seat and/or its signers to the greater community
1. The oDAO SHALL actively govern themselves
    1. The oDAO SHALL select its membership
    1. The oDAO SHALL hold their membership accountable to the values listed here
    1. The oDAO SHOULD hold their membership accountable for performing the responsibilities listed here
    1. The oDAO SHOULD establish internal guidelines to select and remove members
        1. These guidelines SHOULD include diversity to serve protocol safety
        1. These guidelines SHOULD highly value communication
    1. The pDAO MAY nominate a new member or suggest the kicking of an existing member via snapshot vote; if such a vote passes:
        1. The oDAO SHALL internally vote on the membership change, and execute the change if the members.quorum threshold is reached
        1. The oDAO SHALL post a statement with rationale for the decision

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
    1. Reward tree generation and submission (this responsibility REQUIRES the ability to generate
       the current and previous interval's trees)
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

### Updating this RPIP
This is a "Living" RPIP, so that it can be kept up to date with changes to duties.

However, the [Values](#odao-values) section SHALL not be updated. The RPIP as a whole MAY be
superseded with a 2/3 or greater supermajority oDAO vote.

The following process SHALL be followed for the [Responsibilities](#current-odao-responsibilities)
section of this RPIP:

1. An author SHALL make edits as desired and create a pull request; the status in the PR SHALL be
   "Draft"
2. An RPIP editor SHALL review it. If there are no structural issues, the status shall be changed
   to "Review"
3. If there is an associated vote (which MAY or MAY NOT be the case):
   1. The vote procedure SHALL be followed as if this were an entirely new RPIP. This includes
      making forum posts, appropriate wait times, quorums, etcetera, as detailed elsewhere.
   2. If criteria for a vote are not met, the status SHALL be changed to "No Vote Taken" and the PR
      SHALL be closed.
   3. If there was a vote, then after the vote is concluded:
      1. If the vote passed, the status SHALL be changed to "Living" and the PR SHALL be merged
      2. If the vote failed, the status SHALL be changed to "Vote Failed" and the PR SHALL be closed
4. If there was no associated vote (eg, a watchtower update changes duties)
   1. An RPIP editor shall review for accuracy with the necessary parties
   2. The status SHALL be changed to "Living" and the PR SHALL be merged

#### Changelog of oDAO responsibilities

| Date   | Changes |
|--------|---------|
| &nbsp; | &nbsp;  |

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
