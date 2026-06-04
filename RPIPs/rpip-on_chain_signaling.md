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

- There SHALL be a `proposeSignaling` method that creates a pDAO governance proposal as defined in [RPIP-33](RPIP-33.md), with the following parameters:
	- `_proposalMessage`: A string explaining what the proposal does
	- `_blockNumber`: The block number the proposal is being made for
	- `_treeNodes`: A merkle pollard generated at `_blockNumber` for the voting power state of the DAO
- There SHALL be a `voteSignaling` method that allows voting as defined in [RPIP-33](RPIP-33.md) on such pDAO governance proposals without a payload. It SHALL  emit an event with `_voteChoice` and have the following parameters:
	- `_proposalID`: ID of the proposal to vote on
	- `_voteChoice`: calldata encoding the vote
	- `_votingPower`: Total delegated voting power for the voter at the proposal block
	- `_nodeIndex`:  The index of the node voting
	- `_witness`: A merkle proof into the network voting power tree proving the supplied voting power is correct
- There SHALL be no delegate override functionality for signaling votes.

## Rationale

New proposal and vote methods are introduced to clearly separate signaling votes from votes that contain an executable payload.

Using calldata for the vote supports multiple vote types, including weighted voting (used for Management Comittee elections in the past), ranked choice voting (used for RPIP-62), and approval voting (used for RPIP-8). 

However, this proposal only creates the foundation for such signaling votes and it will require additional frontend work to allow voting and tallying of vote outcomes.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
