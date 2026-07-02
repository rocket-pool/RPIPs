---
rpip:
title: On-Chain Signal Voting
description: Add support for signaling votes to the on-chain pDAO voting system.
author: knoshua (@knoshua)
discussions-to: https://dao.rocketpool.net/t/rpip-on-chain-signal-voting/3988
status: Draft
type: Protocol
category (*only required for Protocol ): Core
created: 2026-06-03
---

## Abstract

This proposal adds votes without an executable payload and with arbitrary vote choices to the existing on-chain governance system. This lays the foundation for future migration of pDAO signaling votes from Snapshot.

## Motivation

In 2025, Snapshot introduced Snapshot Pro, which costs $600/month and gives access to premium features. Included in that is access to the voting strategy Rocket Pool uses, because the delegation with override logic is computation-heavy.

This RPIP provides the on-chain basis for a signal voting system that can remove the Snapshot dependency in the future.
## Specification

This specification introduces the following pDAO protocol parameters:

| Name                    | Type | Initial Value | Guardrail     |
| ----------------------- | ---- | ------------- | ------------- |
| `signaling_phase1_time` | days | 7             | >= 1          |
| `signaling_phase2_time` | days | 7             | >= 1          |



- The protocol SHALL allow any node to propose a signaling vote by locking `proposal.bond` RPL. `proposeSignaling` SHALL create a pDAO governance proposal with the following parameters:
	- `proposalMessage`: A string explaining what the proposal is about
	- `blockNumber`: The block number the proposal is being made for
	- `choices`: calldata encoding the available choices and vote format
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

Using calldata for the proposal and vote supports multiple voting types, including weighted voting (used for Management Committee elections in the past), ranked-choice voting (used for RPIP-62), and approval voting (used for RPIP-8). 

However, this proposal only establishes the on-chain foundation for such signaling votes, and the pDAO will need a frontend to enable voting and tallying of vote outcomes.

We also looked at how the Snapshot voting strategy could be adjusted to avoid the need for Snapshot Pro. Delegation without override appears to be sufficient for that. But we have no guarantee that Snapshot won't change the rules in the future, and some view the ability to override as fundamental to our governance.

## Security Considerations

In contrast to the on-chain voting system for parameter changes and treasury spending introduced in [RPIP-33](RPIP-33.md), the signal voting system proposed here has no challenge period between a proposal and the start of voting that would allow for defeating proposals that submit an incorrect merkle tree for voting power. This is a conscious choice that allows for a faster voting process. The expectation is that it would fall to the voting frontend to verify that the used voting power merkle tree is correct and to filter out any votes that use incorrect vote power.

The ability to veto proposals (that use correct vote power and would appear on the voting frontend) and burn the proposer's bond discourages spamming the frontend. While proposals with incorrect vote power may not be vetoable, they would do nothing, since the votes have no executable payload and wouldn't appear on the frontend.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
