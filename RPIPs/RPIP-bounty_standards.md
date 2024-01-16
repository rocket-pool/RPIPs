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

TODO Write

## Motivation

This RPIP is primarily motivated by the desire to see bounties become a consistent and reliable mechanism for the completion of work at the Rocket Pool DAO. The existing bounty system has not been heavily adopted in comparison to the grant and retro frameworks. This is unfortunate, and I believe it represents a missed opportunity. 

Bounties can serve as a much more balanced framework between service providers and the DAO. Grants tend to favour the service provider, and represent a greater risk for the DAO. Retros favour the DAO, and represent a greater risk to the service provider. Both of these have their place, but bounties provide a nice middle ground. The DAO is de-risked because it can approve work separately from provider, and providers are de-risked because the DAO has precommited to reward well-defined milestones.

The current bounty mechanism fall short in that it is missing a layer of work between the proposal of a bounty and the presentation of a bounty to bounty hunters. This means bounties are written to the audience of the GMC (to convince them to adopt the bounty proposal) rather than to the audience of the bounty hunter (that actually needs to deliver the work). This results in bounties that are badly specified from the perspective of the bounty hunter, this increases risk and will prevent individuals and groups from doing this bounty work. This RPIP describes this intervening layer of work and clearly deliniates the difference between bounty proposals (for the GMC to consider) and bounty definitions (for bounty hunters to work on.)

Another area in which the existing bounty mechanisms could be improved is that there is no explicit recognition that ideating, describing and defining an effective bounty can be a non-trivial amount of work, especially for bounties of larger values, or bounties that could have wide-reaching effects when delivered. This RPIP describes optional incentive levers that can be used by the GMC to reward different types of engagement with the bounty mechanisms. 

Finally, in a somewhat speculative inclusion this RPIP describes the concept of bounty contacts, individuals that can help bounty hunters understand and deliver work that meets the goals of Rocket Pool. The motivation here is to try to fill the gap in tribal knowledge that may exist between crypto service providers and the long-standing rocket pool community. Service providers may misunderstand elements of the protocol, may be unaware of information that impacts the bounty and may be unfamiliar with the processes around the delivery of bounties. Having a knowledgable contact may help avoid bad outcomes due to this lack of knowledge.


## Specification
The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119. TODO Remove

### Definitions

**Bounty Proposal**  

A high-level bounty idea that requires more work before it can be adopted by the GMC and made available to bounty hunters. The goal of the bounty proposal is to convince the GMC that this idea is worth the time and effort to adminster. Requirements for proposals are comparitively easy to meet, in order to encourage the community to submit their ideas. Bounty proposals are the 'quick and dirty' path for community members to suggest a bounty idea to the GMC.

**Bounty Definition**  

A detailed and well defined bounty specification that can be adopted by the GMC and made available to bounty hunters. The goal of the bounty definition is to ensure that the bounty deliverables are presented to bounty hunters in a way that results in accurate and effective output taking place under terms fully understood by both parties. Bounty definitions may be delivered by community members alongside their bounty proposal or may be created by the GMC. Bounty definitions are the 'tick all the boxes' path for community members to get their bounty adopted by the GMC.

**Bounty Hunter**  

A person or group that works to deliver the work specified by a well defined bounty.

**Bounty Contact**  

An individual that has expressed willingness to act as a point of contact with bounty hunters for a given bounty. This may be the bounty proposal author, a member of the GMC, or another Rocket Pool community member. The goal of the bounty contacts is to support and liase with bounty hunters so that they are better able to deliver the work specified in the bounty definition. Bounty contacts are listed in bounty definitions. 

### Bounty Lifecycle
![image](../assets/rpip-bounty_standards/bounty-lifecycle.png)

#### Statuses
**Draft - Proposed** - The bounty has been described and proposed to the GMC for adoption. The bounty is not yet eligible for adoption.  
**Draft - Defined** - The bounty has been well defined and made ready for completion by a bounty hunter with the relevant skillsets if adopted by the GMC. The bounty is now eligible for adoption.  
**Draft - Rejected** - The bounty has been rejected by the GMC at this time. The bounty is not eligible for completion.  
**Open** - The bounty has been adopted by the GMC for completion and listed on relevant frontends. The bounty is now eligible for completion.  
**Closed - Completed** - The bounty has been fully completed and paid out by the GMC. The bounty is no longer eligible for completion.  
**Closed - Expired** - The bounty has met its expiration date. The bounty is no longer eligible for completion.  
**Closed - Withdrawn** - The bounty has been withdrawn by the GMC. The bounty is no longer eligible for completion.  

### Bounty Incentive Levers
Bounty Incentive Levers are OPTIONAL incentives that the pDAO empowers the GMC to use to incentivise the generation and successful completion of bounty proposals.

#### Lever Incentives
* The incentive attached to each lever MAY be set by the GMC. 
* Levers MAY have an absolute (fixed amount) component and/or a relative (percentage of total bounty payout) component. 
* The payout of incentives for a given bounty MUST NOT reduce the bounty hunters award payout. 
* When incentive levers are modified by the GMC, incentives SHOULD be paid out in such a way that ambiguity favours incentive recipients.
* The total cost of lever incentives is not fixed. The GMC SHOULD set lever incentives with caution when funds are limited. 

#### Payout
* Bounty incentives SHOULD be paid out of the GMC budget.
* Incentive payouts SHOULD take place within the GMC's regular payout schedule.
* Incentive payouts MUST be denominated in dollars, and MUST be paid out in one of the GMC's accepted payout token options selected by the recipient.

#### Lever A: Proposal Incentive
The proposal incentive lever aims to influence the number of bounty proposals that are recieved by the GMC.
* This incentive SHOULD be paid to the proposal author for a bounty proposal that is adopted by the GMC. 
* Individual incentive payouts for this lever MAY be reduced at the GMC's discretion based on proposal quality and completeness.
* Bounty definitions do not need to be included for a proposer to recieve this incentive.

#### Lever B: Definition Incentive
The definition incentive lever aims to influence the quality and completeness of bounty definitions accompanying bounty proposals.
* This incentive SHOULD be paid to the definiton author for bounty definitions provided alongside a bounty proposal that is adopted by the GMC. 
* This incentive payout SHOULD be paid a maximum once per proposal, even if the bounty proposal is split into multiple definitions.
* Individual incentive payouts for this lever MAY be reduced at the GMC's discretion based on definition quality and completeness.
* This incentive SHOULD go to the proposal author unless an alternative is explicitly preapproved by the GMC adminitrator.

#### Lever C: Support Incentive
The support incentive lever aims to influence the quality and consistency of bounty work submitted by bounty hunters. It aims to do this by rewarding contacts that support, oversee and assist bounty hunters meet the requirements of open bounties. 
* This incentive SHOULD be split equally between bounty contacts that meet the following conditions:
  1. The bounty MUST be completed to the satisfaction of the GMC.
  2. The contact MUST be publicly listed in the bounty definition of the completed bounty.
  3. The contact MUST NOT be affiliated with the completing bounty hunter.
  4. The bounty hunter does not comment negatively on the contact when asked by the GMC.
* Contacts SHOULD NOT assist the bounty hunter directly with the bounty work.
* Contacts SHOULD assist bounty hunters via feedback, direction and oversight upon request.
* Individual incentive payouts for this lever MAY be reduced at the GMC's discretion for any reason. Reasoning must be publicly stated by the GMC.

#### Initial Lever Recommendations
These are non-binding recommendations to the GMC for initial incentive lever settings. They are included as a starting point for discussions, and are not intended to limit GMC decision-making on the issue.
* For the first few months, levers SHOULD be set to higher levels, with the goal of reaching a meaningful number of open bounties.
* Lever A and B SHOULD NOT be zero unless unallocated GMC funds are low.
* Lever B SHOULD be higher than Lever A, in order to incentivize well defined bounty generation.
* Lever B SHOULD have a relative component, as higher-value bounties should be expected to have longer, more strict, and less ambiguous definitions.
* Lever C SHOULD be a focus for experimentation. It is difficult to know how useful or effective it will be without empirical testing.
* Relative values SHOULD be less than 10% of the total bounty value under normal circumstances.

### Templates and Guidelines

Initial guidelines and templates have been provided to the GMC where not already present. The GMC will maintain these and ensure they are accessible to the community. 

Links to the relevant documents are provided here for reference. These documents are not considered to be ratified by the pDAO, and can be modified by the GMC as required.

* [Bounty Proposal Template + Guidelines](https://github.com/rocketpoolgmc/rocketpool-gmc/blob/main/meta/bounty-proposal-template.md)
* [Bounty Definition Template + Guidelines](https://github.com/rocketpoolgmc/rocketpool-gmc/blob/main/meta/bounty-definition-template.md)

## Rationale

### Proposals and Definitions
Goals here were:
* Split the work into something quick and reasonably simple for any community member to do quickly (the proposal) from the complicated part (clearly defining work, bounty structure, etc) in order to lower the barrier to entry while maintaining a baseline for quality. 
* Split the work by audience: the proposal (primarily GMC) and the definition (primarily bounty hunter.)
* Get a semi-consistent definition format that can feed into the bounty portal I've been developing.

### Lifecycle and Statuses
This is mostly just a comprehension aid, but may also help the GMC clearly communicate to the DAO what's going on with various bounties.

### Incentives
Goals here were:
* Provide high optionality to the GMC. This feels like a bit of a cop out, but it is preferable than requiring additional votes in the future to tweak lever values, especially when as-yet we don't have any empirical data on how useful these levers each are.
* Make incentives modular so that different parts of the process can be incentivized separately. 
* The Support incentive for the contact is expected and intended to be at least partially a proxy for the author because the author determines who is listed as contacts on definitions they've authored. Authors that can see bounties through end-to-end as proposer, definer and contact are pretty valuable to the protocol, and it makes sense that they can collect all three incentives. The specification as-is allows optionality here as well though. Some authors may not want to fulfil this role and they can opt out easily. 
* It's hard to know if relative or absolute incentives will work best. Makes sense to allow both + testing.
* Initial lever recommendations are included so that I'm not pushing all of the hard incentive decisions onto the GMC. They represent my best guess as to what would benefit the protocol. None of these should be too huge, but they need to be large enough to affect behavior. Initially higher levers makes sense to kick-start the bounty mechanism because there is an element of self-reinforcement on both the proposal and completion sides that only works once it gets started. 
* The requirement that contacts not be bad-mouthed by the bounty hunter is just to prevent contacts from failing to provide support when asked. 

## Backwards Compatibility
Existing bounties have been given rough definitions already. Upon ratification of this RPIP, these rough definitions will be firmed up and their contents approved by the GMC.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
