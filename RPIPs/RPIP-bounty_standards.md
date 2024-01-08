---
rpip: #<to be assigned>
title: Bounty Standards
description: Describes standards and incentives for the production of effective bounty proposals.
author: LongForWisdom (@LongForWisdom)
discussions-to: <URL>
status: Draft
type: Meta
created: 2024-01-04
requires: 26
---

## Abstract
Abstract is a multi-sentence (short paragraph) technical summary. This should be a very terse and human-readable version of the specification section. Someone should be able to read only the abstract to get the gist of what this specification does.

## Motivation
The motivation section should describe the "why" of this RPIP. What problem does it solve? Why should someone want to implement this standard? What benefit does it provide to the Ethereum ecosystem? What use cases does this RPIP address?

## Specification
The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

### Bounty Lifecycle

![Bounty Lifecycle](..\assets\rpip-bountystandards\bounty-lifecycle.png)

#### Statuses
**Draft - Proposed** - Bounty is described and proposed to the GMC for adoption. Bounty is not yet eligible for adoption.  
**Draft - Defined** - Bounty is well defined and ready for completion by a bounty hunter with the relevant skillsets if adopted by the GMC. Bounty is now eligible for adoption.  
**Draft - Rejected** - Bounty was rejected by the GMC at this time. Bounty is not eligible for completion.  
**Open** - Bounty is adopted by the GMC for completion and listed on relevant frontends. Bounty is now eligible for completion.  
**Closed - Completed** - A Bounty has been fully completed and paid out by the GMC. Bounty is no longer eligible for completion.  
**Closed - Expired** - Bounty has met its expiration date. Bounty is no longer eligible for completion.  
**Closed - Withdrawn** - GMC has withdrawn the bounty. Bounty is no longer eligible for completion.  

### Bounty Proposal Guidelines
* When a single proposal bounty proposal has parts that can only be completed by different groups, it should become multiple definitions. 
* Where possible, bounties should limit the number of distinct skillsets required for completion of the bounty.
* Bounties should be defined in terms of the smallest possible unit of work.
* Include any resources that might reasonably help the bounty hunter complete the bounty.
* Clearly define what tasks are required, and which are optional.
* Clearly list any dependencies, if the bounty cannot be completed in all circumstances.
* Include single sentence / short paragraph description.

### Bounty Incentive Levers
Bounty Incentives Levers are OPTIONAL incentives that the pDAO empowers the GMC to use to incentivise the generation and successful completion of bounty proposals.

#### Lever Incentives
* The incentive attached to each lever MAY be set by the GMC. 
* Levers MAY have an absolute (fixed amount) component and/or a relative (percentage of total bounty payout) component. 
* The payout of incentives for a given bounty MUST NOT reduce the bounty completion payout. 
* When incentive levers are modified by the GMC, incentives SHOULD be paid out in such a way that ambiguity favours incentive recipients.
* Lever incentives are not zero sum. The GMC SHOULD set lever incentives with caution when funds are limited. 

#### Payout
* Bounty incentives SHOULD be paid out of the GMC budget.
* Incentive payouts SHOULD take place within the GMC's regular payout schedule.
* Incentive payouts MUST be denominated in dollars, and MUST be paid out in one of the accepted payout token options selected by the recipient.

#### Lever A: Proposal Incentive
The proposal incentive lever aims to influence the number of bounty proposals that are recieved by the GMC.
* This incentive MUST be paid to the proposal author for a bounty proposal that is adopted by the GMC. 
* The incentive payout for this lever MAY be reduced at the GMC's discretion based on proposal quality and completeness.
* Bounty definitions do not need to be included for a proposer to recieve this incentive.

#### Lever B: Definition Incentive
The definition incentive lever aims to influence the quality and completeness of bounty definitions accompanying bounty proposals.
* This incentive MUST be paid to the definiton author for bounty definitions provided alongside a bounty proposal that is adopted by the GMC. 
* This incentive payout SHOULD be paid a maximum once per proposal, even if the bounty proposal is split into multiple definitions.
* Incentive payouts for this lever MAY be reduced at the GMC's discretion based on definition quality and completeness.
* This incentive must go to the proposal author unless an alternative is explicitly preapproved by the GMC adminitrator.

#### Lever C: Completion Incentive
The completion incentive lever aims to influence the quality and consistency of bounty work submitted by bounty hunters. It aims to do this by rewarding contacts that support, oversee and assist bounty hunters meet the requirements of open bounties. 
* This incentive SHOULD be split equally between bounty contacts that meet the following conditions:
  1. The bounty MUST be completed to the satisfaction of the GMC.
  2. The contact MUST be publicly listed in the relevant bounty definition.
  3. The contact MUST NOT be affiliated with the completing bounty hunter.
  4. The bounty hunter does not comment negatively on the contact when asked by the GMC.
* Contacts SHOULD NOT assist the bounty hunter directly with the bounty work.
* Contacts SHOULD assist bounty hunters via feedback, direction and oversight upon request.
* Incentive payouts for this lever MAY be reduced at the GMC's discretion for any reason. Reasoning must be publicly stated by the GMC.

### Guidelines and Templates

#### Bounty Proposal Template

#### Bounty Definition Template

### Initial Lever Settings

## Rationale
The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work, e.g. how the feature is supported in other languages.

## Backwards Compatibility
All RPIPs that introduce backwards incompatibilities must include a section describing these incompatibilities and their severity. The RPIP must explain how the author proposes to deal with these incompatibilities. RPIP submissions without a sufficient backwards compatibility treatise may be rejected outright.

## Security Considerations
All RPIPs must contain a section that discusses the security implications/considerations relevant to the proposed change. Include information that might be important for security discussions, surfaces risks and can be used throughout the life cycle of the proposal. E.g. include security-relevant design decisions, concerns, important discussions, implementation-specific guidance and pitfalls, an outline of threats and risks and how they are being addressed. RPIP submissions missing the "Security Considerations" section will be rejected. An RPIP cannot proceed to status "Final" without a Security Considerations discussion deemed sufficient by the reviewers.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
