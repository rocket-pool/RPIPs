---
rpip: XX
title: oDAO Timelock
description: 10-day timelock on pDAO contract interactions
author: Mike Leach AKA Wander <GH: VVander | Discord: Wander#4793>
discussions-to: TBD
status: Draft
type: Protocol (Core)
created: 2022-09-13
---

## Abstract

The pDAO guardian contract has the unique privilege of changing some parameters of the protocol and has a significant portion of inflation allocated to it (15% currently). In an effort to increase security, this proposal suggests a 10-day delay to all contract interactions.

## Motivation
A delay on all pDAO contract interactions allows time for the oDAO to deploy a contract upgrade in case the pDAO is compromised.

## Specification
TBD

## Rationale
10 days is chosen as a compromise between reasonable speed of action by the pDAO guardian and a reasonable time for an oDAO response.

## Security Considerations
The basic purpose of this proposal is to increase protocol security outright.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
