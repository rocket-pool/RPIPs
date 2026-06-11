---
rpip:
title: On-Chain Signal Voting
description: Add support for signaling votes to the on-chain pDAO voting system.
author: knoshua (@knoshua)
discussions-to: <URL>
status: Draft
type: Protocol
category (*only required for Protocol ): Core
created: 2025-06-03
---

## Abstract

This proposal adds votes without an executable payload and with arbitrary vote choices to the existing on-chain governance system. This lays the foundation for future migration of pDAO signaling votes from Snapshot. 

## Specification

This specification introduces the following pDAO protocol parameters:

| Name                    | Type | Initial Value | Guardrail<br> |
| ----------------------- | ---- | ------------- | ------------- |
| `signaling_phase1_time` | days | 7             | >= 1          |
| `signaling_phase2_time` | days | 7             | >= 1          |



- The protocol SHALL allow any node to propose a signaling vote by locking `proposal.bond` RPL. `proposeSignaling` SHALL create a pDAO governance proposal as defined in [RPIP-33](RPIP-33.md), with the following parameters:
	- `proposalMessage`: A string explaining what the proposal is about
	- `blockNumber`: The block number the proposal is being made for
	- `treeNodes`: A merkle pollard generated at `blockNumber` for the voting power state of the DAO
- There SHALL be a `voteSignaling` method that allows voting on signaling proposals with delegated vote power for `signaling_phase1_time` days after the proposal's `blockNumber`. It SHALL  emit an event with `voteChoice` and have the following parameters:
	- `proposalID`: ID of the proposal to vote on
	- `voteChoice`: calldata encoding the vote
	- `veto`: whether the voter wants to veto the proposal or not
	- `votingPower`: Total delegated voting power for the voter at the proposal block
	- `nodeIndex`:  The index of the node voting
	- `witness`: A merkle proof into the network voting power tree proving the supplied voting power is correct
- There SHALL be an `overrideVoteSignaling` method that allows voting on signaling proposals with a node's vote power for `signaling_phase2_time` days, starting `signaling_phase1_time` days after the proposal's `blockNumber`. It SHALL  emit an event with `voteChoice` and have the following parameters:
	- `proposalID`: ID of the proposal to vote on
	- `voteChoice`: calldata encoding the vote
	- `veto`: whether the voter wants to veto the proposal or not
- If `proposal.veto.quorum` percent of the vote power voted to veto a proposal, the proposal bond of the proposer SHALL be burned.

## Rationale

New proposal, vote, and override vote methods are introduced to clearly separate signaling votes from votes that contain an executable payload.

Using calldata for the vote supports multiple voting types, including weighted voting (used for Management Committee elections in the past), ranked-choice voting (used for RPIP-62), and approval voting (used for RPIP-8). 

However, this proposal only establishes the on-chain foundation for such signaling votes and the pDAO will need a frontend to enable voting and tallying of vote outcomes.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
