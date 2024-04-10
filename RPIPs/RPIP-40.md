---
rpip: 40
title: Updating of Grants Management Committee 2
description: Replaces RPIP-26, which described the guiding principles, selection of, operation of, and governance of the Grants and Bounties Management Committee.
author: ShfRyn (@ShfRyn), Calurduran (@dafuerstman), Valdorff (@Valdorff)
discussions-to: https://dao.rocketpool.net/t/gmc-updates-proposal/2750
status: Final
type: Meta
created: 2024-02-20
requires: 18, 26
vote-to: https://vote.rocketpool.net/#/proposal/0xe53f98bba8797bfa6ddaf4fd20809c53fabca4aaa9b00d09557e6f270634b820
vote-date: 2024-03-28
vote-result: Passed
---

## Abstract

RPIP-10 sets aside 30% of the pDAO budget for Grants and Bounties. The details of the mechanics of
awarding grants and bounties were discussed in a forum post and in the Grants thread on the Discord
Governance channel. Based on that previous work, we would like to suggest an outline for a Grants
and Bounties Management Committee (hereafter GMC), including guiding principles and governance.
Please see RPIP-10 for governance details that apply to all Management Committees.

The GMC was created in RPIP-15.\
RPIP-18 superceded RPIP-15, with changes to the committee size (enlarged to 9), the frequency of
award rounds (from every-other-month to quarterly), and the removal of the requirement that a
majority of the committee be made up of non-team, non-oDAO members.\
RPIP-26 superceded RPIP-18, it removes the 50% retrospective award cap, adds a GMC Administrator,
and reworks the awards process to a rolling process instead of a quarterly one.\
This RPIP supercedes RPIP-26, it revises the conflict of interest section to allow GMC members to participate in grants that have not been identified as competing grants, it adds additional clarity on GMC members participating in bounties, it makes an adjustment to the application deadline within the schedule, it restores the 'Selection and Governance' section that was incorrectly removed in RPIP-26.

## Motivation

The Rocket Pool community has always strongly contributed to developing a Rocket Pool ecosystem.
From marketing outreach, to developing online tools, the community has self-organized and satisfied
their own needs. The community has previously voted to allocate a portion of the pDAO budget to
reward such past contributions and to incentivize future ones in the form of grants and bounties.
Having an established transparent framework for the awarding of such funds will reinforce confidence
in the protocol as well as further incentivize and reward community involvement. With this proposal,
Rocket Pool will further develop its rich ecosystem: rewarding contributions and enhancing the
Rocket Pool experience for every member.

RPIP-26 added:

- The 50% retrospective award cap has inadvertently created a bottleneck in the GMC's ability to
  distribute funds efficiently. The majority of submissions fall into the retrospective award
  category, causing a significant backlog. Removing the cap would allow the GMC to address the
  backlog and honor the contributions of community members who have made significant contributions
  to the protocol.
- The rigidity of the current awards schedule limits the opportunity for adjustments and modifications, leading to decisions that are often too final. Consequently, there seems to be an imbalance favoring retrospective awards over grants. The implementation of an ongoing awards period, allows for more flexibility and adaptability in the decision-making process. By adopting a rolling awards system, the GMC can achieve enhanced efficiency and provide greater value to the recipients.
- Given the identified weaknesses within the GMC and the increased demand necessitated by rolling awards, there is a need for an expanded role of the GMC Administrator. This role would involve overseeing and facilitating various GMC operations, specifically focusing on the new rolling awards process.

The motivation for the update in this RPIP is that:
- The existing conflict of interest regulations prove overly restrictive, hindering the deliberation process within the GMC. Periodically, the GMC faces emergencies necessitating resource reallocation, potentially compromising the effectiveness of decision-making. Permitting committee members who submit grants to also participate in other grant decisions, with the provision for any member to declare a conflict of interest, offers a pathway to restoring much-needed efficiency without imposing burdensome regulations.
- As the GMC has increased its facilitation of bounties, it has recognized the necessity for greater clarity regarding the bounty process. This clarity aims to mitigate potential conflicts of interest among GMC members who are interested in participating in bounty completion.
- The current schedule causes the discussion period to fluctuate between 9 and 16 days. This update allows a consistent 16-day discussion period, which leads to more time to research, negotiate, and discuss each award.
- RPIP-26 superseded RPIP-18 but incorrectly left out the 'Selection and Governance' section. This update restores that missing section.

## Specification

- The GMC’s chief mission SHALL be to distribute Grants and Bounties, retrospectively and
  prospectively, in order to harness the community’s talent to further the goals of the protocol.
- The GMC SHALL abide by the following definitions for grants, bounties, and retrospective awards:
  - Grant : A proposal submitted by an individual or group that proposes some action with an
    estimate of cost and payment schedule. Grants SHALL be a set amount broken up and paid over a
    set period. This might be X RPL paid over 12 months. This contract is
    awarded to the entity that submitted the proposal if the grant is approved.
  - Bounty : A proposal submitted by an individual or group that proposes a payment for an
    achieved result of a specific task/project. The bounty proposal SHOULD establish the desired
    outcome, state an award compensation amount with a payment schedule, and describe how
    interested parties can compete in the selection process for the bounty. The entity that
    submitted the proposal MAY or MAY NOT be the person(s) awarded the bounty contract.
  - Retrospective Award: A proposal submitted by an individual or group that proposes a payment
    for a previously-achieved result. The retrospective award SHOULD detail the work that was
    completed and the positive impacts for the protocol that would merit such an award. The entity
    that submitted the retrospective award proposal MAY or MAY NOT be the person(s) awarded a
    retrospective award for the work documented in the proposal.

### Operations

#### Awards process

- Community members can submit grant, bounty, and retrospective award applications at any time
  - Submissions received before 7th of the month will be reviewed that month; if received after that, they will be queued for next month
- The GMC will meet on or around the 7th of the month to make preliminary decisions and provide comments
- The GMC Administrator will contact the submitter and:
  - Share preliminary decision and rationale
  - Work with submitter on edits, if desired
  - Assign individual commitee members for verification, if work is specialized
- The GMC will meet on or around the last Sunday of the month to make final approval/rejection decisions and provide comments
- Submitters are free to resubmit applications in later months if desired
- Prior to the first call for applications, the GMC SHALL develop one or more rubrics by which to
  select winning grants and bounties. The rubric(s) SHALL be publicly posted and anyone so
  interested in participating SHALL be solicited in their development. The GMC MAY choose to develop
  separate rubrics for grants, bounties, and retrospective awards.
- The GMC MAY give retrospective awards for previously-completed work. Such applications MAY be
  submitted on behalf of others rather than being self-nominated.
- Anyone MAY file an RPIP disputing a grant, bounty, or retrospective award within two weeks of the
  announcement of recipients. Such an RPIP SHALL be subject to a snapshot vote.
- Funds for grants and bounties SHALL only be considered approved upon either the completion of the
  two-week waiting period without any objecting RPIPs, or the failure of any objecting RPIP snapshot
  votes.

#### Assessment of Awards

- Recipients of Grants SHALL update the GMC in a publicly-available document about their progress on
  at least a monthly basis.
- If a majority of the GMC agrees that a grant recipient is failing to provide the specified
  services to the protocol in a timely manner (as documented in the original application and in
  subsequent monthly updates), the GMC SHALL publicly announce such a decision and cease any future
  payments. This decision MAY be disputed by anyone through the creation of an RPIP within two weeks
  of the GMC's announcement. The RPIP SHALL be subject to a snapshot vote.
- Any group or individual MAY submit a publicly-available document to the GMC claiming successful
  completion of the bounty. The GMC SHALL discuss all such applications. If a majority of the GMC
  agrees then the GMC SHALL announce the award of the bounty. Anyone MAY dispute the awarding of the
  bounty through the creation of an RPIP within two weeks of the GMC's announcement. The RPIP SHALL
  be subject to a snapshot vote.

#### Conflicts of Interest

- The GMC SHALL abide by the following processes to minimize conflicts of interest:
  - Any GMC member SHALL NOT score, vote on, or participate in GMC discussions about any
    retrospective award for which they are nominated. They MAY participate in the ratifying
    snapshot vote.
    - Any GMC member who submits a grant application SHALL abstain from scoring, voting on, or participating in GMC discussions about any competing grants during the application period for which they are an applicant. ANY committee member or the GMC Administrator may identify a grant as a competing grant. In an environment where funds are judged to be limited, the GMC Administrator must identify all grants, retros and bounties as competing. If a grant is identified as competing, committee members who have submitted a grant application for that period cannot vote or discuss that application.  They may participate in the ratifying snapshot vote. They may also score, vote, and participate in discussions in future rounds during which their grant is ongoing, provided they have not submitted an application during that round for any new grants.
  - Any GMC member who submits a bounty SHALL not complete that bounty. If a GMC member plans to complete a bounty, that SHALL be communicated to the GMC as early in the  process as possible.

#### Expanded GMC Administrator Role

The expanded GMC Administrator role's responsibilities will include: meeting coordinator, awards facilitator, spokesperson, grants and bounties liaison, treasurer, and governance author.

The administrator will follow an internal handbook that they manage for their processes. It can be reviewed and revised by the GMC at any time.

##### Hiring Process

###### Job Posting

- The GMC will publish an open job posting for a part-time GMC Administrator position with a 1-year term. The posting will include the position's title, responsibilities, qualifications, expected time commitment, and compensation.
- The job posting will be accessible to the public, allowing interested candidates to submit their applications within a designated 14-day window.

##### Application Review and Interview

- The Grants and Management Committee (GMC) will review the received applications once the application window closes.
- The GMC will conduct interviews with shortlisted candidates to assess their suitability for the role.
  - The interviews may include a combination of virtual or in-person meetings, skill assessments, and reference checks.

##### Decision-Making

- After the interviews and evaluations are complete, the GMC will deliberate and make a decision regarding the candidate selection. The decision will be made through a single choice vote, where each member of the GMC selects their preferred candidate from the pool of applicants. The candidate who receives more than 50% of the votes cast by the GMC members will be selected. In the event that nobody meets the threshold (no candidate receives more than 50% of the votes), or if multiple candidates receive an equal number of votes, The GMC has 7 days to engage in further discussions to reevaluate the candidates, review their qualifications, and reconsider their merits. Within 7 days they will hold another single choice vote to choose their preferred candiate. This 7 day discussion process repeats itself until a candidate is successfully chosen.

- The decision will be based on the candidate's qualifications, experience, alignment with the organization's values, and their ability to fulfill the position's responsibilities effectively.
- The decision will come within 14 days of the application window closing.

##### Transition and Renewal of Administrator's Term:

To ensure a smooth transition and maintain operational continuity, the committee will assess the administrator's term one month prior to its end. At that point, the committee will have two options:

- Term Extension: The committee may choose to extend the current administrator's term by one year, provided they have been performing effectively and meeting the organization's expectations. This allows for continuity in their role and provides stability within the position.
- Hiring Process Initiation: Alternatively, the committee may decide to initiate the hiring process outlined below, seeking potential applicants for the position. This proactive approach ensures a seamless transition if a new candidate is selected.

##### Contingency Plan for GMC Administrator Removal and Replacement

###### Consensus Vote for Removal

- Any committee member can formally request a vote to remove the current GMC Administrator by bringing the matter to the attention of the committee.
  - The request will initiate a consensus vote among the committee members, where they will collectively decide on the removal. A majority vote of greater than 50% is required for removal.

###### Committee Member Replacement

- If the GMC Administrator role is vacant at any time, the GMC SHALL, within at most 3 days, select a current GMC member to act as interim administrator during the hiring process. This selection is based on a vote, with the highest vote receiver being selected. In the event of a tie, the pDAO treasurer will cast the tie breaking vote. The backup will receive compensation equivalent to the administrator's compensation during the time the backup serves.
- The backup committee member will ensure the smooth functioning of the administrator's duties until a replacement is hired.

###### Replacement Hiring Process

- Upon the initiation of the replacement hiring process, the GMC will promptly begin the recruitment procedure outlined in the standard hiring process mentioned above.
- The job posting for the administrator position will be updated to indicate the urgency of the situation and the need for an emergency replacement.

## Selection and Governance
In addition to the "Management Committee Governance" and "Management Committee Selection" sections of RPIP-10, the GMC will abide by the following:

- The GMC SHALL contain nine individuals, except in cases of vacancies.

## Membership

Current and historic membership is recorded in [RPIP-36](RPIP-36.md#grants-management-committee).

## Rationale

From RPIP-18:

The community has long desired a process by which some portion of pDAO funds could be used to reward
past contributions and incentivize future such contributions. Discussion has been ongoing for at
least the past six months on Discord and the forums about what such a grants and bounties award
process might look like and who would comprise the selecting committee. At one point consideration
was given to having the initial committee comprised of Rocket Scientists plus one team member. In
the time since, the Incentives Management Committee has created a precedent for a procedure by which
such a Management Committee’s membership might be selected. This RPIP follows that precedent.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
