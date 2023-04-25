---
rpip: 21
title: Fix oDAO Vote Edge Case
description: Fix an edge case where the oDAO can pass proposals below consensus threshold
author: Valdorff (@Valdorff)
discussions-to: https://dao.rocketpool.net/t/avoid-odao-vote-edge-case/1657
status: Review
type: Protocol
category: Core
created: 2023-04-14
---


## Abstract
Issue 5.10 in https://rocketpool.net/files/consensys-diligence-atlas-v1.2.pdf provides a potential
for oDAO votes to pass without a proper consensus. While the timeline may have made fixing the issue
untenable for Atlas, I believe this should be a priority for the next significant smart contract
release.

## Motivation
### Need for security
Consensus for oDAO votes is based on being over a threshold percentage of the current oDAO size. The
issue pointed out in 5.10 is that oDAO members that leave still have their votes counted (in the
numerator), but the threshold is based on the new smaller size (in the denominator). This allows a
cabal smaller than threshold to all vote, have some members leave, and pass any arbitrary proposal
without reaching threshold. In effect, it creates a secret smaller threshold at
`cabal_members > (threshold_proportion*(total_members + 1))/(threshold_proportion + 1)` - as an
example, that would allow 7 members to pass any proposal in a 19-member oDAO with a >50% threshold
despite only having 37% of the votes. Note that "any proposal" can be far-reaching, such as
permanently transferring upgrade control to their own members.

### Need for priority
The team responded to 5.10:
> We are aware of this limitation but the additional changes required to implement a fix outweigh the concern in our opinion

Compare with the response in 5.11:
> We are aware of this limitation but making this change now with an existing deployment outweighs the concern in our opinion.

At face value, this reads as the team stating that they do not believe this should be fixed. This
RPIP makes it a required priority to fix this.

## Specification
The next significant smart contract release SHALL prevent oDAO proposals from passing without
meeting the intended consensus.

## Reference Implementation
A simple implementation that achieves the goal would be to:
- Record the number of oDAO members when a vote starts; this will be used as the denominator
- Increment this number for active votes when a new member joins
- Do NOT decrement if an oDAO member leaves

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).