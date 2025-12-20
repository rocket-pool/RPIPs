---
rpip:
title: Require Minipools to Use Latest Delegate
description: Create Smartnode update that forces minipools to set use-latest-delegate flag to yes.
author: Dr Doofus (@DrDoofus-MD-PhD-DDS)
discussions-to: https://dao.rocketpool.net/t/require-minipools-to-use-latest-delegate/3844
status: Draft
type: Protocol
category: Core
created: 2025-12-19
requires:
vote-to:
vote-date:
vote-result:
tags:
---

## Abstract

This proposal requires minipools to use the **latest protocol-approved delegate smart contract** after upgrading to a specified future Smartnode version. Once upgraded, node operators would no longer be able to permanently pin minipools to older delegate implementations.

The intent is to ensure protocol-wide consistency, enable governance-approved minipool behavior changes, and remove technical blockers to future enforcement mechanisms (including, but not limited to, forced exits of persistently underperforming minipools).

---

## Motivation

A non-trivial number of minipools are persistently underperforming, reducing overall rETH yield and harming demand. Reduced rETH demand contributes to difficulties clearing the minipool queue and complicates the protocol’s transition to the Saturn 1 architecture, including megapools, the RPL fee switch, and alternative protocol funding mechanisms.

While support efforts to remediate underperforming node operators are ongoing, the protocol currently lacks a reliable enforcement mechanism because minipools may permanently opt out of delegate upgrades. This creates a situation where governance-approved protocol logic cannot be universally applied.

This proposal addresses that limitation.

---

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

- Pin to the delegate active at the time of creation, or
- Always use the latest protocol-approved delegate

This opt-in upgrade model was originally designed to protect node operators from malicious or unsafe upgrades. However, it also allows minipools to permanently avoid protocol changes approved by governance.

---

## Problem Statement

Persistently underperforming minipools impose systemic costs:

- Lower rETH yield
- Reduced rETH demand
- Slower queue clearance
- Delayed or constrained Saturn 1 rollout
- Reduced protocol revenue options (e.g., RPL fee switch, institutional incentives)

Because minipools can remain indefinitely on older delegate logic, it is difficult or impossible to enforce protocol-wide standards. This limitation does not apply to megapools, which do not use the same delegate model.

---

## Specification

This proposal introduces a Smartnode update that enforces the use of the latest delegate for minipools after upgrading.

### Implementation options (non-exclusive)

One of the following (or an equivalent mechanism) would be implemented:

1. Upon Smartnode upgrade, `use-latest-delegate` is set to `yes` and cannot be toggled to `no`.
2. Upon Smartnode upgrade, `use-latest-delegate` is set to `yes`, with optional ability to toggle back (subject to governance decision).

The exact implementation details are left to the core team and governance process.

### Scope

- Applies **only after** upgrading to the specified Smartnode version.
- Does **not** retroactively modify minipools on older Smartnode versions.
- Does **not** itself implement forced exits; it enables future governance-approved mechanisms.

---

## Rationale

This change prioritizes protocol-wide consistency and enforceability over permanent opt-out flexibility. As Rocket Pool scales and transitions to Saturn-era architecture, the inability to uniformly apply protocol logic becomes an increasing risk to rETH holders and protocol sustainability.

The delegate upgrade mechanism already exists; this proposal ensures it cannot be indefinitely bypassed.

---

## Backwards Compatibility

- Existing minipools continue operating normally until the node operator upgrades Smartnode.
- No changes are applied to nodes that do not upgrade.
- Minipools created after upgrading Smartnode would automatically follow the enforced delegate behavior.

---

## Risks and Considerations

- **Governance risk concentration:** Delegate upgrades become more powerful and require stronger review processes.
- **Loss of opt-out:** Node operators lose the ability to permanently pin delegates after upgrading.
- **Trust requirements:** Increased importance of transparent communication, audits, and governance safeguards around delegate changes.
- **Community alignment:** Some node operators may strongly oppose the change on philosophical grounds.

---

## Security Considerations

This proposal increases the importance of governance integrity and delegate upgrade review. Mitigations may include:

- Formalized review and audit processes for delegate updates
- Clear rollout timelines and communication
- Optional grace periods prior to enforcement
- Emergency rollback mechanisms (if technically feasible)

These safeguards are out of scope for this RPIP but should be discussed alongside it.

---

## Governance Path

1. Community discussion (forum + Discord)
2. Sentiment poll
3. Snapshot vote
4. Smartnode implementation (if approved)

---

## Open Questions

- Should delegate upgrades require explicit pDAO approval?
- Should different categories of delegate changes have different governance thresholds?
- Should any opt-out mechanism remain under narrowly defined conditions?
- What safeguards are required before enabling enforcement features like forced exits?

---

## Conclusion

This proposal represents a significant philosophical shift, but one driven by observed protocol limitations rather than theory. As Rocket Pool evolves, ensuring consistent, enforceable protocol behavior may be necessary to protect rETH holders and support long-term scalability.

The community is encouraged to scrutinize this proposal carefully and contribute to shaping the governance controls that would accompany it.
