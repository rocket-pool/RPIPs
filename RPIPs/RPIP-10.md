---
rpip: 10
title: pDAO Budget Allocation
description: Describes the planned usage of pDAO budget moving forward
author: jasperthefriendlyghost, Valdorff (@Valdorff)
discussions-to: https://dao.rocketpool.net/t/pdao-liquidity-committee-and-budget-proposal/895
status: Living
type: Meta
created: 2022-08-09
---


## Abstract
This RPIP provides a split for incoming funds, describes what will be done with existing funds,
describes how spending will be tracked and describes how current reserves and future income may
be reapportioned by the pDAO.

## Motivation
Now that snapshot voting is available to allow "the RP community" to express their will, it is
important to use that will to direct the pDAO's funds as we see fit. These funds can be used for
many purposes, which can change over time, so this RPIP will be a Living document that can reflect
changes to how the funds should be directed.

## Specification
- Incoming funds SHALL be tracked and apportioned per category with the following split:
  - Incentives (e.g., LP bonuses) - 50%
  - Grants and Bounties - 30%
  - Reserve Treasury - 20%
- The pDAO SHALL NOT directly spend funds.
  - Funds SHALL be spent by a Management Committee (hereafter MC)
- Where a matching MC exists (eg, the "Incentives Management Committee" for the "Incentives"
  category), incoming funds SHALL be disbursed to the matching MC as quickly as practicable.
  - When possible, automation SHOULD be used to facilitate and expedite disbursements.
- Where no matching MC exists, funds SHALL stay in the same accounting category indefinitely.
  - Upon creation of a matching MC, these funds SHALL be disbursed to the newly created MC as
  quickly as practicable.  
- The pDAO MAY vote to change the split and/or to move existing funds; this includes retrieving
  funds from an MC, or sending funds to an MC.
- MCs MAY accept funds from sources other than the pDAO.
  - If explicitly requested by the source providing the funds, these funds SHALL NOT be removed by
  the pDAO. In such a case, the MC's Treasurer SHALL track those funds separately
  from pDAO-sourced funds to enable this.
  - If not explicitly stated, the pDAO MAY move these funds. In such a case, the MC's treasurer MAY
  track these funds together with pDAO-sources funds.
- There SHALL NOT be a "Reserve Treasury Management Committee"; this means those funds cannot be
  accessed without a vote to reallocate them to a new category with an MC.
- There SHALL be a pDAO Treasurer appointed via pDAO vote within 3 months; until such a pDAO
  Treasurer is appointed, jasperthefriendlyghost will serve as interim pDAO Treasurer.
  - The pDAO treasurer SHOULD post an update within one week of the end of each rewards period,
  which details the following for each category: starting funds, income, spend, final funds
  - The pDAO MAY vote to change the pDAO Treasurer at any time
- Current reserves SHALL NOT be used except to pay for the pre-existing Bankless agreement until
  Bankless is fully paid (or an arrangement has been made with them to ensure that we can meet our
  obligations regardless of market fluctuations)
  - Once Bankless is fully paid (or an arrangement has been made with them to ensure that we
  can meet our obligations regardless of market fluctuations), all remaining reserves SHALL be split
  in accordance to the currently prevailing split for incoming funds

### Management Committee Governance
The following apply to all MCs, except where their charter explicitly deviates from this Management
Committee Governance section.
- MC members SHALL structure transactions and execute them from a multisig.
  - The multisig MUST allow for successfully signing with at least 2 members missing. The MC MAY
    allow for more missing members when signing.
  - The multisig SHALL require at least a majority in order to sign. The MC MAY require a larger
    majority for signatures.
  - The above means 3 of 5 is the absolute minimum requirement possible for a multisig.
  - Existing MC members MAY vote to remove a member with a 2/3 or greater supermajority.
    - Note that this _cannot_ be done if it would violate the above multisig requirements.
  - The MC SHALL have one or more stated goals; where possible, they SHOULD be measurable enough for
  the community to judge how well the goals are being met.
- The pDAO MAY vote to change the membership of the MC at any time.
- There SHOULD be a pDAO review of MC membership within 6 months of the MC's creation.

#### Management Committee Treasurer
- The MC SHALL appoint one of their number as MC treasurer.
  - The MC MAY change the MC treasurer by a simple majority vote.
- The MC Treasurer SHALL publish reports of how funds were used within one week of the end of each
  rewards period.
  - The report SHALL include: initial funds available, new income received, funds deployed
    (including locations, amounts, and justifications for each platform), final funds available,
    and a list of the active MC members.
  - The MC SHALL receive feedback following those reports.
  - The MC SHOULD respond to this feedback and respect the will of the broader pDAO.

### Management Committee Selection
This process SHALL be followed when committees are newly formed, as well as when all or part of a
committee's membership is to be replaced (eg, due to term limits or a vote).

- Nominations
  - A nomination thread for the MC SHALL be posted on the forum for at least 5 days.
    - Any member of the community MAY nominate any member (including themselves).
    - Any nominated member MAY ask to be removed from consideration.
    - A final list of nominees (except those that asked to be removed) SHALL be created and posted
      as a comment in this thread.
    - If this results in insufficient nominees to fill the spots, the Management Committee Selection
      process fails and must be restarted from the beginning.
- Nominee information sharing
  - Within 3 days of the final list being posted, each nominee SHALL provide the following:
    - An alignment statement explaining why they are motivated to act according to the MC charter.
    - A conflict statement explaining any other entanglements that could be perceived as motivation
      to act against the MC charter or the protocol. This includes entanglements with other crypto,
      other LSD providers, etc.
    - An identity statement explaining as much or as little of who they are as they wish to share.
      This MAY provide verification to the degree the nominee desires.
    - A contribution statement explaining their contributions to RP.
    - If any of the above four statements is not provided, the nominee shall be removed from
      consideration. If this results in insufficient nominees to fill the spots, the Management
      Committee Selection process fails and must be restarted from the beginning.
  - The nominee MAY also provide any additional information they deem helpful.
  - An organizer SHALL provide some basic hard metrics for all candidates; this SHOULD include:
    - Account ages of RP-related accounts.
    - Activity metrics for RP-related accounts (this MAY include items like: number of posts, how
      often their posts are liked, number of git commits, etc).
- Membership selection
  - The information from "Nominee info sharing" SHALL be compiled and made available on a forum post.
  - Once that post is available, a Snapshot “Weighted” vote SHALL be made.
  - Voters MAY split their vote however they wish. If the voter has no specific split they wish to
    follow, it is RECOMMENDED that they evenly split their weight among their top N candidates,
    where N is the desired number of members on the committee.
  - If quorum is not met, the Management Committee Selection process fails and MUST be restarted
    from the beginning.
  - The selected membership SHALL be the nominees with the highest N vote weights, where N is the
    desired number of selections. In the event of a tie, the pDAO treasurer SHALL randomly select
    which of those nominees are selected.

### Updating this RPIP
This is a "Living" RPIP, so that it can be kept up to date with changes to the pDAO budget or budget
procedures. There SHALL NOT be any merged updates (beyond correcting errata and add non-normative
clarifications) unless they are accepted by pDAO vote. The following process SHALL be followed to
update this RPIP:

1. An author SHALL make edits as desired and create a pull request; the status in the PR SHALL be
   "Draft"
2. An RPIP editor SHALL review it. If there are no structural issues, the status shall be changed
   to "Review" 
3. The vote procedure SHALL be followed as if this were an entirely new RPIP. This includes making
   forum posts, appropriate wait times, quorums, etcetera, as detailed elsewhere.
4. If criteria for a vote are not met, the status SHALL be changed to "No Vote Taken" and the PR
   SHALL be closed.
5. If there was a vote, then after the vote is concluded:
   1. If the vote passed, the status SHALL be changed to "Living" and the PR SHALL be merged
   2. If the vote failed, the status SHALL be changed to "Vote Failed" and the PR SHALL be closed 

## Rationale
The main goal is to establish a starting budget and provide for a quick initial state where funds
can be allocated and spent.

The major alteration from the original budget discussion is the removal of the Marketing allocation
from the pDAO budget. This was suggested as the core team has recently hired a Marketing team member
from their funds and the Bankless marketing blitz is likely sufficient coverage for the time being.

The community believes that it's important to start spending, particularly on liquidity incentives
as quickly as possible. The liquidity bucket amounts to 67.5k RPL, or 1,087 ETH at market prices,
annually. For a theoretical run of 1 reward period (28 days), this gives  ~83.6 ETH or ~140,200 USD
to work with. In light of parallel discussions about tokenomics changes, this amount serves as a
conservative starting point.

In addition to https://dao.rocketpool.net/t/pdao-liquidity-committee-and-budget-proposal/895, see 
https://dao.rocketpool.net/t/pdao-budget-definition/644 for more context.

## Historical budget splits

| Date         | Split                                                                                            |
|--------------|--------------------------------------------------------------------------------------------------|
| 2022-08-09   | Incentives (e.g., LP bonuses) - 50%, Grants and Bounties - 30%, Reserve Treasury - 20%           |

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
