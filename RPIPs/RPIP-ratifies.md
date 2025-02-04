---
rpip:
title: Saturn Followup Vote No. 1
description: Followup vote to tie up loose ends in the Saturn specification
author: Valdorff (@Valdorff)
discussions-to: 
status: Draft
type: Protocol
category: Core
created: 2025-02-03
requires: 46, 59
vote-to:
vote-date:
vote-result:
tags: tokenomics-2024, tokenomics-content
---

## Abstract
This RPIP supports a followup vote to tie up loose ends in the Saturn specification

## Specification
- Megapools SHALL use a 2 transaction deposit strategy
  - This is already how RPIP-59 is written. This vote will ratify that decision and make no change to the existing text.
- RPIP-46 SHALL define `vote_eligible_RPL_in_their_megapool` as `min(1.5*RPL value of megapool bonded_eth, megapool staked rpl)`
  - This results in a small change in text to RPIP-46 (which is currently in the Living state) 

## Rationale

### 2 Tx deposit strategy

Kane had been the strongest voice towards 3 Tx. In discord he recently said:
> knoshua's post [here](https://discord.com/channels/405159462932971535/1215788197842255972/1261344716394463392) removed the weak preference i held for the 3 tx option so i don't have any strong desire to argue in favour of it. i no longer think the benefit of removing all assignments from user deposits outweighs the increased simplicity of the 2 tx approach. and the removal of socialised assignments assuage my main concern in that regard anyway.


### vote_eligible_RPL_in_their_megapool
This tweak is needed to avoid ambiguity. Full discord discussion is [here](https://discord.com/channels/405159462932971535/1215788197842255972/1333479516244410471), with an excerpt provided:
> Hmm... so RPIP-43 seems unambiguous about vote_eligible_rpl. For your example: min(1.5*(80+4), 150+24) = 126.
> 
> The RPIP-46 bit does seem insufficient: vote_eligible_RPL_in_their_megapool doesn't define how to count the specific subsets of RPL.

Thanks to @sckuzzle for finding this issue. 

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).