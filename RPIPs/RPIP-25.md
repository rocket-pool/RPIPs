---
rpip: 25
title: RPL Inflation Allocation
description: Describes the planned usage of RPL inflation
author: jasperthegovghost (@jasperthefriendlyghost), Valdorff (@Valdorff)
discussions-to: https://dao.rocketpool.net/t/odao-charter-draft-and-more/1832
status: Living
type: Meta
created: 2023-06-02
vote-to: https://vote.rocketpool.net/#/proposal/0x510383ca82a0096fa670a260692cf7a4097e199ce4f731dc4efd97a21f19f988
vote-date: 2023-07-17
vote-result: Passed
---


## Abstract
This RPIP provides the desired allocation of new inflation and provides context around that split.

## Motivation
Now that snapshot voting is available to allow "the RP community" to express their will, it is
important to use that will to direct the pDAO's funds as we see fit. These funds can be used for
many purposes, which can change over time, so this RPIP will be a Living document that can reflect
changes to how the funds should be directed.

## Specification
The current RPL inflation allocation is:

- Node Operators (`rocketClaimNode`) - 50%
- pDAO (`rocketClaimDAO`) - 47.5%
- oDAO (`rocketClaimTrustedNode`) - 2.5%

After Saturn 2, RPL inflation allocation SHALL be set to:

- Node Operators (`rocketClaimNode`) - 0%
- pDAO (`rocketClaimDAO`) - 95%
- oDAO (`rocketClaimTrustedNode`) - 5%

### Updating this RPIP
This is a "Living" RPIP, so that it can be kept up to date with changes to Inflation Allocation.
There SHALL NOT be any merged updates (beyond correcting errata and add non-normative
clarifications) unless they are accepted by pDAO vote. The following process SHALL be followed to
update this RPIP:

1. An author SHALL make edits as desired and create a pull request; the status in the PR SHALL be
   "Draft"
2. An RPIP editor SHALL review it. If there are no structural issues, the status shall be changed
   to "Review"
3. The vote procedure SHALL be followed as if this were an entirely new RPIP. This includes making
   forum posts, appropriate wait times, quorums, etcetera, as detailed elsewhere.
4. If criteria for a vote are not met, the status SHALL be changed to "No Vote Taken" and the PR
   SHALL be closed.
5. The inflation allocation to the oDAO SHOULD NOT be changed (up or down) based on RPL price. Price
   risk was already considered in the selection of the inflation allocation.
6. If a vote is carried out, it must reach a supermajority of 75% "For" in order to pass
7. If there was a vote, then after the vote is concluded:
   1. If the vote passed, the status SHALL be changed to "Living" and the PR SHALL be merged
   2. If the vote failed, the status SHALL be changed to "Vote Failed" and the PR SHALL be closed

## Rationale
1. Payments to the oDAO:
    1. Must cover ongoing expenses and initial expenses
    2. Should help keep members eager to serve in their role
    3. Should mitigate the chance of bribery
2. Currently, inflation is split up 70% to NOs, 15% to pDAO, 15% to oDAO
3. The Arbitrum security council was the most apt comparison based on powers and TVL in the protocol
    1. They are paid $60k/yr per seat (USD denominated)
    2. The oDAO have duties beyond protocol updates
    3. The oDAO take on price risk
    4. Other researched groups included the security multisigs for Aave, Balancer, Hop, Stakewise
       and Aura; as well as the oracle networks for Maker, Stakewise, and Lido.
4. We’re targeting
    1. An 11-15 member oDAO long-term
       1. The pDAO based long-term inflation numbers on 13 seats
       2. The pDAO recommend ~11-15 members as effective for their duties and well-rewarded 
    2. The same rough ballpark as the Arbitrum security council per seat, but a bit higher
    3. Decreasing pay over time to help align with the goals of removing duties and eventually
     eliminating the oDAO

## Historical Inflation Allocation

| Date         | Allocation                                                                                                      |
|--------------|-----------------------------------------------------------------------------------------------------------------|
| 2021-09-30   | Node Operators (`rocketClaimNode`) - 70%, pDAO (`rocketClaimDAO`) - 15%, oDAO (`rocketClaimTrustedNode`) - 15%  |
| 2023-11-14   | RPIP-25 approved a staged transition* from 70% Node Operators / 22% pDAO / 8% oDAO to 70% Node Operators / 28.5% 
| 2025-03-24   | Node Operators (`rocketClaimNode`) - 70%, pDAO (`rocketClaimDAO`) - 27.5%, oDAO (`rocketClaimTrustedNode`) - 2.5%  |
| 2026-07-10   | Node Operators (`rocketClaimNode`) - 50%, pDAO (`rocketClaimDAO`) - 47.5%, oDAO (`rocketClaimTrustedNode`) - 2.5%  |

*RPIP-25 originally specified the following staged pDAO/oDAO allocation changes, while keeping Node Operators at 70%:

| Transition | Timing | Node Operators | pDAO | oDAO |
|------------|--------|----------------|------|------|
| 0 | Initial | 70% | 22% | 8% |
| 1 | 2 reward periods later | 70% | 23% | 7% |
| 2 | 2 reward periods later | 70% | 24.1% | 5.9% |
| 3 | 2 reward periods later | 70% | 25.2% | 4.8% |
| 4 | 2 reward periods later | 70% | 26.3% | 3.7% |
| 5 | 2 reward periods later | 70% | 27.4% | 2.6% |
| 6 | 2 reward periods later | 70% | 28.5% | 1.5% |

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
