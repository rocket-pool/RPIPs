---
rpip: 80
title: Exit Requests, Triggering Exits, and Minipool Penalties
description: Specifies the underlying exit mechanism and minipool penalty system used by RPIP-71 and RPIP-73.
author: knoshua (@knoshua)
discussions-to: <URL>
status: Draft
type: Protocol
category: Core
created: 2026-05-07
requires (*optional): 44
---

## Abstract

This proposal introduces EL-triggered exits for minipools and extends how they are used for megapools. It also specifies a layered "request then enforce" exit flow  and ensures that the total amount of ETH requested to exit is tracked. Lastly, it applies penalties to minipools whose delegate prevents protocol-triggered exits.

## Motivation

Both RPIP-71 (rETH withdrawal liquidity) and RPIP-73 (rETH protection from underperforming nodes) specify when and which validators to exit for their respective goal. This RPIP seeks to provide a unified approach for how to execute those exits.
A cooperative exit phase minimizes gas spent and allows for SaaS providers to execute exits for their customers. Tracking the total requested ETH ensures that exits for withdrawal liquidity aren't excessive.

Minipools are complicated by opt-in delegates: operators can choose whether to upgrade to a new delegate or remain on older delegates indefinitely. The design therefore cannot assume that all minipools support triggered exits. A penalty in lieu of a triggered exit for minipools on old delegates aims to align node operator incentives with the protocol.

## Specification

This specification introduces the following pDAO protocol parameters:

| Name                     | Type  | Initial Value | Guardrail<br> |
| ------------------------ | ----- | ------------- | ------------- |
| `cooperative_exit_phase` | Hours | 72            |               |
| `no_exit_penalty`        | ETH   | 0.1           |               |
| `no_exit_cooldown`       | Days  | 28            | > 7           |

### Delegates Incorporating EIP-7002

- A new minipool delegate SHALL expose an interface that allows initiating an execution-layer-triggered exit under the conditions defined in this specification.
- The exit functionality of megapools introduced in RPIP-44 SHALL be extended to allow initiating an execution-layer-triggered exit under the conditions defined in this specification.

### Tracking Requested ETH

- The protocol SHALL expose a variable `requested_eth` that tracks ETH expected to exit under this specification. 
- For the purpose of this specification expected user capital is defined as `32 ETH - validator bond`.

### Exit Requests

- The protocol SHALL be able to request a validator to exit for underperformance (RPIP-73) or withdrawal liquidity (RPIP-71). The protocol MAY provide an interface for future protocol contracts to request exits.
- When a validator is requested to exit, the protocol MUST expose an “exit requested” signal that is observable to the node operator and `requested_eth` SHALL be increased by the expected user capital of that validator.
- After `no_exit_cooldown` has passed since a request, the validator SHALL no longer be considered requested to exit and can be requested to exit again.
- Requesting a validator that is already considered requested to exit SHALL not reset the window or add to `requested_eth`.

### Triggering Exits

- If a validator's delegate supports EL-triggered exits and it has been requested to exit for at least `cooperative_exit_phase`, the protocol SHALL allow anyone to trigger an exit.
- If such an exit is triggered, `requested_eth` SHALL be decreased by the expected user capital of that validator.

### Penalizing Minipools 

- If a minipools's delegate does not support triggered exits and it has been requested to exit for at least `cooperative_exit_phase`, the protocol SHALL allow anyone to prove that the validator has not exited (beacon state proof: `exit_epoch = FAR_FUTURE_EPOCH`).
- If such a proof is provided, the penalty of the minipool SHALL be increased by `no_exit_penalty` and `requested_eth` SHALL be decreased by the expected user capital of that validator.
- Another such proof and penalty SHALL only be possible once `no_exit_cooldown` has passed and the validator was requested to exit another time.

## Rationale

### Handling of Minipools

Since minipool delegates are opt-in, it is unrealistic to assume that all minipools will support EIP-7002 triggered exits in a timely manner and any design must handle a heterogeneous validator set that includes megapools, exit-capable minipools, and non-exit-capable minipools. Options include:
1. Completely ignoring all minipools.
2. Restrict exits to only upgraded minipool delegates.
3. Try to include all minipools for exits and penalize those that refuse to exit.

Rocket Pool is in transition from minipools to megapools as the primary abstraction for new validators. Minipool deposits are already disabled and RPL tokenomics and commission policy are centered around megapools. As long as a large fraction of the backing set remains in minipools, protocol-level control over commission is constrained and every protocol change must account for both architectures, slowing down protocol development. 
The first option therefore appears counterproductive: directly, by reducing the number of megapool validators and indirectly, by making staying on minipools more attractive. 
The concern with the second option is that it would incentivize operators to deliberately avoid upgrading and potentially seek to disable the use-latest-delegate flag again in order to evade exits. This could lead to a very small set of minipools that support EL-triggered exits, which then would inherit the problems from the first option.
That's why the proposal uses the third option. A penalty for failing to exit is meant to provide an economic incentive for protocol-aligned behavior: upgrading to the new delegate or responding to exit requests.

### Limiting Minipool Penalty Frequency 

A validator that was requested to exit  in the last `no_exit_cooldown` days cannot be requested to exit again. This is relevant for the case of minipools that can't be exited and limits the frequency with which penalties can be applied, giving the node operator an opportunity to upgrade instead of quickly ramping up the penalty.

### Penalty Sizing

The specification uses an absolute penalty value, meaning that 16 ETH and 8 ETH minipools receive the same penalty per failed exit. Arguments could be made for scaling either way. If we wanted to ensure that both cases experience the same loss of APR, the penalty for 16 ETH should be scaled up. From the protocol's point of view, the 8 ETH minipool failing to exit 24 ETH of user capital is more damaging than the 16 ETH case and arguably should be penalized more. 


## Security Considerations [TODO]

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
