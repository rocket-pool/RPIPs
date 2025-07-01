---
rpip: 73
title: Rewards Tree v11 (Saturn-compatible)
description: This proposal standardizes the Rewards Tree generation process for Saturn 1 and beyond.
author: Jacob Shufro (@jshufro)
status: Draft
type: Protocol
created: 2025-06-30
requires: 62, 46, 72
vote-to:
vote-date:
vote-result:
---

## Abstract

This proposal establishes the Rewards Tree generation process required for Saturn 1. With the addition of UARS in [RPIP-46](RPIP-46.md) and [RPIP-72](RPIP-72.md), the protocol needs a way to distribute ETH to megapools with RPL staked (`voter_share`), to the pDAO (`pdao share` described in RPIP-72), and to continue to distribute eth to nodes opted in to the Smoothing Pool.

The following changes will be made to the smart contracts:

1) The megapool delegate will, at the time of distribution, calculate the operator share of the rewards adjusted for operator commission (`node_operator_commission_share + node_operator_commission_share_council_adder`) and send it to the operator's primary withdrawal address, as usual. The remainder shall be sent to the Smoothing Pool, and a global counter called `protocolShare` shall be incremented by the amount.

2) The `RewardSubmission` struct shall be extended to include a `uint256`-typed field specifying the `pdaoShare`, the amount of ETH sent to the pDAO per RPIP-72's `pdao_share`.

3) The `RewardSubmission` struct shall be extended to include a `uint256`-typed field specifying the `voterShare`, the amount of ETH sent to megapool operators with vote-eligible staked RPL, per RPIP-46's `voter_share` calculations.

4) `merkleTreeCID` will be removed or marked obsolete.

5) Upon rewards snapshot submission consensus and execution, the `protocolShare` counter shall be decreased by an amount equal to `pdaoShare + voterShare`.

Additionally, the following changes will be made to the Rewards Tree generation process with respect to ETH rewards:

1) Attestation inclusion eligibility will be reworked to include attestations that are valid within the fork they are assigned. This serves to better align Smoothing Pool performance to the underlying protocol.

2) When iterating the nodes that are members of the smoothing pool, each minipool will receive attestation scores in the same manner as v8/v9/v10.

3) When iterating the nodes that are members of the smoothing pool, each megapool will receive attestation scores based on the fixed commission defined in RPIP-46 and RPIP-72. The node's score will be incremented to reflect `node_operator_commission_share + node_operator_commission_share_council_adder`, and a global counter will track the scores assigned to `protocolShare`, in the same way that `reth_share` is tracked. Scores will be scaled linearly to normalize for balances higher than 32 eth, such that the scores for a megapool with 64 total eth balance shall be twice the score for a minipool.

4) The final scores will be used to determine how much of the non-protocol-share ETH (`balance - protocolShare`) goes to rETH, to node operators, and to UARS.

5) The amount of ETH that goes to UARS will be added with `protocolShare` before being divided proportionately between `pdao_share` and `voter_share`.

6) Each megapool will receive an amount of `voter_share` according to its vote-eligible RPL proportionate to the total vote-eligible RPL staked across all megapools, whether or not it is a member of the smoothing pool, and regardless of its performance. This calculation will be done over the chain state at the end of the interval.

7) `pdao_share` will be sent to \[TBD\]

Additionally, the following changes will be made to the Rewards Tree generation process with respect to RPL rewards:

1) All staked RPL shall be considered, whether `legacy` or `megapool staked RPL` as defined in [RPIP-43](RPIP-43.md)

## Specification

\[TBD\]

## Rationale

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).