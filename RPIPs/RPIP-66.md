---
rpip: 66
title: Saturn Follow-up Vote No. 1
description: Follow-up vote to tie up loose ends in the Saturn specification
author: Valdorff (@Valdorff)
discussions-to: https://dao.rocketpool.net/t/saturn-follow-up-votes-1/3504
status: Final
type: Protocol
category: Core
created: 2025-02-03
requires: 46, 59
vote-to: https://vote.rocketpool.net/#/proposal/0xa7ba503f5f6916cba3803222aaf006b8dc35854e19fbac2f88f6eed06d0748ee
vote-date: 2025-03-19
vote-result: Passed
tags: tokenomics-2024, tokenomics-content
---

## Abstract
This RPIP supports a follow-up vote ratifying plans to tie up loose ends in the Saturn specification. It also provides a platform for community members to raise and discuss any as-yet-unnoticed problems in those plans.

## Specification
- RPIP-46 SHALL define `vote_eligible_RPL_in_their_megapool` as `min(1.5*RPL value of megapool bonded_eth, megapool staked rpl)`
  - This results in a small change in text to RPIP-46 (which is currently in the Living state)
- RPIP-59 SHALL remove scrubbing and `scrub_period` 
- RPIP-59 SHALL add a minimum value to `time_before_dissolve` of 2 days

## Rationale

### vote_eligible_RPL_in_their_megapool
This tweak is needed to avoid ambiguity. Full discord discussion is [here](https://discord.com/channels/405159462932971535/1215788197842255972/1333479516244410471), with an excerpt provided:
> Hmm... so RPIP-43 seems unambiguous about vote_eligible_rpl. For your example: min(1.5*(80+4), 150+24) = 126.
> 
> The RPIP-46 bit does seem insufficient: vote_eligible_RPL_in_their_megapool doesn't define how to count the specific subsets of RPL.

Thanks to @sckuzzle for finding this issue.

### time_before_dissolve guardrail
This dev team suggestion prevents setting the parameter so low that Node Operators can't effectively make deposits.

### scrubbing removal
Per the dev team, there is no need for a scrub process in Saturn due to stake proofs -- as a result, we can simplify this away.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).