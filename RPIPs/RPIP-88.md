---
rpip: 88
title: Protocol Development
description: Determines the process around core protocol development and funding
author: Darren Langley (@langers)
discussions-to: https://dao.rocketpool.net/t/dev-team-funding/2038
status: Draft
type: Meta
created: 2026-09-03
requires: RPIP-10, RPIP-37, RPIP-81
vote-to: 
vote-date: 
vote-result: 
---

## Abstract
To ensure the long-term sustainability and continued innovation of the Rocket Pool protocol, this document lays out the high-level roles, responsibilities, processes, and funding for core protocol development.

## Motivation

> All USD figures in this RPIP are indicative only, quoted at an RPL price of ~$1.53 (20 October 2026).

The macro environment has been tough on token valuations. In prior years protocol funding covered the core team's operating costs comfortably, but that has not been the case since mid-2024. The team has been drawing on ICO funds (the "dev wallet") to bridge the shortfall, which shortens runway.

The core team runs a lean operation and reviews operating expenditure closely, but current operating costs run between roughly $1.1M and $1.5M per year (the variation is largely audit-driven), while RPIP-37 funding currently delivers in the order of $85,000 per year (~56,000 RPL). The magnitude of the shortfall is stark.

Income from protocol-owned assets is expected to provide a meaningful supplement over time, but that is not yet available at scale. Protocol funding remains the core team's primary income source for the foreseeable future.

Reducing the team's burn rate at this point in the cycle preserves the ability to keep delivering at a critical moment for the protocol and to be well positioned when conditions improve and adoption accelerates. Retaining talent through the downturns, not only the upturns, has been a consistent priority.

[RPIP-37](RPIP-37.md) sets protocol development funding at "5% of RPL inflation", drawn as a yearly lump sum from the pDAO reserve. Since the introduction of [RPIP-81](RPIP-81.md), that 5% of inflation is approximately 35% of the annual pDAO Reserve Treasury allocation, or roughly 55,000-57,000 RPL per year (the most recent annual disbursement, in October 2025, was 54,683 RPL &mdash; see [RPIP-10](RPIP-10.md) `Direct expenses`). This RPIP instead sets protocol development funding at **80% of the pDAO Reserve Treasury allocation** ([RPIP-10](RPIP-10.md)), approximately 128,000 RPL per year.

## Related RPIPs
- [RPIP-37: Protocol Development, superseded by this RPIP](RPIP-37.md)
- [RPIP-10: pDAO Budget Allocation](RPIP-10.md)
- [RPIP-24: oDAO Charter](RPIP-24.md)
- [RPIP-25: RPL Inflation Allocation](RPIP-25.md)
- [RPIP-81: Rebalance RPL Inflation for Protocol Funding](RPIP-81.md)

## Specification

### Prioritisation

- Each year, the Core Protocol Team MUST facilitate a prioritisation discussion with the Rocket Pool community.
- pDAO SHOULD engage in the prioritisation discussion to ensure development team prioritises are aligned with pDAO expectations.
- The Core Protocol Team MUST summarise findings of prioritisation discussion and communicate them to the pDAO.

### Roadmap

- The Core Protocol Team MUST define an indicative protocol roadmap or release schedule based on the prioritisation findings.
- The Core Protocol Team MUST provide quarterly updates on the roadmap.
- The protocol roadmap is expected to evolve over time, the Core Protocol Team SHOULD communicate these changes in the quarterly updates. 

### Governance

- The Core Protocol Team MUST draft Rocket Pool Improvement Proposals (RPIPs) for new protocol features.
- pDAO SHOULD engage in discussion and co-development of the new protocol features.
- In order for new protocol features to be included, they MUST be approved by a pDAO vote.

### Funding

- Each year, the pDAO MUST vote to agree a protocol development funding budget.
- On a successful vote, the pDAO MUST pay a lump-sum payment from the pDAO reserve to cover protocol development costs for the upcoming year.
- Protocol development funding is set to **80%** of the pDAO Reserve Treasury allocation, as defined in [RPIP-10](RPIP-10.md).
- The pDAO MAY vote to update this RPIP and change the percentage as protocol and market conditions evolve.

### Process

- The Core Protocol Team SHOULD engage in continuous process improvement.
- The Protocol DAO (pDAO) SHOULD engage in providing feedback on the development process. 

## Rationale

### What is not changed from RPIP-37

- All sections other than Funding are unchanged.
- The first 2 bullets of the Funding section are unchanged.
- The funding is set to a higher level and expressed differently.
- Re-evaluating with market conditions aspect is highlighted with a new bullet.

### Why express funding as a share of the Reserve Treasury

RPIP-37 already draws protocol development funding from the pDAO reserve as a direct Reserve Treasury expense. Expressing the funding level as a share of the Reserve Treasury allocation, rather than as a share of total RPL inflation, keeps the parameter aligned with the pool the money actually comes from.

### Why 80%

80% closes part of the gap between current RPIP-37 funding (~56,000 RPL, in the order of $85,000/year) and the core team's operating costs ($1.1M-$1.5M/year), taking the annual draw to roughly 128,000 RPL (in the order of $195,000/year at ~$1.53/RPL), while leaving a 20% residual in the Reserve Treasury for other direct pDAO reserve expenses. It is deliberately framed as a level that should be re-evaluated as market conditions improve rather than a permanent entitlement.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).