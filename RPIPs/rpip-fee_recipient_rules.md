---
rpip: #<to be assigned>
title: Current Fee Recipient Rules
description: Specifies the rules for setting the fee recipient on proposed blocks and the penalties for breaking them
author: Ramana Kumar (@xrchz), Joe Clapis (@jcrtp)
discussions-to: https://github.com/rocket-pool/rocketpool-research/issues
status: Draft
type:  Informational
created: 2024-04-14
---

## Abstract
This RPIP describes the current state of the specification of Rocket Pool's fee recipient rules and proposed penalty system.
Node operators are supposed to follow these rules when setting the fee recipient (or providing a fee recipient to a MEV relay) on any blocks they propose.
A penalty system to make it costly to break the rules has been proposed but is not currently implemented.

## Motivation
Rocket Pool's expectations of node operators needs a clear specification so that node operators know what they ought to do and what to expect if they do not follow the rules.

The rules regarding fee recipients and the proposed penalty system have been developed in the external [Rocket Pool Research](https://github.com/rocket-pool/rocketpool-research/tree/master/Merkle%20Rewards%20System) repository. However, as with [RPIP-51](./RPIP-51.md), these rules are a core protocol specification whose proper home is in an RPIP where they can be easily found and referred to with a clear status as an official definition of (part of) the Rocket Pool protocol. This RPIP is an informational document describing the status quo, with the intention of following up with a future pDAO-ratified RPIP (which may or may not include changes).

## Specification
There are three parts to the fee recipient rules and penalty system, only two of which at present have extant (draft) specifications.
1. The fee recipient a Rocket Pool node operator SHOULD use when they propose a block.
2. The fee recipient a Rocket Pool node operator did use when they proposed a block.
3. The penalties applied when the actual fee recipient differs from the expected fee recipient.

Relevant drafts from the external [research repository](https://github.com/rocket-pool/rocketpool-research) are attached to this RPIP as assets:
1. [Expected Fee Recipient Calculation](../assets/rpip-fee_recipient_rules/fee-recipient-spec.md)
2. No specification for this section of the rules currently exists.
3. [Penalty System (draft)](../assets/rpip-fee_recipient_rules/penalty-system.md)

## Rationale
This specification describes the material available to Rocket Pool node operators as of April 2024.
Since the penalty system is not being used, it is purely informational.
The intention here is to capture the status quo and highlight the need for further work to fill out and implement the penalty system.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
