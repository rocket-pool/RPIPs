---
rpip:
title: Require Minipools to Use Latest Delegate
description: Require Smartnode update to enforce use of the latest delegate for minipools.
author: Dr Doofus (@DrDoofus-MD-PhD-DDS)
discussions-to: https://dao.rocketpool.net/t/require-minipools-to-use-latest-delegate/3844
status: Draft
type: Protocol
category: Core
created: 2026-01-05
tags: [minipools, delegate, smartnode, governance]
---

## Abstract

This proposal requires that a future specified Smartnode update force minipools to use the **latest delegate smart contract**. Once upgraded, node operators would no longer be able to permanently pin minipools to older delegate implementations. The intent is to ensure protocol-wide consistency, enable governance-approved minipool behavior changes, and remove technical blockers to future enforcement mechanisms (including, but not limited to, forced exits of persistently underperforming minipools).

## Motivation

A non-trivial number of minipools are persistently underperforming, reducing overall rETH yield and harming demand. Reduced rETH demand contributes to difficulties clearing the minipool queue and complicates the protocol’s transition to the Saturn 1 architecture, including megapools, the RPL fee switch, and alternative protocol funding mechanisms.

While support efforts to remediate underperforming node operators are ongoing, the protocol currently lacks a reliable enforcement mechanism because minipools may permanently opt out of delegate upgrades. This creates a situation where governance-approved protocol logic cannot be universally applied.

This proposal addresses that limitation.

## Background

### Minipool delegate architecture

Rocket Pool minipools are implemented using a proxy pattern. The minipool contract itself contains minimal logic and instead forwards most calls to a **delegate implementation contract** (the “delegate”).

The delegate contract controls, among other things:

- Minipool lifecycle and state transitions
- Validator launch conditions
- ETH balance distribution between node operators and rETH holders
- Penalty and slashing logic
- Withdrawal, exit, and finalization behavior
- Trusted-node and governance-authorized actions
- Bug fixes and protocol rule updates without redeploying minipools

### `use-latest-delegate` flag

Node operators may currently configure each minipool to either:

- Use the delegate active at the time of creation, then upgrade to future delegates of their choosing, or
- Use the latest protocol delegate at all times

This opt-in upgrade model was originally designed to protect node operators from malicious or unsafe upgrades. However, it also allows minipools to permanently avoid protocol changes approved by governance.

### Historical rationale for opt-in delegate upgrades

The opt-in `use-latest-delegate` model was originally designed to serve multiple purposes, including:

1. Protect node operators from potentially malicious or unsafe governance upgrades.
2. Allow minipools to converge on a long-lived (“Lindy”) delegate without mandatory churn.
3. Provide node operators an exit path if they disagreed with protocol changes.

This proposal revisits these assumptions in light of changes to Rocket Pool’s governance and security model.

## Specification

### Implementation

- A Smartnode update SHALL enforce the use of the latest delegate for minipools. The exact implementation method is left to the Smartnode development team, subject to this specification.

- The Smartnode update SHOULD be implemented at the earliest reasonable opportunity that does not interfere with the Saturn 1 launch.

- The Smartnode SHALL remove the ability to change the `use-latest-delegate` flag via Smartnode configuration UI and/or command line interface.

### Scope

- Applies **only after** upgrading to the specified Smartnode version.

- Does **not** retroactively modify minipools on older Smartnode versions.

- Does **not** itself implement forced exits; it enables future governance-approved mechanisms.

- Does **not** grant any emergency or unilateral powers to the core team or governance bodies beyond existing delegate upgrade mechanisms.

### Delegate Update Oversight

- Delegate updates that materially affect validator exits, reward distribution, penalties, or governance authority MUST only be implemented following pDAO vetting via vote.

## Rationale

Persistently underperforming minipools impose systemic costs:

- Lower rETH yield
- Reduced rETH demand
- Slower queue clearance
- Delayed or constrained Saturn 1 rollout
- Reduced protocol revenue options (e.g., RPL fee switch, institutional incentives)

Because minipools can remain indefinitely on older delegate logic, it is difficult or impossible to enforce protocol-wide standards. This limitation does not apply to megapools, which do not use the same delegate opt-in model.

This change prioritizes protocol-wide consistency and enforceability over permanent opt-out flexibility. As Rocket Pool scales and transitions to Saturn-era architecture, the inability to uniformly apply protocol logic becomes an increasing risk to rETH holders and protocol sustainability.

The delegate upgrade mechanism already exists; this proposal ensures it cannot be indefinitely bypassed.

Several original justifications for opt-in delegate upgrades have been partially mitigated over time. Saturn-era governance introduces additional safeguards, including a Security Council veto, reducing the risk of malicious upgrades. Additionally, long governance lead times and explicit votes may serve as a functional replacement for individual opt-out as an exit mechanism.

## Backwards Compatibility

- Existing minipools continue operating normally until the node operator updates Smartnode.

- No changes are applied to nodes that do not update.

- Minipools created after updating Smartnode would automatically follow the enforced delegate behavior.

- Node operators retain the ability to exit minipools prior to upgrading Smartnode, and governance processes are expected to provide sufficient notice before delegate changes that materially affect operator behavior.

## Risks and Considerations

- **Governance risk concentration:** Delegate upgrades become more powerful and require stronger review processes.

- **Loss of opt-out:** Node operators lose the ability to permanently choose specific delegates after upgrading.

- **Trust requirements:** Increased importance of transparent communication, audits, and governance safeguards around delegate changes.

- **Community alignment:** Some node operators may strongly oppose the change on philosophical grounds.

- **Imperfect enforcement**: Because this change is gated on Smartnode updates, some node operators—either due to custom setups or intentional avoidance—may remain on older Smartnode versions and continue using older delegates. This limits immediate coverage and means the proposal should be viewed as a necessary enabling step rather than a complete solution on its own.

- **Loss of Lindy delegate stability:** Forcing delegate upgrades sacrifices the ability for minipools to remain indefinitely on a long-lived delegate implementation. While this may increase upgrade churn, it is a deliberate tradeoff in favor of enforceability and protocol-wide consistency.

## Conclusion

This proposal represents a significant philosophical shift, but one driven by observed protocol limitations rather than theory. As Rocket Pool evolves, ensuring consistent, enforceable protocol behavior may be necessary to protect rETH holders and support long-term scalability.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
