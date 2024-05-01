---
rpip: 39
title: Bounty Incentives
description: Proposes to allow the Grants Management Committee to pay out incentives to community members engaging with the bounty system.
author: LongForWisdom (@LongForWisdom)
discussions-to: https://dao.rocketpool.net/t/rpip-39-bounty-incentives/2753
status: Final
type: Meta
created: 2024-02-16
requires: 40
vote-to: https://vote.rocketpool.net/#/proposal/0xde403d91a1d45e958b4ad3c98c45bd07b219a5d498e81a4ade91431a0a979da9
vote-date: 2024-04-24
vote-result: Passed
---

## Abstract

This RPIP proposes to allow the Grants Management Committee to pay out incentives to community members engaging with the bounty system. Incentives are limited to 10% of the total bounty reward amount. The allocation method for bounty incentives is left to the GMC to determine. These incentives would be paid out of the GMC's budget. The pDAO may challenge individual payouts and changes to the incentive allocation method using the existing committee decision challenge system. 

## Motivation
This RPIP is motivated by the desire to see bounties become a consistent and reliable mechanism for the completion of work at the Rocket Pool DAO. The existing bounty system has not been heavily adopted in comparison to the grant and retro frameworks. This is unfortunate, and it represents a missed opportunity.

Bounties can serve as a balanced framework between service providers and the DAO. Grants tend to favor the service provider and represent a greater risk for the DAO. Retros favor the DAO and represent a greater risk to the service provider. Both of these have their place, but bounties provide a nice middle ground. The DAO is de-risked because it can approve work separately from a provider. Providers are de-risked because the DAO has committed to rewarding well-defined milestones.

Currently, there is little recognition that creating an effective and actionable bounty is a non-zero amount of work. Secondly, there is a natural gap in tribal knowledge between members of the Rocket Pool community and crypto service providers. The author hopes that this RPIP will allow the GMC to encourage the community to both propose bounties and support well-meaning service providers that lack some of that deep knowledge of the protocol and DAO.

## Specification

### Bounty Incentives
* The pDAO empowers the Grants Management Committee (GMC) to incentivize the generation and successful completion of bounty proposals using their budget if they choose to do so.
* The total funding allocated to incentives for a given bounty MUST NOT exceed 10% of the bounty reward payout.
  * The total cost of bounty incentives is not otherwise limited. The GMC SHOULD use bounty incentives with caution. 
* Incentives for a given bounty MUST NOT reduce the bounty hunter reward payout.
* Bounty incentives SHOULD be allocated according to a consistent method which the GMC MUST communicate publicly.

### Incentive Payouts
* Bounty incentives MUST be paid out of the GMC budget.
* Incentive payouts MUST be denominated in dollars and SHOULD be paid out in one of the GMC's accepted payout token options, selected by the recipient.
* Incentive payouts SHOULD take place within the GMC's regular payout schedule.
* Incentive payouts SHOULD be announced alongside bounty results for a given GMC round.

### pDAO Recourse
The pDAO MAY use the existing committee challenge system to challenge GMC bounty incentive allocation decisions. This SHOULD take place either:
* When the incentive allocation method is changed. 
* When an incentive is announced to be paid out to an individual for a given bounty.

### Current and Historic Incentive Allocation Methods

The current and historic Bounty Incentive allocation methods should be added to this section by the GMC Administrator when determined by the GMC.

## Rationale

**High Definition versus Low Definition**  
One of the feedback points that came up during the discussion of this RPIP was whether to define this structure in detail or leave these details primarily to the GMC. On balance, the author feels that a well-defined structural framework agreed upon by the pDAO is preferable to one that is fully flexible for the GMC. However, feedback from the community has indicated either the opposite position or no position on this issue. Given that a low-definition RPIP is less of an imposition than a high-definition one, the author opted for this version. If the GMC wishes to use a high-definition framework, they can define and communicate it themselves. The previous, high-definition version of this RPIP can be viewed [here](https://github.com/rocket-pool/RPIPs/blob/3730a0f794730bdeebc9208284728d03457f6131/RPIPs/RPIP-39.md).

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
