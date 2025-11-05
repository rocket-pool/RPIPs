---
rpip: 75
title: Change Express Ticket Allocation for Saturn 1
description: Change express ticket allocation for a more balanced megapool migration given the current and expected post Saturn 1 modest rETH growth.
author: Epineph, NeverAnIsland (@NeverAnIsland)
discussions-to: https://dao.rocketpool.net/t/reduce-express-ticket-allocation/3749
status: Review
type: Protocol
category: Core
created: 2025-10-24
requires:
vote-to:
vote-date:
vote-result:
tags:
---

## Abstract
The RPIP proposes to reduce `express_queue_tickets_base_provision` to 0 and increase `express_queue_rate` parameter to 4. This will decrease the total number of express tickets offered to existing node operators and eliminate the base express tickets offered to nodes.  At the same time, this will increase the proportion of new validators preferentially coming from the express queue to reduce uncertainty for migrating NOs.

## Motivation
Due to various changes in staking assumptions since the tokenomic changes were initially envisioned, the number of express tickets distributed are likely to greatly outweigh the number of ETH deposits available to be matched, overloading and ultimately defeating the point of the express queue.

## Specification
The RPIP editors SHALL modify RPIP-59 as follows
- Set `express_queue_tickets_base_provision` to 0 (from 2);
- Set `express_queue_rate` to 4 (from 2).

## Rationale
The idea behind priority ticket distributions was intended to prevent an outflow of NO operator ETH in the run-up to Saturn 1, which would hazardous to both rETH earnings and RPL value; it would do so by taking away some of the variability.

At that time, a 10% RPL requirement meant we were highly NO limited. The NO demand problem was anticipated to get worse as we got closer to Saturn 1 as there is no direct migration of NO ETH. Moreover, NOs exiting to get access to Saturn benefits would potentially create a huge amount of unproductive ETH for months before Saturn.

The priority ticket schema was intended to give benefit to NOs to continue or even join staking up until the release of Saturn 1. However, in the intervening time we have released Saturn 0, which

1. removed the RPL requirement and thus the RPL protective goal of priority tickets;
2. eliminated the NO shortage and thus the rETH protective goal of priority tickets.

We have also seen that rETH demand is shallower than anticipated, suggesting that there are not hundreds of thousands of rETH waiting to be staked after Saturn 1 launches. Under current distribution scheme ~600,000 additional ETH would need to be staked just to provide enough to migrate existing NOs, assuming no new NOs (~900,000 ETH if there are new NO ETH deposited after Saturn 1).

The new distribution of express tickets ensure that:

1. There is no vector for an attack where sybiled megapools can clog the priority queue from existing NOs;
2. Every NO who is validating at the release of Saturn 1 has priority for at least two megapool validators;
3. There are enough express tickets so that >99% of NOs can get all their RPL effectively staked from express tickets;
4. If 100% of NOs migrated, all express tickets could be used without additional rETH deposits;
5. Although fewer tickets exist, they provide a more reliable migration pathway for existing NOs to migrate some of their stake;
6. There will be a more diverse group of NOs migrating some stake to megapools, instead of a smaller group migrating all their stake.

## Considerations
This is an alteration of a living RPIP coming in towards the last minute, and changes towards investing assumptions should not be taken lightly.  However, in this case it seems unlikely that there are NOs who are remain staking in order to get express tickets.  Even if there were NOs who were invested based on the idea they would get tickets, it is highly unlikely that the allocated express tickets will actually be usable.

If the number of express tickets is too low to allow sufficient migration (i.e., priority queue empty and many NOs remain in legacy minipools), this can be adjusted during the next smart contract upgrade.

## Security Considerations
This is a change to the smart contract that has not been fully through the audit life cycle.  However, these changes to constants are overall lower risk than changing the logic of the smart contract.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
