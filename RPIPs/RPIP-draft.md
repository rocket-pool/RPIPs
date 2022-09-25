---
rpip: 
title: Creation of Grants and Bounties Management Committee
description: Describes the guiding principles, selection of, operation of, and governance of the Grants and Bounties Management Committee.
author: Calurduran (@dafuerstman)
discussions-to: https://dao.rocketpool.net/t/grants-and-bounties-rpip/1064/7
status: Draft
type: Meta
created: 2022-09-18
---


## Abstract
RPIP-10 sets aside 30% of the pDAO budget for Grants and Bounties. The details of the mechanics of awarding grants and bounties were discussed in a forum post and in the Grants thread on the Discord Governance channel. Based on that previous work, I would like to suggest an outline for a Grants and Bounties Management Committee (hereafter GMC), including guiding principles and governance. Please see RPIP-10 for governance details that apply to all Management Committees.

## Motivation
The Rocket Pool community has always strongly contributed to developing a Rocket Pool ecosystem. From marketing outreach, to developing online tools, the community has self-organized and satisfied their own needs. The community has previously voted to allocate a portion of the pDAO budget to reward such past contributions and to incentive future ones in the form of grants and bounties. Having an established transparent framework for the awarding such funds will reinforce confidence in the protocol as well as further incentivize and reward community involvement. With this proposal, Rocket Pool will further develop its rich ecosystem: rewarding contributions and enhancing the Rocket Pool experience for every member.


## Specification
- The GMC’s chief mission SHALL be to distribute Grants and Bounties, retrospectively and prospectively, in order to harness the community’s talent to further the goals of the protocol.
- The GMC SHALL abide by the following definitions for grants, bounties, and retrospective awards:
    - Grant : A proposal submitted by an individual or group that proposes some action with an estimate of cost and payment schedule. Grants are a set amount broken up and paid over a set period. This might be X RPL paid over 13 inflation cycles (13x 28 days). This contract is awarded to the entity that submitted the proposal if the grant is approved.
    - Bounty : A proposal submitted by an individual or group that proposes a payment for an achieved result of a specific task/project. The bounty proposal should establish the desired outcome, state an award compensation amount with a payment schedule, and describe how interested parties can compete in the selection process for the bounty. The entity that submitted the proposal may or may not be the person(s) awarded the bounty contract.
    - Retrospective Award: A proposal submitted by an individual or group that proposes a payment for a previously-achieved result. The retrospective award should detail the work that was completed and the positive impacts for the protocol that would merit such an award. The entity that submitted the retrospective award proposal MAY or MAY NOT be the person(s) awarded a retrospective award for the work documented in the proposal.


## Operations
### Awards process
- The GMC SHALL publish an open call for grant, bounty, and retrospective award applications by the first of the month of the first month after the successful creation of the scoring rubric (see below), with deadlines for application falling on the 15th of that month. Subsequent calls for applications SHALL occur every other month thereafter, following the same 1st of the month/15th of the month deadline schema.
- The GMC SHALL score applications and announce proposed recipients by the final day of the month after the successful creation of the scoring rubric. Subsequent announcements of proposed recipients SHALL occur every other month thereafter, following the same final day of the month deadline schema.
- Prior to the first call for applications, the GMC SHALL develop one or more rubrics by which to select winning grants and bounties. The rubric(s) SHALL be publicly posted and the community SHALL be solicited in their development. The GMC MAY choose to develop separate rubrics for grants, bounties, and retrospective awards.
- Grants SHALL be awarded in one of six denominations:
    - Category A: 500 RPL over 13 inflation periods
    - Category B: 750 RPL over 13 inflation periods 
    - Category C: 1000 RPL over 13 inflation periods
    - Category D: 1500 RPL over 13 inflation periods
    - Category E: 2000 RPL over 13 inflation periods
    - Custom: A size to be determined through negotiation
- Bounties SHOULD be custom size in line with the expected value of a successful completion. Grants of a custom size SHOULD, on the other hand, be rare.
- The GMC MAY give retrospective awards for work completed prior to the first call for applications, or for work that was started prior to the first call for applications. Such applications MAY be submitted on behalf of others rather than being self-nominated. In any given award period, no more than 50% of the awarded RPL SHALL go to retroactive awards.
- The GMC SHALL announce its proposed winners prior to the final day of even-numbered months. Each award SHALL individually be subject to snapshot voting as soon as is practical thereafter.
- Funds for grants and bounties SHALL only be considered approved upon the successful passage of the snapshot vote.


### Assessment of Awards
- Recipients of Grants SHALL update the GMC in a publicly-available document about their progress on at least a monthly basis.
- If a majority of the GMC agrees that a grant recipient is failing to provide the specified services to the protocol in a timely manner (as documented in the original application and in subsequent monthly updates), the GMC SHALL put the revocation of the grant up for a snapshot vote.
- Any group or individual MAY submit a publicly-available document to the GMC claiming successful completion of the bounty. The GMC SHALL discuss all such applications. If a majority of the GMC agrees then the GMC SHALL put the successful closure of the bounty and the subsequent payment of the bounty up for a snapshot vote.

### Conflicts of Interest
- The GMC SHALL abide by the following processes to minimize conflicts of interest:
    - Any GMC member SHALL NOT score, vote on, or participate in GMC discussions about any retrospective award for which they are nominated. They may participate in the ratifying snapshot vote.
    - Any GMC member who submits a grant application SHALL abstain from scoring, voting on, or participating in GMC discussions about any grants during the application period for which they are an applicant. They may participate in the ratifying snapshot vote. They may also score, vote, and participate in discussions in future rounds during which their grant is ongoing, provided they have not submitted an application during that round for any new grants.


## Selection and Governance
In addition to the "Management Committee Governance" section of RPIP-10, the GMC will abide by the following:
- The GMC SHALL contain a minimum of seven individuals. The GMC SHOULD be composed of a mix of core team and community members but SHALL at any point in time have a majority of the membership be community members.
- Nominations
    - A nomination thread for the GMC SHALL be posted on the forum for at least 5 days.
    - Any member of the community MAY nominate any member (including themselves).
    - Any nominated member MAY ask to be removed from consideration.
    - A final list of nominees (except those that asked to be removed) SHALL be created and posted as a comment in the aforementioned thread.
    - If this results in insufficient nominees to fill the spots, the Management Committee Selection process fails and must be restarted from the beginning.
- Nominee information sharing
    - Within 3 days of the final list being posted, each nominee SHALL provide the following:
     - An alignment statement explaining why they are motivated to act according to the MC charter.
    - A conflict statement explaining any other entanglements that could be perceived as motivation to act against the MC charter or the protocol. This includes entanglements with other crypto, other LSD providers, etc.
    - If this is not provided, the nominee shall be removed from consideration. If this results in insufficient nominees to fill the spots, the GMC Selection process fails and must be restarted from the beginning.
- Within 3 days of the final list being posted, each nominee MAY provide the following:
    - An identity statement explaining as much or as little of who they are as they wish to share. This MAY provide verification to the degree the nominee desires.
    - A contribution statement explaining their contributions to RP.
- The nominee MAY also provide any additional information they deem helpful.
-   An organizer SHALL provide some basic hard metrics for all candidates; this SHOULD include:
    - Account ages of RP-related accounts.
    - Activity metrics for RP-related accounts (this MAY include items like: number of posts, how often their posts are liked, number of git commits, etc).
- Membership selection
- The information from “Nominee info sharing” SHALL be compiled and made available on a forum post.
- Once that post is available, a Snapshot “Weighted” vote SHALL be made.
- Voters MAY split their vote however they wish; it is RECOMMENDED that they evenly split their weight among their top 7 candidates, where 7 is the desired number of members on the committee.
- If quorum is not met, the GMC Selection process fails and MUST be restarted from the beginning.
- The selected membership SHALL be the nominees with the highest 7 vote weights, where 7 is the desired number of selections. In the event of a tie, the pDAO treasurer SHALL randomly select which of those nominees are selected.

## Rationale
The community has long desired a process by which some portion of pDAO funds could be used to reward past community member contributions and incentivize future such contributions. Discussion has been ongoing for at least the past six months on Discord and the forums about what such a grants and bounties award process might look like and who would comprise the selecting committee. At one point consideration was given to having the initial committee comprised of Rocket Scientists plus one team member. In the time since, the Incentives Management Committee has created a precedent for a procedure by which such a Management Committee’s membership might be selected. This RPIP follows that precedent.


## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
