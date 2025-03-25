---
rpip: 68
title: Increasing oDAO Share of Inflation
description: Increase the oDAO share of inflation to appropriately cover costs
author: Valdorff (@Valdorff)
discussions-to: https://dao.rocketpool.net/t/odao-inflation-vote/3525
status: Final
type: Protocol
category: Core
created: 2025-24-05
requires: 24, 25
vote-to:
vote-date:
vote-result:

---

## Abstract
This RPIP increases the share of inflation to the oDAO from 1.5% to 2.5%. The share to the pDAO is reduced from 28.5% to 27.5% to fund this. The 70% share to node operators is unchanged.

## Specification
- The pDAO Guardian SHALL call bootstrapSettingClaimers with _trustedNodePercent=25000000000000000, _protocolPercent=275000000000000000, and _nodePercent=700000000000000000
- The pDAO suggest kicking rocketpool-t (following the RPIP-24 process)
- Per [RPIP-25](https://rpips.rocketpool.net/RPIPs/RPIP-25), a vote modifying inflation allocation must reach a 75% supermajority to pass

## Rationale

### Inflation changes
At the current rate, a single oDAO seat is estimated to earn 988 RPL this year. At the time of this writing, that’s 2.52 ETH. In the last 12 months, the rocket scientist oDAO seat spent 1.91 ETH on gas. This means that amortized hardware and all other operating expenses have to fit in the remaining 0.61 ETH to break even. See [Ken’s breakdown of costs](https://discord.com/channels/405159462932971535/405163713063288832/1341509490423632005) for a bit more detail.

This RPIP tries to provide a bit more breathing room for oDAO seats before operating at a loss by increasing pay per seat by 67%.

#### Addressing RPL price
RPIP-25 establishes that
> The inflation allocation to the oDAO SHOULD NOT be changed (up or down) based on RPL price. Price risk was already considered in the selection of the inflation allocation.

[RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119) defines:
> **SHOULD NOT**   This phrase, or the phrase "NOT RECOMMENDED" mean that there may exist valid reasons in particular circumstances when the particular behavior is acceptable or even useful, but the full implications should be understood and the case carefully weighed before implementing any behavior described with this label.

Clearly, we are acting to change the inflation allocation based on RPL price. Since oDAO nodes now face potentially operating at a loss, we believe this required action despite the recommendation in RPIP-25. If RPL price recovers enough to take away the realistic threat of operating at a loss, we would expect to reduce the oDAO allocation back again, such that the initial amount of “price risk” is reflected. 

### oDAO membership suggestions
[RPIP-24](https://rpips.rocketpool.net/RPIPs/RPIP-24) process establishes that:
- The pDAO MAY nominate a new member or suggest the kicking of an existing member via snapshot vote; if such a vote passes:
  - The oDAO SHALL internally vote on the membership change, and execute the change if threshold for membership change reached
  - The oDAO SHALL post a statement with rationale for the decision

In this RPIP, the pDAO formally suggests kicking one seat to trigger that process; however, we ask that the oDAO more generally reconsider their membership.

While the oDAO are ultimately in control of their own membership, we ask that they keep in mind that:
- [RPIP-25](https://rpips.rocketpool.net/RPIPs/RPIP-25) was targeted at funding 11-15 seats with a suggested size of 13
  - Going down from the current 16 seats to 13 seats would increase pay per seat by 23%, which is important given that tight pay is a consideration
-  Multiple Rocket Pool Pty Ltd. seats reduces the theoretical cost of attack if we treat the company as a single entity
- There may be valid reasons to remove other seats, such as how well the seat has lived up to the values/responsibilities in the oDAO charter or a seat wishing to step down

## Additional work
The authors of this RPIP will submit a bounty for a watchtower upgrade that will help reduce oDAO gas costs.

Right now, oDAO nodes carry out their duties beyond consensus. There’s a bit of a detail here in timing: if consensus is hit before a node hits its periodic duty check, then the node won’t carry out its duty. Looking at a fairly stable period in the fall, it looks like ~90% of nodes submitted per duty.

The reason for voting past consensus was to get oDAO health metrics per [RPIP-19](https://rpips.rocketpool.net/RPIPs/RPIP-19). In the interest of saving gas, the bounty would be for creating a scheme where (a) only votes up to consensus are submitted and (b) all oDAO nodes take on each duty regularly so that we can still monitor health. This would reduce gas costs ~37.5% in a 16-seat oDAO or ~40% in a 13-seat oDAO. This would be equivalent to a ~60-67% increase if gas were the only cost; insofar as it’s not, this isn’t quite that impactful, but it seems a prudent step regardless.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
