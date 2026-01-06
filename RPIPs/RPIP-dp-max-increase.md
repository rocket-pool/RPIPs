---
rpip:
title: Increase Deposit Pool Maximum to 100,000 ETH
description: Increase the Deposit Pool maximum ETH limit to allow additional buffer ahead of Saturn 1 megapool launch.
author: Dr Doofus (@DrDoofus-MD-PhD-DDS)
discussions-to:
status: Draft
type: Protocol
category: Core
created: 2026-01-06
requires: RPIP-33
vote-to: Change deposit.pool.maximum to 100,000 ETH
vote-date:
vote-result:
tags: [deposit-pool, saturn-1, megapools, parameters]
---

## Abstract

This RPIP proposes increasing the Deposit Pool maximum balance from 18,000 ETH to 100,000 ETH by updating the on-chain parameter:

`deposit.pool.maximum = 100,000 ETH`

This RPIP is required to enable such an on-chain vote.

## Motivation

There are two primary motivations for this change:

- **Saturn 1 Megapool Readiness**: With the planned launch of Saturn 1 and the introduction of megapools targeted for February 9, it is possible that larger ETH inflows may occur. Increasing the Deposit Pool cap allows ETH to accumulate in advance, ensuring smoother megapool onboarding.

- **Avoiding the RPIP-74 80% Threshold**: The Deposit Pool is approaching levels that risk crossing the 80% utilization threshold defined in RPIP-74, which would automatically re-enable the minipool queue. Increasing the maximum provides operational headroom and reduces the risk of unintentionally toggling queue behavior during a critical transition period.

## Specification

### Scope

The following parameter SHALL be updated via onchain governance vote upon passage of this RPIP:

Parameter: `deposit.pool.maximum`

**Current Value: `18,000 ETH`**

**Proposed Value: `100,000` ETH**

No other parameters or behaviors are modified by this RPIP.

## Rationale

The change is purely a capacity adjustment to build deposit pool ETH in anticipation of megapools in Saturn 1.

Although rETH yield will take a temporary hit with potentially increased deposit pool ETH, the increased capital efficiency of megapools and number of Node Operators waiting to migrate should quickly use up stored ETH.

## Backwards Compatibility

This change is fully backwards compatible. Existing contracts, minipools, and node operator behavior remain unaffected.

## Security and Other Considerations

No new attack vectors are introduced.

## References

RPIP-33: Governance-gated parameter changes

RPIP-74: Minipool queue activation based on Deposit Pool utilization

Saturn 1: Upcoming protocol upgrade introducing megapools

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
