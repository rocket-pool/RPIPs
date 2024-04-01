---
rpip:
title: Lower Minipool Requirements
description: Remove the RPL staking requirement; allow 4 ETH of NO bond
author: Valdorff (@Valdorff)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-15
---

## Abstract
This proposal lowers requirements for minipools. This makes them more attractive. Additionally, the lower required NO bond will allow for higher capital efficiency. Note that this proposal removes the current RPL value capture methodology, so there should be a new form of value capture to replace it (either immediately or concretely planned)


## Specification
- Node Operators SHALL be able to create minipools with 4 bonded ETH
- Node Operators SHALL NOT be required to stake RPL in order to create minipools
- If [rpip-uvc](./draft-uvc.md) is not implemented at the same time, rETH commission SHALL be set to 8%

## Notes
With an 8% rETH commission, these pools will be higher earners than current LEB8s (boost over solo: `28*.08/4=56%` vs `24*.14/8=42%`). This also gives rETH holders an APR boost for this time period. When UVC comes online the rETH commission and share to NOs will be rebalanced according to those settings.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
