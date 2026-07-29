---
rpip: 84
title: pDAO Signaling Governance
description: Establish a platform independent signaling process, adopt RocketDash, and provide an emergency method for changing signaling platforms.
author: Dr Doofus (@DrDoofus-MD-PhD-DDS)
discussions-to: TBD
status: Draft
type: Meta
created: 2026-07-18
requires: RPIP-4, RPIP-33
---

## Abstract

This RPIP establishes a platform independent framework for pDAO signaling votes and designates RocketDash as the initial Recognized Signaling Platform. The Recognized Signaling Platform is the system designated to conduct pDAO votes that are not conducted on-chain.

It also creates an on-chain Emergency Ballot System (EBS) for changing the Recognized Signaling Platform when the existing platform is unavailable or no longer trusted and establishes a public registry of authorized vote publishers.

## Motivation

[RPIP-4](RPIP-4.md) established Snapshot as the platform for Rocket Pool governance votes. Snapshot now costs approximately $600 per month. Rocket Pool currently manages this expense by activating and deactivating the service around scheduled votes.

The long term intention is for the on-chain pDAO system to support additional forms of governance. Until that work is complete, RocketDash provides a community developed alternative for conducting signaling votes.

Existing governance documents identify Snapshot directly rather than defining a replaceable signaling platform. This creates a continuity problem if Snapshot, RocketDash, or another future platform becomes unavailable or untrustworthy.

RPIP-4 also permits Darren Langley or a party he entrusts to publish votes, but there is no maintained public record of who currently holds that authority.

## Specification

### Recognized Signaling Platform

* The **Recognized Signaling Platform** SHALL be the platform used for pDAO signaling votes under [RPIP-4](RPIP-4.md).
* RocketDash at `https://rocketdash.net/vote` SHALL be the initial Recognized Signaling Platform under this RPIP.
* References to Snapshot voting in existing RPIPs SHALL refer to the Recognized Signaling Platform unless the reference is specifically historical or technical to Snapshot.
* The current and historical Recognized Signaling Platforms SHALL be recorded in this RPIP.

### Vote Administration

* The initial **Lead Vote Administrator** under this RPIP SHALL be Darren Langley (`langers`).
* The Lead Vote Administrator MAY appoint or remove **Authorized Vote Publishers** without a pDAO vote.
* Appointments and removals MUST be publicly announced and recorded in the Vote Publisher History.
* Only the Lead Vote Administrator and current Authorized Vote Publishers MAY publish official votes on the Recognized Signaling Platform.

### Changing the Recognized Signaling Platform

* The pDAO SHALL have the ability to change the Recognized Signaling Platform through a vote conducted on the current Recognized Signaling Platform.
* A successful platform change vote MAY also appoint a Lead Vote Administrator or Authorized Vote Publishers for the replacement platform.
* Following a successful vote, the current platform and historical record in this RPIP SHALL be updated without an additional vote.

### Emergency Ballot System

* [RPIP-33](RPIP-33.md) SHALL recognize an **Emergency Signaling Platform Change** as an allowed on-chain proposal type.
* Any node eligible to create an on-chain pDAO proposal under RPIP-33 MAY create an Emergency Ballot System proposal.
* An Emergency Ballot System proposal MAY only:

  * change the Recognized Signaling Platform; and
  * appoint the vote administrators needed to operate the replacement platform.
* The proposal message MUST plainly identify:

  * the Recognized Signaling Platform to be replaced;
  * the proposed Recognized Signaling Platform;
  * its canonical URL or other unique identifier;
  * any proposed changes to vote administration;
  * related public discussion on the forum.
* Emergency Signaling Platform Change proposals:
  * SHALL require a forum sentiment poll, but
  * SHALL NOT require a separate signaling vote.
* The proposal SHALL use a standardized no-operation payload that changes no protocol parameter, spends no treasury funds, and performs no Security Council action.
* Tooling (preferably Smart node) SHALL provide a method to create an Emergency Signaling Platform Change proposal with the required message.
* The proposal SHALL use the existing RPIP-33 voting process and passing criteria.
* An Emergency Signalling Platform Change proposal SHALL be considered passed when, at the end of the RPIP-33 voting process, it is not vetoed, has met quorum, has more voting power For than Against, and the platform to be replaced matches the Recognized Signaling Platform.
* A passing proposal SHALL take governance effect at the end of voting. Later execution or expiration of its no-operation payload SHALL NOT alter that effect.
* If multiple Emergency Signaling Platform Change proposals become successful in the same block, they SHALL be applied in ascending proposal ID order.
* Following a successful platform change, this RPIP SHALL be updated without an additional vote.

### Updating This RPIP

This is a Living RPIP.

Substantive changes to the signaling process, platform change process, Emergency Ballot System, or vote administration authority MUST be approved by pDAO vote.

The following changes are ministerial and do not require an additional vote:

* recording a platform change already approved by the pDAO;
* recording a change to the Lead Vote Administrator already approved by pDAO vote;
* recording the appointment or removal of an Authorized Vote Publisher;
* adding an end date to a historical entry;
* correcting platform URLs, account identifiers, or other non-substantive administrative information.

## Current Signaling Platform

| Platform   | Canonical location                       |            Effective date | Authorization |
| ---------- | ---------------------------------------- | ------------------------: | ------------- |
| RocketDash | `https://rocketdash.net/vote`  | Upon passage of this RPIP | RPIP-TBD      |

## Signaling Platform History

| Platform   |            Effective date |                  End date | Authorization |
| ---------- | ------------------------: | ------------------------: | ------------- |
| Snapshot   |                2022-09-06 | Upon passage of this RPIP | RPIP-4        |
| RocketDash | Upon passage of this RPIP |                         — | RPIP-TBD      |

## Vote Publisher History

| Role                                                  | Person                     | Platform identity      | Effective date | End date | Authority |
| ----------------------------------------------------- | -------------------------- | ---------------------- | -------------: | -------: | --------- |
| Lead Vote Administrator and Authorized Vote Publisher | Darren Langley (`langers`) | `[ACCOUNT OR ADDRESS]` |     2022-09-06 |        — | RPIP-4    |

## Rationale

### Platform independent governance

RPIP-4 directly identifies Snapshot as the platform used for governance votes. This RPIP instead defines the Recognized Signaling Platform as a replaceable component of the governance process.

This permits Rocket Pool to adopt RocketDash now without assuming that RocketDash, Snapshot, or any other platform will remain the permanent solution.

Existing RPIP-4 requirements remain in effect except where this RPIP expressly modifies them.

### RocketDash

RocketDash provides substantially the same governance function currently provided by Snapshot while avoiding Snapshot’s recurring subscription cost.

RocketDash is intended as an interim system while broader on-chain governance functionality is developed. However, this RPIP does not require another migration on a predetermined schedule. RocketDash may remain the Recognized Signaling Platform for as long as the pDAO considers it suitable.

### Normal platform changes

Under ordinary circumstances, the current Recognized Signaling Platform should be used to approve its replacement.

For example, RocketDash could conduct a vote to return to Snapshot, move to another signaling platform, or transfer signaling votes to a future version of the on-chain pDAO system.

The platform table is updated after that decision. Updating the table records the vote result and is not a second governance decision.

### Emergency Ballot System

A signaling system cannot be relied upon to authorize its own replacement after it has become unavailable or untrustworthy. The Emergency Ballot System provides an independent fallback using the existing on-chain pDAO. The EBS is only to be used for signaling platform changes and only when the community has agreed via discussion and sentiment poll that the existing system is no longer trustworthy or useable. 

An Emergency Ballot System proposal is intended to be readable in the same manner as any other proposal. The proposal message, rather than an encoded calldata value, identifies the proposed platform.

For example:

```text
EBS: Replace RocketDash with Snapshot
Current platform: RocketDash
Proposed platform: Snapshot
Canonical location: https://snapshot.box/#/s:rocketpool-dao.eth
Lead Vote Administrator: Darren Langley
Discussion: [FORUM OR RPIP LINK]
```

Voters would vote For, Against, Abstain, or Veto on that statement.

The associated calldata would not contain coded messages or lookups to another website. Every Emergency Ballot System proposal would use the same standardized payload. Its only purpose is to allow the existing on-chain proposal lifecycle to complete without changing protocol settings or moving funds.

Proposers should not need to construct this calldata manually. Smart node may implement the EBS workflow in any manner that reliably produces the required message and standardized payload, e.g., via a `rocketpool pdao propose emergency-signaling-platform-change` command that simply takes a proposal message.

The platform change takes effect when the proposal becomes successful under the RPIP-33 voting process, rather than when its no-operation payload is executed. This prevents anyone from changing the order of overlapping EBS results by choosing when to execute them.

Requiring both the platform to be replaced and its replacement allows a later proposal to correct an earlier one without waiting for the earlier payload to be executed. A later proposal naming an outdated current platform has no effect. Proposal ID ordering resolves results that become successful in the same block.

### Scope of the Emergency Ballot System

The Emergency Ballot System is limited to signaling platform continuity. It cannot pass an ordinary RPIP, change a protocol parameter, spend treasury funds, or modify the Security Council.

It may identify the initial vote administrators for the replacement platform because a platform change would not be operational if nobody were authorized to publish votes on it.

Any eligible node may raise an Emergency Ballot System proposal under the existing RPIP-33 process. The proposal bond, challenge process, Against votes, and Veto votes provide protections against spam, malformed proposals, and malicious use.

### Vote administration

RPIP-4 currently gives vote publication authority to Darren Langley and parties he entrusts.

Requiring a separate pDAO vote for each delegated publisher would make routine administration unnecessarily difficult and would be more restrictive than the authority already provided by RPIP-4.

This RPIP preserves the Lead Vote Administrator’s ability to appoint trusted publishers while requiring appointments to be publicly recorded. The registry provides transparency about who may publish official ballots and preserves a historical record when those permissions change.

## Backwards Compatibility

Existing RPIP-4 voting rules remain in effect. References in existing RPIPs to Snapshot as the official venue for pDAO signaling votes SHALL instead refer to the Recognized Signaling Platform. Historical references, literal Snapshot URLs, and technical details specific to Snapshot remain unchanged and SHALL NOT be interpreted as requirements for RocketDash or a future Recognized Signaling Platform.

## Security Considerations

### Signaling platform integrity

The Recognized Signaling Platform must correctly apply pDAO voting power, delegation, eligibility, quorum, and outcome rules.

A platform should not be adopted unless its ballots and results can be verified to a degree considered acceptable by the pDAO and consistent with RPIP-4.

### Emergency proposal clarity

An Emergency Ballot System proposal must unambiguously identify one proposed replacement platform. It should not present multiple platform choices because the on-chain system uses For, Against, Abstain, and Veto rather than a multi-choice ballot.

The proposal should identify the platform by both name and canonical location to reduce ambiguity.

### Conflicting emergency proposals

More than one Emergency Ballot System proposal may be active at the same time. Voters should verify both the platform to be replaced and the proposed replacement, particularly when considering a contingent corrective proposal. For example, if somehow an emergency proposal changing from "A" to "B" is deemed inappropriate by the community, the subsequent proposal can be submitted from "B" to "C". 

### Vote publisher authority

Authorized Vote Publishers can advance or withhold official signaling votes. Appointments should therefore be limited to trusted parties and recorded promptly.

Appointment as an Authorized Vote Publisher does not authorize modification of approved proposal text, voting rules, quorum, or vote duration.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
