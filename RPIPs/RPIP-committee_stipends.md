---
rpip: #<to be assigned>
title: Committee Stipends
description: Defines stipends for members of pDAO committees. 
author: LongForWisdom (@LongForWisdom), Patrick_J
discussions-to: <URL>
status: Draft
type: Meta
created: 2024-03-04
requires: 10, 40
---

## Abstract

This RPIP defines stipends for members of pDAO committees. The total stipend budget for a given committee is calculated based on the target number of members for that committee, the expected hours for that committee and the global stipend rate, which is the same for all committees. Committees may split their stipend budget amongst their members in any manner they choose. 

Stipends to the members of each committee are paid by the Grants Management Committee.

## Motivation
The motivation section should describe the "why" of this RPIP. What problem does it solve? Why should someone want to implement this standard? What benefit does it provide to the Ethereum ecosystem? What use cases does this RPIP address?

## Specification

### Stipends
* A fixed monthly stipend budget SHALL be distributed to each pDAO committee as defined in RPIP-10.
* This stipend SHOULD be dispersed to voting committee members based on their contributions to the committee over the previous month. 
* The stipend is denominated in USD.

### Formula and Input Parameters
Three input parameters impact each committee's total stipend.

$TargetHours$ - Expected Hours. The amount of hours the pDAO hopes each member commits to committee work each month. This parameter may be different for each committee.  

$TargetCount$ - Target Member Count. The target number of members for the given committee. This parameter may be different for each committee.  

$TargetRate$ - Global Stipend Rate. The hourly rate for time committed to a committee. This parameter is shared across all committees.  

The total stipend to be split amongest the committee members each month is defined by the following formula.
```math
$$

Committee Monthly Stipend Budget = TargetHours * TargetCount * TargetRate

$$
```

#### Parameter Changes

* The Expected Hours input for a given committee MAY be adjusted via the internal vote of that committee. 
  * Increases to Expected Hours MUST have a unanimous positive vote outcome, which MUST be posted publicly.
  * Decreases to Expected Hours MUST have a majority positive vote outcome, which MUST be posted publicly. 
  * Any Expected Hours change is subject to pDAO challenge, and SHALL NOT be acted upon until 1 calendar month has passed after the decision has been announced.
* The Target Member Count SHALL be modified only by the pDAO via voted modification to this RPIP and SHOULD follow the usual processes for such a modification. 
* The Global Stipend Rate SHALL be modified only by the pDAO via voted modification to this RPIP and SHOULD follow the usual processes for such a modification. 

### Current and Historic Parameter Values

TODO fill in. 

### Payments

#### Split
The split of the total committee stipend SHOULD be determined by the voting members of the committee in question. 
* Each committee SHOULD be consistent in the method used to determine the payout split. 
* Roles that are compensated through other mechanisms SHOULD be discounted when the committee determines the stipend split.

#### Distribution
The Grants Management Committee is responsible for distributing the stipends for each committee directly to its members.
* Stipends SHOULD be paid out in one of the GMC's accepted payout token options, selected by the recipient.
  * Conversion from USD to dollar-stablecoin payout token SHOULD be priced 1:1. 
  * Conversion from USD to any other payout token SHOULD be done using the market price at the time of payment transaction creation. 
* Other committees MUST provide the GMC Administrator with the details of their stipend split each month. 
  * They MUST communicate payment addresses, stipend share in USD, and requested payment token for each of their members. 
  * They MUST meet any deadline or format requirements imposed by the GMC Administrator.
* Any undistributed funds for a given month are forfeit by the recieving committee.

## Rationale
The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work, e.g. how the feature is supported in other languages.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
