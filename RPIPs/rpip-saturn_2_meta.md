---
rpip:
title: Saturn 2 Upgrade
description: Provides an introduction and overview of the Saturn 2 upgrade, its likely contents, and its current status.
author: knoshua (@knoshua)
discussions-to: <URL>
status: Living
type: Informational
created: 2026-05-27
---

## Abstract

This informational RPIP provides an introduction and overview of the Saturn 2 upgrade and its likely contents.

Saturn 2 is the second major phase of the tokenomics-driven roadmap, originally outlined in RPIP-49, and follows the Saturn 1 upgrade. Whereas Saturn 1 focused on deploying megapools, the first stage of Universal Adjustable Revenue Split (UARS) and other supporting mechanics, Saturn 2 is centered on three broad themes: protocol-level rETH liquidity, protection of rETH from underperforming validators, and adjusting economics. It also includes some governance improvements.

This RPIP briefly describes the RPIPs that are planned or considered for inclusion in Saturn 2 and how they fit together. It also explains which components originally earmarked for Saturn 2 in RPIP-49 have been deferred, and why the pDAO’s position on them has evolved.

This RPIP represents the current state of an ongoing effort, and the final contents of the Saturn 2 upgrade may differ from the set described here based on team and community feedback and governance decisions.

## Overview

At a high level, Saturn 2 is intended to:

- Provide protocol-level rETH withdrawal liquidity by allowing rETH holders to request withdrawals that automatically trigger validator exits when necessary, subject only to Ethereum’s exit queue.
- Protect rETH yield from long-running underperformance by introducing an on-chain, permissionless mechanism to eject badly performing validators.
- Include minipools in both of the above as far as possible by introducing a penalty system.
- Increase megapool bond requirements so the protocol can reach a megapool-only state more quickly while still fairly treating node operators whose minipools may be exited to honor rETH withdrawals.
- Reduce RPL issuance to 1.5% and end ongoing rewards for node operators.
- Improve and extend the protocol in several smaller aspects: speed up the creation of new validators, enable pDAO spending of treasury ETH, and enable on-chain pDAO signaling votes.

The following items that were included in the original Saturn 2 scope of the 2024 Tokenomics vote will be deferred:

- Lowering the bond requirement per megapool validator to 1.5 ETH after the second one.
- Implement a more sophisticated revenue share model (e.g., RPL Burn or Buy & LP).

The sections below group the Saturn 2 components by theme and then summarize the individual RPIPs.

## rETH Withdrawal Liquidity and Underperformance Protection

### [RPIP-71: rETH Withdrawal Liquidity via EIP-7002](RPIP-71.md)

**Status:** Draft; Not yet voted on.

The goal of RPIP-71 is to make rETH more attractive by giving a liquidity guarantee to rETH holders and minimizing the times of rETH trading below the peg. It defines a path for rETH holders to redeem rETH for ETH in-protocol, when validators are exited for that purpose, and how these validators are selected.

- rETH holders can place their rETH in a withdrawal queue, at which point it stops earning rewards.
- The withdrawal queue gets prioritized over new validators, and rETH burns outside the withdrawal queue.
- When the rETH in the withdrawal queue reaches a threshold that justifies it, validators are exited.
- RPIP-71 depends on RPIP-80’s exit-request framework.
- In a first phase, the oDAO selects minipools, higher commission first, to align withdrawal liquidity with the desired transition to megapools.
- In a second phase, megapool validators are selected in a trustless manner.
- The pDAO still needs to decide on a concrete exit criterion for megapools. Options discussed so far include:
  - Lower RPL stake first
  - first in, first out
  - together with increasing bond requirement, exiting from megapools, the furthest below the bond requirement
  - biasing towards larger nodes
  - Bias towards not hitting the same node again

### [RPIP-73: rETH Protection From Underperforming Nodes](RPIP-73.md)

**Status:** Draft; Not yet voted on.

RPIP-73 introduces a permissionless on-chain challenge and exit mechanism for validators that perform significantly worse than expected over long durations. It focuses on protecting rETH yield by removing validators that are consistently offline or otherwise severely underperforming, rather than on one-off performance incidents.

- Timely target attestation rate is used as a proxy for performance.
- Anyone can challenge underperforming validators, and the correctness of challenges is ensured with proofs against on-chain beacon chain data.

## Exit and Entry Infrastructure

### [RPIP-80: Exit Requests, Triggering Exits, and Minipool Penalties](RPIP-80.md)

**Status:** Draft; Not yet voted on.

RPIP-80 defines the common exit-request abstraction that underpins both rETH withdrawal exits (RPIP-71) and underperformance exits (RPIP-73).

- Validators that ought to exit are first given an opportunity to do so via the consensus-layer exit route, without incurring gas costs. This process is expected to be automated via smartnode.
- For validators that don’t respond to these requests in time, anyone can trigger an exit.
- Minipools that cannot be exited instead incur a penalty each time they fail to respond to an exit request, providing an economic incentive to exit.
- Accounting for ETH expected to exit, or already in the process of exiting, ensures we don’t exit more validators than necessary for rETH withdrawals.

### [RPIP-44: Integrating Execution Layer Triggerable Exits](RPIP-44.md)

**Status:** Included in 2024 Tokenomics Rework vote

RPIP-44 provides node operators with an alternative way to exit megapool validators from the execution layer without requiring the validator's keys. It also enables exiting validators in case a megapool has debt from MEV penalties or a shortfall due to exited validators.

### [RPIP-79: Faster Withdrawal Credentials Check](RPIP-79.md)

**Status:** Draft; Not yet voted on.

RPIP-79 speeds up validator onboarding and removes the dependency on the entry queue for the second deposit, while still guarding against withdrawal-credential front-running in megapools. In the context of exiting underperformers (RPIP-73), this minimizes churn from replacing them with better-performing operators.

- Currently, creating a megapool validator requires a beacon-state proof to verify that the withdrawal credentials are correctly set before the second deposit (31 ETH) can occur, which can only be done once the first deposit (1 ETH) has passed through the beacon chain queue.
- Under the proposal, the design is flipped: A proof can be provided to show that the withdrawal credentials are set incorrectly.
- If no such proof is provided within a reasonable time, the second deposit is allowed to proceed.

## Economics

RPIP-49 originally framed Saturn 2 in terms of several tokenomics components: the remainder of the bond curve, RPL inflation reduction, and Universal Adjustable Revenue Split (UARS) completion, with the implementation of the revenue share strategy. Saturn 2, as described in this RPIP, maintains some of that scope but defers or reprioritizes other parts.

### [RPIP-46: Universal Adjustable Revenue Split](RPIP-46.md)

**Status:** Included in 2024 Tokenomics Rework vote. Partially implemented in Saturn 1; RPL inflation change targeted for Saturn 2, and revenue-share decision deferred.

Saturn 1 introduced UARS with a configurable split of ETH commission between node operator commission, voter share (vote-eligible RPL), and rETH share, while extending RPL issuance rewards to all staked RPL.

For Saturn 2, RPIP-46 specifies that:

- RPL inflation is reduced to 1.5% per year, down from the historical 5%.
- RPL issuance rewards to node operators are eliminated. Voter share provides an incentive to stake RPL.
- All RPL inflation is directed to the DAOs: 95% to the pDAO and 5% to the oDAO.
- The outcome of a revenue share vote is implemented, potentially RPL Burn or RPL Buy & LP.

The RPL inflation-related changes are still planned for Saturn 2. The current view is that, at this point, rETH demand-related work has higher priority than implementing complex new RPL value-capture mechanisms, and that detailed revenue-share strategies can be revisited after the rETH-focused work of Saturn 2 is delivered.

### [RPIP-42: Bond Curves](RPIP_42.md)


**Status**: Included in 2024 Tokenomics Rework vote. Partially implemented in Saturn 1; removing remaining aspects from Saturn 2 not yet voted on.

RPIP-42 specifies that reduced_bond is lowered to 1.5 ETH with Saturn 2. While this further improves the efficiency of megapools and is safe with the introduction of RPIP-44, it is unclear whether the rETH demand will be sufficient to support 1.5 ETH validators immediately. The current thinking is not to lower reduced_bond until there is rETH demand that justifies it.

### [RPIP-83: Increasing Bond Requirement](RPIP-83.md)

RPIP-83 instead proposes increasing the required node-operator bond for each megapool validator to 6 ETH. This change is motivated by the current lack of rETH demand, the desire to reach a “megapool-only” state sooner, and the goal of keeping our large node operator set intact with exits for rETH withdrawal liquidity under RPIP-71. Specifically, RPIP-83 specifies:

- Require 6 ETH per new validator in megapools.
- Allow node operators to top up their bonds to match the new 6 ETH-per-validator curve.
- Megapool rewards are used to increase the bond below the bond curve rather than being paid out directly.

## Governance

### [RPIP-82: Enable pDAO Spending of ETH and ERC-20](RPIP-82.md)

**Status:** Draft; Not yet voted on.

[RPIP-72](RPIP-72.md) introduced a pdao_share to UARS (set to 0) that will allow the pDAO to collect a share of the ETH earned by megapool validators in the future, but there is currently no way to spend such ETH in the treasury. RPIP-82 adds this functionality and also allows spending any other ERC-20 token.

### RPIP-TBD: On-Chain Signal Voting

**Status:** Draft; Not yet voted on.

The Houston upgrade introduced on-chain pDAO voting for parameter changes and treasury spending ([RPIP-33](RPIP-33.md)), but the pDAO still relies on Snapshot voting for other governance votes. This RPIP lays the foundation for on-chain signal voting that, together with a voting frontend, will support arbitrary vote types, including weighted voting (used for IMC and GMC elections), ranked-choice voting, and approval voting.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
