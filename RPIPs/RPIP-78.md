---
rpip: 78
title: Increase Deposit Pool Maximum
description: Increase the Deposit Pool maximum to allow additional buffer ahead of Saturn 1 megapool launch.
author: Dr Doofus (@DrDoofus-MD-PhD-DDS)
discussions-to: https://dao.rocketpool.net/t/increase-deposit-pool-max-sentiment-poll/3865
status: Final
type: Protocol
category: Core
created: 2026-01-06
requires: RPIP-33
vote-to:
vote-date:
vote-result:
tags: [deposit-pool-max, saturn-1, megapools]
---

## Abstract

This RPIP proposes increasing the Deposit Pool maximum balance from 18,000 ETH to 6,000,000 ETH (nearly 20% of staked ETH at the time this was written) by updating the on-chain parameter:

`deposit.pool.maximum`

Note that the specific value is effectively uncapping the pool and is meant primarily for signaling.

## Motivation

There are three primary motivations for this change:

- **Saturn Readiness**
  With the planned Saturn 1 launch and introduction of megapools (targeted for February 9), larger inflows are possible and, in fact, have already been arriving. Increasing the Deposit Pool cap allows ETH to accumulate in advance, supporting smoother megapool onboarding.

- **Avoiding RPIP-74 Threshold Effects**
  The Deposit Pool could potentially cross the 80% utilization threshold defined in RPIP-74, which would trigger re-enabling of the minipool queue. This was fine when launch was months down the road, but is not needed weeks before launch.

- **Avoiding Rejection of Large Deposits**
  If the Deposit Pool reaches its maximum capacity while no minipools are available to absorb additional ETH, the protocol may be forced to reject new deposits. This could discourage institutional or large-scale participants and negatively impact Rocket Pool’s reputation at a critical growth phase.

## Specification

### Scope

The following parameter SHALL be updated via onchain governance:

- **Parameter**: `deposit.pool.maximum` (uint256)
- **Current value**: `18000000000000000000000`
- **Proposed value**: `6000000000000000000000000`

By delivering the payload:
`proposalSettingUint(
  "rocketDAOProtocolSettingsDeposit",
  "deposit.pool.maximum",
  6000000000000000000000000
)`

No other parameters or behaviors are modified by this RPIP.

## Rationale

- This is a capacity only change--it simply allows more ETH if rETH demand exists.
- The change is fully reversible via future governance if conditions change.

## Backwards Compatibility

This change is fully backwards compatible.

## Security and Other Considerations

The maximum for the Deposit Pool primarily exists to limit drag on rETH yield. A large amount of ETH in the pool means a lot of rETH has been minted that gets rewards, but the corresponding ETH is not matched to a validator that is earning rewards. Thus, the yield is reduced.

This is contrary to the protocol’s usual goals, however, in this case:

- The build-up is temporary and potentially advantageous to Saturn 1 and megapool launch
- Capital efficiency is greater for megapools, so much so that we do not expect to be Node Operator limited for a very long time

The core team and pDAO should monitor the yield and the Deposit Pool and propose to decrease the maximum via onchain vote if the situation requires it.

## References

- RPIP-33: Governance-gated parameter changes
- RPIP-74: Minipool queue activation based on Deposit Pool utilization

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
