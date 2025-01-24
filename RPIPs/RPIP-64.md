---
rpip: 64
title: On-Chain pDAO Quora
description: This proposal defines initial values for the quorum parameters of the on-chain pDAO.
author: haloooloolo (@haloooloolo)
discussions-to: https://dao.rocketpool.net/t/on-chain-quorum-initialization-poap/3474
status: Draft
type: Protocol
created: 2025-01-11
requires: 4, 33
---

## Abstract
This proposal aims to define initial quorum values for the Rocket Pool Protocol DAO ahead of its first on-chain vote. [RPIP-33](RPIP-33.md) introduced a standard vote quorum analogous to the pre-existing Snapshot quorum. Moreover, it added a veto quorum which, if met, allows for penalization of the vote's proposer. Both of these parameters are to be set to values that allow for effective and secure governance.

## Specification
- `proposal.quorum` SHALL be set to 15%
- `proposal.veto.quorum` SHALL be set to 20%

## Rationale
### Proposal Quorum
The `proposal.quorum` parameter is to be interpreted as a share of vote power that has been initialized on-chain. In order for a proposal to succeed, the sum of its `for`, `against`, and `abstain` votes must equal or exceed quorum. This is similar in concept to the pDAO Snapshot quorum, which [RPIP-4](RPIP-4.md) defines as 15% of total pDAO vote power. In the past, this on-chain parameter has been a moving target due to the ever-increasing amount of vote power initialized by node operators. The intention is for these two quora to represent the same amount of vote power despite this discrepancy in definition. [RPIP-63](RPIP-63.md) first attempted to account for this by setting `proposal.quorum` to 30% after half of all vote power had been initialized. Due to a recent community initiative, all on-chain vote power is now active, which allows for a final parameter change to 15%.

### Proposal Veto Quorum
Similarly to `proposal.quorum`, `proposal.veto.quorum` represents a share of initialized vote power. When voting against a proposal, pDAO voters have the option to add a veto. If the amount of veto votes equals or exceeds the veto quorum, the proposal is rejected and the proposer's RPL bond is burned, even if a majority voted in favor. The mechanism acts as a defense against malicious or generally undesirable proposals. Setting the veto quorum too high would invalidate the mechanism; on the other side, setting it too low facilitates governance capture. Past pDAO proposals have seen vote participation fluctuate between 15% and 30%. Official delegates represent 21.6%, the largest 10 of whom can combine to reach 15%. Given the nature of the quorum and the current distribution of vote power, a veto quorum of 20% is suggested as a reasonable tradeoff for the protocol.
