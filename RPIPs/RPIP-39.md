---
rpip: 39
title: Bounty Incentives
description: Proposes a set of optional incentive levers that can be used by the GMC to encourage bounty proposals.
author: LongForWisdom (@LongForWisdom)
discussions-to: https://dao.rocketpool.net/t/rpip-39-bounty-incentives/2753
status: Draft
type: Meta
created: 2024-02-16
requires: 26
---

## Abstract

This RPIP proposes a set of optional incentive levers that can be used by the GMC to encourage bounty proposals. There are three proposed 'incentive levers', which are settings to incentivize different parts of the bounty process. One incentivizes bounty proposals, one incentivizes bounty definitions, and the last incentivizes an informal support role for individual bounties. Values for these incentive levers are not mandated by this RPIP. Instead, these are left up to the GMC to manage on an ongoing basis, and they would be paid out of the GMC's budget. The pDAO may challenge lever value changes or incentive payouts via the existing GMC challenge process.


## Motivation
This RPIP is motivated by the desire to see bounties become a consistent and reliable mechanism for the completion of work at the Rocket Pool DAO. The existing bounty system has not been heavily adopted in comparison to the grant and retro frameworks. This is unfortunate, and it represents a missed opportunity.

Bounties can serve as a balanced framework between service providers and the DAO. Grants tend to favor the service provider and represent a greater risk for the DAO. Retros favor the DAO and represent a greater risk to the service provider. Both of these have their place, but bounties provide a nice middle ground. The DAO is de-risked because it can approve work separately from a provider. Providers are de-risked because the DAO has committed to rewarding well-defined milestones.

Currently, there is little recognition that creating an effective and actionable bounty is a non-zero amount of work. Secondly, there is a natural gap in tribal knowledge between members of the Rocket Pool community and crypto service providers. This RPIP describes optional incentive levers that may be used by the GMC to encourage the community to both propose bounties and to support well-meaning service providers that lack some of that deep knowledge of the protocol and DAO.

## Specification

### Definitions

These definitions are descriptive and not prescriptive. The GMC maintains the authority to modify or change these structures or roles in the future.  

**Bounty Proposal**  
A high-level bounty idea that requires more work before it can be adopted by the GMC. The goal of the bounty proposal is to convince the GMC that this idea is worth the effort and funding to pursue. Proposal requirements are easy to meet to encourage community engagement. Bounty proposals are the 'quick and dirty' path for community members to suggest a bounty idea to the GMC.

**Bounty Definition**  
A detailed and well-defined bounty specification that can be adopted by the GMC. The goal of the bounty definition is to communicate the content and terms of the bounty to bounty hunters. Community members may provide a bounty definition alongside their bounty proposal, it may be created by the GMC, or submitted by a separate community member. Bounty definitions are the 'tick all the boxes' path for community members to get their bounty adopted by the GMC.

**Bounty Support Contact**  
An individual who acts as a supporter and point of contact with bounty hunters for a given bounty. This may be the bounty proposal author, a member of the GMC, or another community member. The bounty support contact's goal is to support and liaise with bounty hunters so that they are better able to deliver the work specified in the bounty definition. Bounty contacts are listed in bounty definitions.

### Bounty Incentive Levers
Bounty Incentive Levers are OPTIONAL incentives that the pDAO empowers the GMC to use to incentivize the generation and successful completion of bounty proposals.
* The incentive attached to each lever MAY be set by the GMC.
* Levers MAY have minimum and maximum values, which MAY be defined in absolute dollar amounts and/or as a percentage of the bounty hunter award payout.
* Incentives for a given bounty MUST NOT reduce the bounty hunter award payout.
* The total cost of lever incentives is not limited. The GMC SHOULD set lever incentives with caution.
* When incentive lever values are modified by the GMC:
  * Consideration SHOULD be given to the expectations of incentive recipients. 
  * Decreases in lever values SHOULD come into effect at the start of the next reward period. 

#### Payout
* Bounty incentives MUST be paid out of the GMC budget.
* Incentive payouts SHOULD take place within the GMC's regular payout schedule.
* Incentive payouts MUST be denominated in dollars and SHOULD be paid out in one of the GMC's accepted payout token options, selected by the recipient.

#### pDAO Recourse
The pDAO MAY use the existing committee challenge system to challenge GMC incentive decisions. This may take place either:
* When a lever incentive setting is changed by the GMC.
* When an incentive is announced to be paid out to a given proposer, definer, or support contact.

### Lever A: Proposal Incentive
The proposal incentive lever aims to influence the number of bounty proposals that are received by the GMC.
* This incentive SHOULD be paid to the proposal author for a bounty proposal that is adopted by the GMC.
* Individual incentive payouts for this lever MAY be reduced at the GMC's discretion based on proposal quality and completeness.
* Bounty definitions do not need to be included for a proposer to receive this incentive.

### Lever B: Definition Incentive
The definition incentive lever aims to influence the quality and completeness of the bounty definitions that accompany bounty proposals.
* This incentive SHOULD be paid to the definition author for bounty definitions that are adopted by the GMC.
* This incentive payout SHOULD be paid a maximum of once per bounty proposal, even if the bounty proposal is split into multiple bounty definitions.
* Individual incentive payouts for this lever MAY be reduced at the GMC's discretion based on definition quality and completeness.

### Lever C: Support Incentive
The support incentive lever aims to influence the quality and consistency of the bounty work submitted by bounty hunters. It aims to do this by rewarding community contacts that can support, oversee, and assist bounty hunters meet the requirements of open bounties.
* This incentive SHOULD be split equally between bounty support contacts listed on a bounty where the following conditions are met:
  1. The bounty MUST be completed (in full or in part) to the satisfaction of the GMC.
  2. The contact MUST be publicly listed in the bounty definition of the completed bounty.
  3. The contact MUST NOT be affiliated* with the completing bounty hunter.
  4. The bounty hunter SHOULD NOT comment negatively on the contact when asked by the GMC.
* Contacts SHOULD assist bounty hunters via feedback, direction, and oversight _upon request_.
* Contacts SHOULD NOT assist the bounty hunter directly with the bounty work.
* Individual incentive payouts for this lever MAY be reduced at the GMC's discretion for any reason. Reasoning must be publicly stated by the GMC.

\* Affiliated here is intended to cover financial or business relationships, familial relationships, friendships pre-dating Rocket Pool, etc. It should not extend to the bonds formed within the RP community. 

### Current and Historic Settings

| Date Changed              | Lever A - Minimum / Maximum | Lever B: Minimum / Maximum | Lever C: Minimum / Maximum |
|---------------------------|-----------------------------|----------------------------|----------------------------|
| 2024-02-16 (RPIP Written) | $0 / 0%                     | $0 / 0%                    | $0 / 0%                    |

<!--

Commentary for Editors:

The above table may be generated using this [google sheet](https://docs.google.com/spreadsheets/d/18JWtUSD6Ej5h2lDLDNEljjWM61XdqqiYkRYTSk8awKA/edit#gid=0), which should be maintained in parallel. If you need to modify the sheet and do not have permission then:
1. Copy the sheet
2. Set it to public read-only
3. Replace this link with your updated sheet
4. Convert the new sheet to markdown using: https://www.tablesgenerator.com/markdown_tables or similar.

-->

## Rationale

**High optionality for the GMC**  
This feels like a bit of a cop-out, but it is preferable to needing pDAO votes in the future to tweak lever values, especially when as yet we don't have any empirical data on how useful these levers each are.

**Modular Incentives**  
Allows for experimentation and flexibility that combining the incentives would not. Does increase complexity.

**The Support Incentive**  
 It is expected and intended for this to at least partially be a proxy for the author because the author determines who is listed as a contact on the definitions they're writing. Authors who can see bounties through from end-to-end as proposer, definer, and contact are pretty valuable to the DAO, and it makes sense that they can collect all three incentives. The specification as-is allows optionality here as well though. Some authors may not want to fulfil this role and they can opt out easily.

**Relative vs Absolute**  
It's hard to know if relative or absolute incentives will work best. Makes sense to allow both for experimentation purposes.

**Reporting Requirement For Contacts**  
The requirement that Bounty Hunters not report negatively on the Bounty Contacts is intended to prevent contacts from failing to provide support when asked because that's the whole thing we want to incentivize. However, 'SHOULD' is used intentionally here to cover the possibility of bounty hunters attempting to take advantage of this requirement.

**High Definition versus Low Definition**  
One of the feedback points that came up during the discussion of this RPIP was whether to define this structure in detail or leave these details primarily to the GMC. On balance, the author feels that a well-defined structural framework agreed upon by the pDAO is preferable to one that is fully flexible for the GMC. There is a great deal of flexibility built into the structure already. The inflexible parts should help to ensure that incentives are deployed transparently and consistently by the GMC in the long term, even as membership fluctuates.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
