---
rpip: 21
title: Understanding oDAO Vote Edge Case
description: Understanding the edge case where the oDAO can pass certain votes below consensus
author: Valdorff (@Valdorff)
discussions-to: https://dao.rocketpool.net/t/avoid-odao-vote-edge-case/1657
status: Final
type: Informational
created: 2023-04-14
---


## Abstract
Issue 5.10 in https://rocketpool.net/files/consensys-diligence-atlas-v1.2.pdf provides a potential
for oDAO votes to pass without a proper consensus. This issue was misunderstood by community members
(including the author of this RPIP) and led to an RPIP that was based on the misunderstanding
(readable in the git history for RPIP-21). This RPIP will now serve to be very clear about what the
issue is and is not.

## What the issue is
- A set of duties get the current member count when calculating if consensus has been met
- This pattern means that if members vote (increment the numerator) then leave (decrement the
  denominator), they can have an outsized impact on a vote
- Duties where this applies includes votes on network balances, network prices, penalties,
  and minipool scrubs (`voteScrub` for new minipools and `voteCancelReduction` for bond reductions)

## What the issue is not
- This importantly does NOT apply to upgrade proposals
- Those proposals go through `RocketDAONodeTrustedProposals.propose` and `_votesRequired` gets
  stored as a part of the proposal, so they are not affected by Issue 5.10

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).