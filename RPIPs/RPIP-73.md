---
rpip: 73
title: rETH Protection From Underperforming Nodes
description: Force exit significantly underperforming validators to protect rETH from yield loss.
author: knoshua (@knoshua)
discussions-to: <URL> TODO
status: Draft
type: Protocol
category: Core
created: 2025-07-30
requires (*optional): 44
---


## Abstract

This proposal introduces a permissionless exit mechanism for significantly underpforming minipools and megapool validators and applies penalties to minipools whose delegate prevents protocol-triggered exits.
## Motivation

Rocket Pool node operators have been expected to perform well, since underperformance results in lower rewards. Despite this existing performance incentive, some node operators significantly underperform, with validators offline for extended periods of time (for example, abandoned validators). This reduces the relative value of rETH, as yield losses are socialized across all rETH holders.

At the same time, Rocket Pool is in transition from minipools to megapools as the primary abstraction for new validators. Minipool deposits are already disabled and RPL tokenomics and commission policy are centered around megapools. As long as a large fraction of the backing set remains in minipools, protocol-level control over commission is constrained and every protocol change must account for both architectures.

Minipools are additionally complicated by opt-in delegates: operators can choose whether to upgrade to a new delegate or remain on older delegates indefinitely. The protocol therefore cannot assume that all minipools support EIP-7002 exits, and any design must handle a heterogeneous validator set that includes megapools, exit-capable minipools, and non-exit-capable minipools.

## Specification

This specification introduces the following pDAO protocol parameters:

| Name                           | Type   | Initial Value     | Guardrail<br> |
| ------------------------------ | ------ | ----------------- | ------------- |
| `performance_period`           | Epochs | 22016 (~100 days) |               |
| `performance_threshold`        | pct    | 94                |               |
| `performance_challenge_period` | Hours  | 24                |               |
| `performance_challenge_bond`   | RPL    | 100               | > 20          |

This specification uses the following pDAO protocol parameters introduced in RPIP-71:

| Name                             | Type  | Initial Value | Guardrail<br> |
| -------------------------------- | ----- | ------------- | ------------- |
| `no_exit_penalty`                | ETH   | 0.1           |               |
| `no_exit_cooldown`               | Days  | 28            | > 7           |

### Delegates Incorporating EIP-7002

- New minipool and megapool delegates MUST expose an interface that allows Rocket Pool network contracts to initiate an execution-layer-triggered exit in accordance with EIP-7002.

### Performance Challenge Mechanism

- The protocol SHALL allow anyone to propose a validator exit by providing a `start_epoch` and `performance_period * (1 - performance_treshold)`epochs within `[start_epoch, start_epoch + performance_period]` and locking `performance_challenge_bond` for `performance_challenge_period`. 
- If a validator received a `no_exit_penalty` within `no_exit_cooldown`, proposing a validator exit SHALL not be possible.
- The protocol SHALL allow anyone to defeat a proposed exit by proving that for one epoch in the challenge, the `previous_epoch_participation` in the Beacon State shows a timely **target** attestation. The person defeating the challenge SHALL be awarded the `performance_challenge_bond`. 
- If a proposed exit is not defeated within `performance_challenge_period`, the protocol SHALL allow anyone to trigger an exit if the validator delegate supports it or apply a `no_exit_penalty` otherwise.


## Rationale

### Permissionless Exit Mechanism

This specification prioritizes a precisely defined and deterministic metric that can be verified on-chain to determine which validators to exit. Elements of subjectivity like committees or off-chain analysis are avoided.
It uses the **target** timeliness flag as a proxy for attestation performance, which itself is only a proxy for overall performance. Sync committees and block proposals are not factored in at all and account for ~15% of rewards (ignoring MEV).

### Offline Exits Implicit

Earlier discussions considered having a secondary exit criterion for validators being offline a certain amount of time. Since being offline implies target vote not being timely, we would already be exiting people that are offline for 6% over a ~100 day window. A secondary criterion would only be necessary if we wanted to be stricter than that.

### Timeliness Flag Choice

Ethereum attestations consist of:
	- voting for a source checkpoint
	- voting for a target checkpoint
	- voting for a chain head block
Rewards and penalties are based on correctness and timeliness of these votes. A full explanation of attestation rewards is available at [ETH2book](https://eth2book.info/capella/part2/incentives/rewards/).

The Beacon State gives us access to the timeliness flags for each validator and each epoch, so in theory we could construct a challenge mechanism that perfectly maps to attestation performance. Using just one of the timeliness flags instead is simpler to implement and makes exiting validators cheaper.

After doing some [empirical analysis](https://badperformers.streamlit.app/) it appears that both target timeliness and source timeliness are quite close matches to actual attestation performance. Target timeliness gives a little bit more leeway to people being offline. The initial threshold of 94% aims to exit people below 90% effectiveness with 95% sensitivity.
### Minipool Penalties
see RPIP-71 Rationale
