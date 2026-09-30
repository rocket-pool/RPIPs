---
rpip: TBD
title: Increase Protocol Development Funding
description: Amends RPIP-37 to fund protocol development from 95% of the pDAO Reserve Treasury allocation.
author: Darren Langley (@langers)
discussions-to: https://dao.rocketpool.net/t/protocol-development-funding/4029
status: Draft
type: Meta
created: 2026-09-03
requires: RPIP-10, RPIP-37, RPIP-81
vote-to:
vote-date:
vote-result:
tags: [protocol-development-funding, pdao-reserve]
---

## Abstract

[RPIP-37](RPIP-37.md) sets protocol development funding at "5% of RPL inflation", drawn as a yearly lump sum from the pDAO reserve. Since the introduction of [RPIP-81](RPIP-81.md), that 5% of inflation is approximately 35% of the annual pDAO Reserve Treasury allocation, or roughly 55,000&ndash;57,000 RPL per year (the most recent annual disbursement, in October 2025, was 54,683 RPL &mdash; see [RPIP-10](RPIP-10.md) `Direct expenses`).

This RPIP amends the Funding section of RPIP-37 so that protocol development funding is instead set to **95% of the pDAO Reserve Treasury allocation** ([RPIP-10](RPIP-10.md)), approximately 150,000 RPL per year. The annual pDAO budget vote, the lump-sum disbursement mechanism, and all other provisions of RPIP-37 are unchanged.

> All USD figures in this RPIP are indicative only, quoted at an RPL price of ~$1.73 (7 September 2026).

## Motivation

The macro environment has been tough on token valuations. In prior years protocol funding covered the core team's operating costs comfortably, but that has not been the case since mid-2024. The team has been drawing on ICO funds (the "dev wallet") to bridge the shortfall, which shortens runway.

The core team runs a lean operation and reviews operating expenditure closely, but current operating costs run between roughly $1.1M and $1.5M per year (the variation is largely audit-driven), while RPIP-37 funding currently delivers in the order of $95,000 per year (~56,000 RPL). The magnitude of the shortfall is stark.

Income from protocol-owned assets is expected to provide a meaningful supplement over time, but that is not yet available at scale. Protocol funding remains the core team's primary income source for the foreseeable future.

Reducing the team's burn rate at this point in the cycle preserves the ability to keep delivering at a critical moment for the protocol and to be well positioned when conditions improve and adoption accelerates. Retaining talent through the downturns, not only the upturns, has been a consistent priority.

## Specification

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

The **Funding** section of [RPIP-37](RPIP-37.md) SHALL be replaced in its entirety with the following:

> ### Funding
>
> - Each year, the pDAO MUST vote to agree a protocol development funding budget.
> - On a successful vote, the pDAO MUST pay a lump-sum payment from the pDAO reserve to cover protocol development costs for the upcoming year.
> - Protocol development funding is set to **95%** of the pDAO Reserve Treasury allocation, as defined in [RPIP-10](https://rpips.rocketpool.net/RPIPs/RPIP-10).
> - The pDAO MAY vote to update this RPIP and change the percentage as protocol and market conditions evolve.

No other section of RPIP-37 is modified by this RPIP.

The RPL amount for the upcoming year's lump sum SHALL be calculated as 95% of the RPL allocated to the Reserve Treasury category over the funding period, using the inflation allocation and pDAO internal split in effect at the time of the vote ([RPIP-81](RPIP-81.md), recorded in [RPIP-10](RPIP-10.md)'s `Historical budget splits`).

## Rationale

### Why express funding as a share of the Reserve Treasury

RPIP-37 already draws protocol development funding from the pDAO reserve as a direct Reserve Treasury expense. Expressing the funding level as a share of the Reserve Treasury allocation, rather than as a share of total RPL inflation, keeps the parameter aligned with the pool the money actually comes from.

### Why 95%

95% closes part of the gap between current RPIP-37 funding (~56,000 RPL, in the order of $95,000/year) and the core team's operating costs ($1.1M&ndash;$1.5M/year), taking the annual draw to roughly 150,000 RPL (in the order of $260,000/year at ~$1.73/RPL), while leaving a 5% residual in the Reserve Treasury for other direct pDAO reserve expenses. It is deliberately framed as a level that should be re-evaluated as market conditions improve rather than a permanent entitlement.

### What is not changed

- The annual pDAO budget vote in RPIP-37 is retained; this RPIP changes the default amount that vote ratifies, not the requirement to vote.
- The Prioritisation, Roadmap, Governance, and Process sections of RPIP-37 are unchanged.

## Backwards Compatibility

Prior lump-sum disbursements (recorded in [RPIP-10](RPIP-10.md)'s `Direct expenses`) are unaffected. The new percentage applies from the next funding period.

## Test Cases

Not applicable; this is a Meta RPIP that does not introduce protocol changes.

## Reference Implementation

The change is realised by:

1. Merging the amended Funding section into [RPIP-37](RPIP-37.md).
2. The pDAO's next annual protocol development funding vote ratifying a lump sum equal to 95% of the Reserve Treasury allocation for the period.
3. The on-chain pDAO proposal that pays that lump sum from the reserve, per [RPIP-10](RPIP-10.md), with the expense recorded in RPIP-10's `Direct expenses` table.

## Security Considerations

No security considerations.

## Governance Considerations

This RPIP amends a Meta RPIP and follows the standard RPIP process: forum discussion, editor review, and a pDAO snapshot vote under the usual quorum and majority rules.

This RPIP does not itself approve any specific disbursement; each year's lump sum is still ratified by the RPIP-37 budget vote and paid via an on-chain pDAO proposal.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
