---
rpip: 18
title: Updating of Grants Management Committee
description: Replaces RPIP-15, which described the guiding principles, selection of, operation of, and governance of the Grants and Bounties Management Committee.
author: Calurduran (@dafuerstman), Valdorff (@Valdorff)
discussions-to: https://dao.rocketpool.net/t/rpip-modifying-operations-of-gmc/1448
status: Obsolete
type: Meta
created: 2023-01-23
vote-to: https://vote.rocketpool.net/#/proposal/0x75ca9e35555150c713e332d585b9fdf7185c9cf4222ad705459b30cf6d6bb7fb
vote-date: 2023-02-12
vote-result: Passed
---

## \>\> NOTICE <<
This RPIP has been made obsolete by the passage of [RPIP-26](RPIP-26.md).

## Abstract
RPIP-10 sets aside 30% of the pDAO budget for Grants and Bounties. The details of the mechanics of awarding grants and bounties were discussed in a forum post and in the Grants thread on the Discord Governance channel. Based on that previous work, we would like to suggest an outline for a Grants and Bounties Management Committee (hereafter GMC), including guiding principles and governance. Please see RPIP-10 for governance details that apply to all Management Committees. This RPIP supercedes RPIP-15, with changes to the committee size (enlarged to 9), the frequency of award rounds (from every-other-month to quarterly), and the removal of the requirement that a majority of the committee be made up of non-team, non-oDAO members.

## Motivation
The Rocket Pool community has always strongly contributed to developing a Rocket Pool ecosystem. From marketing outreach, to developing online tools, the community has self-organized and satisfied their own needs. The community has previously voted to allocate a portion of the pDAO budget to reward such past contributions and to incentivize future ones in the form of grants and bounties. Having an established transparent framework for the awarding of such funds will reinforce confidence in the protocol as well as further incentivize and reward community involvement. With this proposal, Rocket Pool will further develop its rich ecosystem: rewarding contributions and enhancing the Rocket Pool experience for every member.

## Specification
- The GMC’s chief mission SHALL be to distribute Grants and Bounties, retrospectively and prospectively, in order to harness the community’s talent to further the goals of the protocol.
- The GMC SHALL abide by the following definitions for grants, bounties, and retrospective awards:
    - Grant : A proposal submitted by an individual or group that proposes some action with an estimate of cost and payment schedule. Grants SHALL be a set amount broken up and paid over a set period. This might be X RPL paid over 13 inflation cycles (13x 28 days). This contract is awarded to the entity that submitted the proposal if the grant is approved.
    - Bounty : A proposal submitted by an individual or group that proposes a payment for an achieved result of a specific task/project. The bounty proposal SHOULD establish the desired outcome, state an award compensation amount with a payment schedule, and describe how interested parties can compete in the selection process for the bounty. The entity that submitted the proposal MAY or MAY NOT be the person(s) awarded the bounty contract.
    - Retrospective Award: A proposal submitted by an individual or group that proposes a payment for a previously-achieved result. The retrospective award SHOULD detail the work that was completed and the positive impacts for the protocol that would merit such an award. The entity that submitted the retrospective award proposal MAY or MAY NOT be the person(s) awarded a retrospective award for the work documented in the proposal.

## Operations
### Awards process
- The GMC SHALL publish an open call for grant, bounty, and retrospective award applications by the first of the month of the first month after the successful creation of the scoring rubric (see below), with deadlines for application falling on the 15th of that month. Subsequent calls for applications SHALL occur at the start of every quarter - January, April, July, and October, following the same 1st of the month/15th of the month deadline schema.
- The GMC SHALL score applications and announce proposed recipients by the final day of the month after the successful creation of the scoring rubric. Subsequent announcements of proposed recipients SHALL occur every quarter - January, April, July, and October, following the same final day of the month deadline schema.
- Prior to the first call for applications, the GMC SHALL develop one or more rubrics by which to select winning grants and bounties. The rubric(s) SHALL be publicly posted and anyone so interested in participating SHALL be solicited in their development. The GMC MAY choose to develop separate rubrics for grants, bounties, and retrospective awards.
- The GMC MAY give retrospective awards for previously-completed work. Such applications MAY be submitted on behalf of others rather than being self-nominated. In any given award period, no more than 50% of the awarded RPL SHALL go to retroactive awards.
- Anyone MAY file an RPIP disputing a grant, bounty, or retrospective award within two weeks of the announcement of recipients. Such an RPIP SHALL be subject to a snapshot vote.
- Funds for grants and bounties SHALL only be considered approved upon either the completion of the two week waiting period without any objecting RPIPs, or the failure of any objecting RPIP snapshot votes.

### Assessment of Awards
- Recipients of Grants SHALL update the GMC in a publicly-available document about their progress on at least a monthly basis.
- If a majority of the GMC agrees that a grant recipient is failing to provide the specified services to the protocol in a timely manner (as documented in the original application and in subsequent monthly updates), the GMC SHALL publicly announce such a decision and cease any future payments. This decision MAY be disputed by anyone through the creation of an RPIP within two weeks of the GMC's announcement. The RPIP SHALL be subject to a snapshot vote.
- Any group or individual MAY submit a publicly-available document to the GMC claiming successful completion of the bounty. The GMC SHALL discuss all such applications. If a majority of the GMC agrees then the GMC SHALL announce the award of the bounty. Anyone MAY dispute the awarding of the bounty through the creation of an RPIP within two weeks of the GMC's announcement. The RPIP SHALL be subject to a snapshot vote.

### Conflicts of Interest
- The GMC SHALL abide by the following processes to minimize conflicts of interest:
    - Any GMC member SHALL NOT score, vote on, or participate in GMC discussions about any retrospective award for which they are nominated. They MAY participate in the ratifying snapshot vote.
    - Any GMC member who submits a grant application SHALL abstain from scoring, voting on, or participating in GMC discussions about any grants during the application period for which they are an applicant. They may participate in the ratifying snapshot vote. They may also score, vote, and participate in discussions in future rounds during which their grant is ongoing, provided they have not submitted an application during that round for any new grants.

## Selection and Governance
In addition to the "Management Committee Governance" and "Management Committee Selection" sections of RPIP-10, the GMC will abide by the following:
- The GMC SHALL contain nine individuals, except in cases of vacancies.
- Upon approval of this RPIP, immediate elections following the guidelines of RPIP-10 SHALL take place for any new or vacant seats. These elections SHALL supercede the rules for filling vacancies in the previous bullet.

## Rationale
The community has long desired a process by which some portion of pDAO funds could be used to reward past contributions and incentivize future such contributions. Discussion has been ongoing for at least the past six months on Discord and the forums about what such a grants and bounties award process might look like and who would comprise the selecting committee. At one point consideration was given to having the initial committee comprised of Rocket Scientists plus one team member. In the time since, the Incentives Management Committee has created a precedent for a procedure by which such a Management Committee’s membership might be selected. This RPIP follows that precedent.

## Membership

Current and historic membership is recorded in [RPIP-36](RPIP-36.md#grants-management-committee).

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).