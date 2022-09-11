---
rpip: 
title: pDAO Guardian Multisig Charter
description: Describes what the pDAO Guardian Multisig is for and how it will execute
author: Valdorff (@Valdorff)
discussions-to: https://dao.rocketpool.net/t/pdao-guardian-multisig-charter/1025/3
status: Draft
type: Meta
created: 2022-09-11
---

## Abstract
The pDAO guardian multisig (PGM) is a group intended to use the guardian role to (A) safeguard the
integrity of the protocol and (B) carry out the will of the overall community.

## Motivation
The pDAO guardain has powerful abilities to change settings and spend from the treasury. It also has
a unique ability to serve as a check on the oDAO by limiting the maximium allowed penalty. The pDAO
guardian can only be transferred to a new holder by the current holder.

Given the above, it is critical that the pDAO guardian be held as safely as possible. It should be
resilient to bad actors, inactivity, and accidents. It is the author's belief that this is best
achieved by allowing the overall community make informed decisions about whom they would trust to
hold the pDAO guardian.

## Specification

### Guiding Principles
- The primary role of the pDAO guardian multisig (PGM) SHALL be to safeguard the long-term integrity
  of the protocol. This includes:
  - The PGM membership SHALL routinely check that all members are able to sign and responsive; if
    this is not the case, they SHALL publicly say so and ask the pDAO for a new membership vote
  - In votes with vote manipulation, malicious action, or proposals that would result in clear
    damage to the Rocket Pool project, if the results of a vote would need PGM action to execute,
    the PGM SHALL use its veto power per RPIP-4. The PGM MUST NOT use its veto power in other
    situations.
  - In case of a critical exploit, the PGM SHALL take immediate action to lock the relevant portions
    of the protocol (eg, disable user deposits). The PGM MUST NOT unilaterally change a setting in
    any case other than a critical exploit. The PGM SHALL immediately write a report on the exploit,
    the action taken, and a recommended path for the pDAO to vote on.
  - In case of a rogue oDAO, the PGM SHALL do what it can to limit damage (eg, by using
    `setMaxPenaltyRate` to prevent theft of capital from NOs)
  - Insofar as possible, the PGM SHOULD support limiting dangerous pDAO guardian powers to the
    minimum needed to carry out the above. Because limits on dangerous powers reduce the damage a
    future rogue guardian could do, the PGM SHOULD support limiting such powers unless there is an
    explicit reason not to.
- The secondary role of the PGM SHALL be to faithfully execute the will of the pDAO as shown via
  snapshot votes. This explicitly includes:
  - Votes that limit or remove pDAO guardianship powers
  - Votes that change PGM membership
  - Votes that PGM members disagree with, or even have some security concerns about
  - Votes that require repeated actions, such as desired budget disbursements

### PGM Membership Selection
This procedure is based on the MC selection process from [RPIP-10](RPIP-10.md). Because RPIP-10 is a
"Living" RPIP that may be updated, the desired text is copied over here in full (with modifications
to make it about the PGM). Note that this procedure should be followed both for initial membership
selection, and to replace all or part of the membership after a vote to do so.

- Nominations
  - A nomination thread for the PGM SHALL be posted on the forum for at least 5 days.
    - Any member of the community MAY nominate any member (including themselves).
    - Any nominated member MAY ask to be removed from consideration.
    - A final list of nominees (except those that asked to be removed) SHALL be created and posted
      as a comment in this thread.
    - If this results in insufficient nominees to fill the spots, the PGM Membership Selection
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
      consideration. If this results in insufficient nominees to fill the spots, the PGM Membership
      Selection process fails and must be restarted from the beginning.
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
    where N is the desired number of members in the PGM.
  - If quorum is not met, the PGM Membership Selection process fails and MUST be restarted
    from the beginning.
  - The selected membership SHALL be the nominees with the highest N vote weights, where N is the
    desired number of selections. In the event of a tie, the pDAO treasurer SHALL randomly select
    which of those nominees are selected.

### Governance
- PGM members SHALL structure transactions and execute them from a multisig.
  - The multisig MUST allow for successfully signing with at least 4 members missing
  - The multisig SHALL require at least one more signature than a majority in order to sign
  - The above means 6 of 10 is the absolute minimum size for the PGM
- The PGM SHOULD be able to respond to an emergency (eg, needing to stop deposits) within 3 hours
  - PGM members SHALL each be available to at least 3 other PGM members via something that alerts
    (eg, a cell phone number)
- The PGM SHALL ensure that every member has signed within the last 3 months. If they have not, the
  pDAO SHALL be informed so that they can move to replace the member that cannot sign. A transaction
  MAY be created specifically for the purpose of showing that members can sign.
- The pDAO MAY vote to change the membership of the PGM at any time (in part or as a whole)
- The initial PGM SHALL be a 7 of 12 multisig, with membership selected per PGM Membership
  Selection. The nomination thread MAY be started while this RPIP is still being voted on.

## Rationale
The pDAO guardian wields significant powers, which are useful as a safety net for various edge
cases (eg, setting the max penalty OR immediately stopping deposits). Additionally, it has some
powers that provide convenience (eg, changing the size of the deposit pool without requiring a
contract change with audits). Note that as we move forward, some of these powers (eg, the ability
to require more than 100% oDAO consensus; eg, pDAO budget disbursements) may be removed, moved to
other groups, or automated -- this is absolutely ok, and in the long run, we may even be able to
remove the pDAO guardian entirely.

Due to the breadth of the guardian's current powers, it's critical to place the guardian in safe and
robust hands, as well as to provide a clear code of conduct for the PGM membership to follow. This
is what this charter attempts to frame and move us towards.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
