---
rpip: <TBD>
title: Minipool Queue Closure Pre-Saturn 1
description: Close the minipool queue before Saturn 1 in order to direct more ETH deposits to megapools at launch.
author: Dr Doofus (@DrDoofus-MD-PhD-DDS)
discussions-to:
status: Draft
type: Protocol
category: Core
created: 2025-09-09
requires:
vote-to:
vote-date:
vote-result:
tags:
---

## Abstract

This RPIP proposes closing the minipool queue ahead of the Saturn 1 upgrade to ensure protocol resources are directed toward megapools, avoid allowing uninformed node operators to create less desirable minipools too close to Saturn 1 launch, and improve the health of the rETH peg. The closure would occur approximately **Oct 15, 2025** (about two weeks prior to Saturn 1’s scheduled launch on **Oct 29, 2025**). The RPIP also defines communication requests and re-enable conditions in case of exceptional deposit pool demand.

## Motivation

**Alignment with Saturn 1:** After Saturn 1, node operators will only launch validators via megapools. Allowing new minipool entries up to launch consumes ETH that should instead support megapools.

**Protecting uninformed node operators:** Users wishing to use Rocket Pool to start validators, despite the protocol's best effort, might not be aware of the impending switch to megapools. The minipool queue cannot be exited. Not allowing them late admission to the queue is likely in their best interests.

**rETH peg health:** Active minipool queues consume deposit pool ETH that could otherwise support redemptions, worsening the rETH discount.

**Simplified UX:** Fewer active queues near Saturn launch reduces confusion and helps newcomers understand their options.

## Specification

### Closure Date

- The minipool queue SHALL be closed on **October 15, 2025** (14 days before Saturn 1 launch), or to the best estimate of 14 days if the launch date changes. A small deviation from that date is allowed for convenience of implementation.

### Scope

- **New minipool deposits**: Disabled at both contract and frontend levels.
- **Existing minipool queue entries**: Continue processing under current rules.

### Re-Enable Conditions

- If the **Deposit Pool utilization ≥ 80%**, the Security Council SHALL re-enable minipool deposits until utilization falls below this threshold. This safeguard ensures Rocket Pool can accept large ETH inflows without unnecessary governance delays.

### User Communications

- The Core Team and the pDAO SHOULD explain, in as targeted a way as possible, that those coming to create minipools have other options and are not just turned away:

  - **Frontend**: Prominent display explaining: _“The Minipool Queue is closed in anticipation of Saturn 1 (launching Oct 29, 2025). Learn more about Megapools and rETH here.”_
  - **Documentation updates**: Minipool docs updated with closure date, rationale, and Saturn 1 resources.
  - **Community comms**: Forum post, Discord announcement. Messaging should frame closure as an **exclusive / transitional event**, not a rejection of new participants.

## Rationale

Closing the Minipool Queue ahead of Saturn 1 balances three priorities:

1. **Preserve deposit pool ETH for Megapools**.
2. **Protect new NOs from starting new minipools**.
3. **Strengthen the rETH peg by freeing ETH for redemptions**.

The chosen date balances governance lead time with user notice. Automatic re-enable safeguards ensure the protocol can still absorb large ETH inflows if required.

## Backward Compatibility

- Active minipools are unaffected.
- Existing minipool queue entries will continue until matched.
- No change to megapool functionality.

## Security Considerations

As of now, the security council is made up only of the core team. This would give them ability to re-open the queue without pDAO intervention.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
