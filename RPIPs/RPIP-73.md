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
requires (*optional): 80
---


## Abstract

This proposal introduces a permissionless on-chain challenge mechanism to exit significantly underperforming minipools and megapool validators. Target timeliness flags are used to approximate attestation performance.

## Motivation

Rocket Pool node operators have been expected to perform well, since underperformance results in lower rewards. Despite this existing performance incentive, some node operators significantly underperform, with validators offline for extended periods of time (for example, abandoned validators). This reduces the relative value of rETH, as yield losses are socialized across all rETH holders.

This proposal chooses an approximation that allows an objective on-chain implementation over subjective off-chain solutions.

## Specification

This specification introduces the following pDAO protocol parameters:

| Name                           | Type   | Initial Value     | Guardrail<br> |
| ------------------------------ | ------ | ----------------- | ------------- |
| `performance_exits_enabled`*   | bool   | `true`            |               |
| `performance_period`           | Epochs | 22016 (~100 days) |               |
| `performance_threshold`        | pct    | 94                |               |
| `performance_challenge_period` | Hours  | 24                |               |
| `performance_challenge_bond`   | RPL    | 100               | > 20          |

A * designates this parameter as being modifiable by the Security Council without a delay.

### Performance Challenge Mechanism

- If `performance_exits_enabled` is `true` , the protocol SHALL allow anyone to propose a validator exit by providing a `start_epoch` and `performance_period * (1 - performance_treshold)`epochs within `[start_epoch, start_epoch + performance_period]` and locking `performance_challenge_bond` for `performance_challenge_period`. 
- The protocol SHALL allow anyone to defeat a proposed exit by proving that for one epoch in the challenge, the `previous_epoch_participation` in the Beacon State shows a timely **target** attestation. The person defeating the challenge SHALL be awarded the `performance_challenge_bond`. 
- If a proposed exit is not defeated within `performance_challenge_period`, the protocol SHALL allow anyone to add the validator as "requested to exit" as defined by RPIP-<TODO: Add RPIP number>.

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

### Implementation of Epoch Call Data

To challenge a validator, a challenger needs to provide `performance_period * (1-performance_threshold)` epochs. For the initial parameters in this proposal, this means 1321 epochs. To keep gas cost for call data reasonable, a number of optimizations could be considered:
- Represent epochs as offsets from `start_epoch`. This allows using `uint16[]` for `performance_period` < 292 days, reducing gas cost by a factor of 4. For the proposed initial parameters this would translate to (non-zero) 2642 bytes, but size would increase as `performance_threshold` is lowered.
- Represent all the epochs as a bitset, with 1 representing a not-timely target attestation, resulting in 2752 bytes of call data. Since zero bytes are 1/4 of the gas, this may still be preferable to the epoch-offset approach.

The specifications intends to leave this open as an implementation detail. 

## Security Considerations

### Adverse Network Events

In case of Ethereum wide attestation degradation, many validators could become exit eligible based on the fixed criterion that was chosen based on stable network conditions. To avoid exits of validators that are generally performing fine, the security council has the ability to disable this exit mechanism by setting `performance_exits_enabled` to `false`. Exits can be re-enabled by the pDAO or the Security Council once the period of instability is no longer within `performance_period`.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
