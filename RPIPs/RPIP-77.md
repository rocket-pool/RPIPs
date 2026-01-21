---
rpip: 77
title: Set Smart Node Default to Use Latest Delegate for Minipools
description: Update Smart Node so by default minipools use the latest protocol-approved delegate and remove supported Smart Node configuration paths for setting older delegate implementations.
author: Dr Doofus (@DrDoofus-MD-PhD-DDS)
discussions-to: https://dao.rocketpool.net/t/require-minipools-to-use-latest-delegate/3844
status: Final
type: Protocol
category: Core
created: 2026-01-05
tags: [minipools, delegate, smartnode]
---

## Abstract

This proposal specifies that a future Smart Node update will by default set minipools managed via Smart Node to use the latest delegate smart contract and remove supported Smart Node configuration options for setting older delegate implementations. The intent is to improve protocol-wide consistency among minipools, enable pDAO-approved minipool changes, and remove some technical blockers to future enforcement mechanisms (including, but not limited to, forced exits of persistently underperforming minipools).

## Motivation

A non-trivial number of minipools are persistently underperforming, reducing overall rETH yield and harming demand. Reduced rETH demand complicates the protocol’s transition to the Saturn 1 architecture, which includes megapools, the RPL fee switch, and alternative protocol funding mechanisms.

While support efforts to remediate underperforming node operators are ongoing, the protocol currently lacks an enforcement mechanism since minipools are not set to use the most recent delegate contract by default, and it is easy for minipool operators to opt out of delegate upgrades. This creates a situation where governance-approved protocol logic cannot be universally applied. Currently, only 7% of operators have proactively selected to use the latest delegate, and many of those who have not are likely not even aware of this option.

## Background

### Minipool delegate architecture

Rocket Pool minipools are implemented using a proxy pattern. The minipool contract itself contains minimal logic and instead forwards most calls to a **delegate implementation contract** (the "delegate”). The delegate controls, among other things:

- Minipool lifecycle and state transitions
- Validator launch conditions
- ETH balance distribution between node operators and rETH holders
- Penalty and slashing logic
- Withdrawal, exit, and finalization behavior
- Bug fixes and protocol rule updates without redeploying minipools

### `use-latest-delegate` flag

Node operators may currently configure each minipool to either:

- Use the delegate active at the time of creation (default), then upgrade to future delegates of their choosing, or
- Use the latest delegate at all times

### Historical rationale for opt-in delegate upgrades

The opt-in `use-latest-delegate` model was originally designed to:

1. Protect node operators from potentially malicious or unsafe governance upgrades.
2. Allow minipools to converge on a long-lived (“Lindy”) delegate without mandatory churn.
3. Provide node operators an exit path if they disagreed with protocol changes.

This proposal revisits these assumptions in light of changes to Rocket Pool’s governance and security model.

## Specification

### Implementation

- Smart Node SHALL monitor the return status of `getFinalised()` and `getUseLatestDelegate()` on minipool contracts of its associated node.

- Should `getFinalised()` and `getUseLatestDelegate()` both return `false`, Smart Node SHALL attempt to initiate a call of `setUseLatestDelegate(true)` to the minipool contract from the local node wallet.

- A Smart Node update SHALL remove supported Smart Node configuration options for setting older minipool delegate implementations.

- The Smart Node update SHOULD be implemented at the earliest reasonable opportunity that does not interfere with the Saturn 1 launch.

### Scope

- Applies **only after** upgrading to the specified Smart Node version.

- Does **not** retroactively modify minipools on older Smart Node versions.

- Does **not** itself implement forced exits; it enables future governance-approved mechanisms.

- Does **not** grant any emergency or unilateral powers to the core team or governance bodies beyond existing delegate upgrade mechanisms.

- Does **not** prevent an older delegate from being used at the contract level, it just removes the ability to change the use latest delegate option via Smart Node.

### Delegate Update Oversight

- Delegate updates that materially affect validator exits, reward distribution, penalties, or governance authority MUST only be implemented following pDAO vetting via vote.

## Rationale

Persistently underperforming minipools impose systemic costs:

- Lower rETH yield
- Reduced rETH demand
- Slower queue clearance
- Delayed or constrained Saturn 1 rollout
- Reduced protocol revenue options (e.g., RPL fee switch, institutional incentives)

Because the previous Smart Node behavior did not set the use latest delegate flag to yes, many Node Operators by default remain on older delegate versions, even if they would be satisfied to be on the most recent deployment. This is an unnecessary obstacle that this proposal corrects. Note that this limitation does not apply to megapools, which do not use the same delegate opt-in model.

## Backwards Compatibility

- Existing minipools continue operating normally until the node operator updates Smart Node.

- No changes are applied to nodes that do not update.

- Node operators retain the ability to exit minipools prior to upgrading Smart Node, and governance processes are expected to provide sufficient notice before delegate changes that materially affect operator behavior.

- Node operators can change delegate versions outside the scope of Smart Node UI and/or commandline interfaces.

## Risks and Considerations

- **Governance risk concentration:** Delegate upgrades become more powerful and require stronger review processes.

- **Trust requirements:** Increased importance of transparent communication, audits, and governance safeguards around delegate changes.

- **Community alignment:** Some node operators may strongly oppose the change on philosophical grounds. Even though there are ways around this.

- **Imperfect enforcement**: Because this change is gated on Smart Node updates, some node operators—either due to custom setups or intentional avoidance—may remain on older Smart Node versions and continue using older delegates. This limits immediate coverage and means the proposal should be viewed as a necessary enabling step rather than a complete solution on its own.

- **Loss of Lindy delegate stability:** Forcing delegate upgrades sacrifices the ability for minipools to remain indefinitely on a long-lived delegate implementation. While this may increase upgrade churn, it is a deliberate tradeoff in favor of enforceability and protocol-wide consistency among minipools managed via Smart Node.

Several original justifications for opt-in delegate upgrades have been partially mitigated over time. Saturn-era governance introduces additional safeguards, including a Security Council veto, reducing the risk of malicious upgrades. Additionally, long governance lead times and explicit votes may serve as a functional replacement for individual opt-out as an exit mechanism.

## Conclusion

This proposal represents a philosophical shift, but one driven by observed protocol limitations rather than theory. As Rocket Pool evolves, ensuring consistent, enforceable protocol behavior may be necessary to protect rETH holders and support long-term scalability.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
