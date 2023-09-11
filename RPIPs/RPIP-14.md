---
rpip: 14
title: Temporary pDAO Guardian Reassignment
description: Temporarily reassign pDAO guardian from dev team to oDAO
author: 'Mike Leach AKA Wander (GH: VVander | Discord: Wander#4793)'
discussions-to: https://dao.rocketpool.net/t/proposal-for-temporary-pdao-guardian-reassignment/1024
status: Finalized
type: Meta
created: 2022-09-07
---

## Abstract

Currently, the pDAO guardian is held solely by Rocket Pool Pty Ltd. In an effort to increase security, this proposal suggests a temporary reassignment of the pDAO guardianship to a multi-sig controlled by the oDAO members until the pDAO is designed and developed more thoroughly.

## Specification
A 8/14 multi-sig wallet consisting of the current (2022-09-07) oDAO members SHALL be created, and the pDAO guardian ownership SHALL be immediately but temporarily transferred to this wallet.

## Rationale
This is explicitly proposed as a temporary measure because there is no broad agreement yet on a better design for the pDAO guardian. 

The oDAO currently already has the same technical capabilities as the pDAO guardian via contract upgrades, and this will not change regardless of the design the pDAO uses, so it's a logical choice for a temporary guardian.

## Security Considerations
The basic purpose of this proposal is to increase protocol security by increasing the number of signers needed for the pDAO guardian to make transactions.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
