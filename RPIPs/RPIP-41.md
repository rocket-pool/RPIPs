---
rpip: 41
title: Committee Stipends
description: Defines stipends for members of pDAO committees.
author: LongForWisdom (@LongForWisdom), Patrick_J
discussions-to: https://dao.rocketpool.net/t/rpip-41-committee-stipends/2824
status: Final
type: Meta
created: 2024-03-04
requires: 10, 40
vote-to: https://vote.rocketpool.net/#/proposal/0xe89ab0a846846884b088ba4ec5e37fb4ecb1a3a5b9639c53ee480055570c53d2
vote-date: 2024-05-10
vote-result: Passed
---

## Abstract

This RPIP defines stipends for members of pDAO committees. The total stipend budget for a given committee is calculated based on the number of members for that committee, the target hours per month for that committee, and the global stipend rate which is the same for all committees. Committees may split their stipend budget amongst their members in any manner they choose.

Stipends to the members of each committee are paid by the Grants Management Committee from the GMC's budget.

This RPIP does not fund committee members retroactively.

## Motivation
This RPIP is primarily motivated by a desire to see the efforts being made by members of pDAO committees to be rewarded monetarily for the time they are committing to Rocket Pool as members of those committees. A secondary yet still important motivation is to help ensure that the pDAO continues to attract sufficient nominees for these committee membership roles in the future.

Despite this motivation, there is also a desire to maintain a volunteer ethos among committee members. Compensation of any kind does threaten this ethos, however, it is hoped that by having financial compensation be closer to 'symbolic' than 'market rate' the pDAO can preserve this ethos.

## Specification

### Stipends
* A monthly stipend budget SHALL be allocated to each pDAO committee as defined in RPIP-10.
* This stipend SHOULD be dispersed to committee members based on their contributions to the committee over the previous month.
* The stipend is denominated in USD.

### Formula and Input Parameters
Three input parameters affect each committee's total stipend budget.

$MemberCount$ - Member Count. The number of members in the given committee. This input SHOULD reflect the current reality, rather than the ideal number of members.  

$TargetHours$ - Target Hours. The amount of hours the pDAO hopes each member commits to committee work each month. This parameter MAY be different for each committee.  

$TargetRate$ - Global Stipend Rate. The hourly rate for time committed to a committee. This parameter is shared across all committees.  

The committee stipend budget to be split amongst the committee members each month is defined by the following formula.
```math
$$

Committee Monthly Stipend Budget = MemberCount \times TargetHours \times TargetRate

$$
```

#### Parameter Changes

* The Target Hours input for a given committee MAY be adjusted via the internal vote of that committee.
  * Increases to Target Hours MUST have a unanimous positive vote outcome, which MUST be posted publicly.
  * Decreases to Target Hours MUST have a majority positive vote outcome, which MUST be posted publicly.
  * Any Target Hours change is subject to pDAO challenge, and SHALL NOT be acted upon until 1 calendar month has passed after the decision has been announced.
* Roles and responsibilities financially compensated through other mechanisms SHOULD be discounted when considering the Target Hours parameter.
* The Target Member Count SHALL be modified only by the pDAO via a voted modification to this RPIP and SHOULD follow the usual processes for such a modification.
* The Global Stipend Rate SHALL be modified only by the pDAO via a voted modification to this RPIP and SHOULD follow the usual processes for such a modification.

### Current and Historic Parameter Values

Future committees SHOULD be added to this RPIP and members SHOULD expect to receive a stipend for their hours at the same Global Stipend Rate. This SHOULD be done without a separate pDAO vote to modify this RPIP.


| Date                       | Global Stipend Rate |
|----------------------------|---------------------|
| 2024-03-04 (RPIP Authored) | 30 USD / hr         |

#### Grants Management Committee

| Date                       | Target Member Count | Target Hours        | Target Monthly Stipend Budget |
|----------------------------|---------------------|---------------------|------------------------------|
| 2024-03-04 (RPIP Authored) |                   9 |                  13 |                    $3,510.00 |

#### Incentives Management Committee

| Date                       | Target Member Count | Target Hours        | Target Monthly Stipend Budget |
|----------------------------|---------------------|---------------------|------------------------------|
| 2024-03-04 (RPIP Authored) |                   9 |                  13 |                    $3,510.00 |


<!--
Commentary for Editors:
The above tables may be generated using this [google sheet](https://docs.google.com/spreadsheets/d/1aXE-bbVK5IB54TfqRPq9_kyMKMDDDiVDuw3yA3f5XJg/edit#gid=958081945), which should be maintained in parallel. If you need to modify the sheet and do not have permission then:
1. Copy the sheet
2. Set it to public read-only
3. Replace this link with your updated sheet
4. Convert the new sheet to markdown using: https://www.tablesgenerator.com/markdown_tables or similar.
-->

### Payments

#### Split
The split of the committee stipend budget SHOULD be determined by the voting members of the committee in question.
* Each committee SHOULD be consistent in the method used to determine the payout split.
* Roles and responsibilities financially compensated through other mechanisms SHOULD be discounted when the committee determines the stipend split.

#### Distribution
The Grants Management Committee is responsible for distributing the stipends for each committee directly to its members. 
* Stipends SHALL be paid from the GMC's budget.
* The GMC Administrator SHOULD determine the period for payouts and communicate this to each committee.
  * By default this is assumed to be monthly, but MAY be increased to ease administrative burden if needed.
* Stipends SHOULD be paid out in one of the GMC's accepted payout token options, selected by the recipient.
  * Conversion from USD to dollar-stablecoin payout token SHOULD be priced 1:1.
  * Conversion from USD to any other payout token SHOULD be done using the market price at the time of payment transaction creation.
* Other committees MUST provide the GMC Administrator with the details of their stipend split for each period.
  * They MUST communicate payment addresses, stipend share in USD, and requested payment token for each of their members.
  * They MUST meet any deadline or format requirements imposed by the GMC Administrator.
* Any undistributed funds for a given period are forfeited by that committee.

## Rationale

**Distribution Period**  
Monthly is assumed to be the default as it's been central to the discussions in the past. A quarterly distribution may be preferable as this would reduce administrative overhead. This is left up to the GMC Administrator because the burden largely falls on them. 

**Global Stipend Rate versus Community Specific**  
I chose a global rate due to the desire to not chase market rates and to maintain the volunteer ethos. The stipend is a reward for committing time to rocket pool in an 'official' capacity. On this basis, it shouldn't matter how committee members spend that time.

**Target Hours Changes**  
This input may change regularly as the DAO evolves, and we want to avoid a pDAO vote each time. The committee members are best positioned to set this input, so they can change it via vote. The unanimous threshold is in place to protect committee members from an increase in expectations that they do not agree to.

**pDAO Challenge**  
Strictly, this does not exist as a thing outside of the GMC. But, the process resolves problems of this type effectively, and it deserves to be more widely adopted. It is not defined in this RPIP and should be understood to work in the same way as with the GMC.

**Committee Managed Split**  
I have some misgivings over leaving this freedom to each committee because it will mean inconsistent methods across different committees. But, I'm unconvinced that the pDAO mandating a specific method is preferable. The freedom to pick something that works is important, especially at this stage.

**GMC Administration**  
The GMC will administer the distribution of stipends and fund them out of the GMC budget because there are no better-suited sub-structures within the DAO. If such a structure is added, it should take over this role from the GMC.

**RPIP Editors and pDAO Treasurer**  
Both of these positions are intentionally not included in this RPIP because, at the time of writing, they do not fit into the committee framework. The author believes strongly that these positions should be elected and receive the same stipend rate. The author believes moderately strongly that both of these roles should move under the committee framework in the future to solve this problem.  

**Retroactive vs Forward Looking**
This RPIP is forward looking only to reduce initial vote complexity and ongoing 'RPIP legislative debt'. The GMC has existing mechanisms that previous committee members can use to seek retroactive funding if they feel the need to do so. 

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
