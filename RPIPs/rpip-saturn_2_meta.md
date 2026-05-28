---
rpip:
title: Saturn 2 Upgrade
description: Provides an introduction and overview of the Saturn 2 Upgrade, its likely contents, and its current status.
author: knoshua (@knoshua)
discussions-to: <URL>
status: Living
type: Informational
created: 2026-05-27
---

## Abstract
This informational RPIP provides an introduction and overview of the Saturn 2 upgrade and its likely contents. It lists and briefly describes RPIPs planned or considered to be included, and links out to the RPIPs that specify those components in greater detail.

It also addresses components that were originally planned to be included in Saturn 2 based on [RPIP-49](RPIP-49.md) and why the pDAO's position on it has changed since then.
## Contents

### [RPIP-44: Integrating Execution Layer Triggerable Exits](RPIP-44.md)

This allows node operators to exit their megapool validators on the execution layer and anyone to exit them in case the megapool has `debt` from mev penalties or shortfall from exited validators.

### [RPIP-46: Universal Adjustable Revenue Split](RPIP-46.md)

This RPIP has been partially implemented in Saturn 1 already. In Saturn 2, it calls for setting RPL inflation to 1.5% and removes RPL rewards to node operators.

### [RPIP-71: rETH Withdrawal Liquidity via EIP-7002](RPIP-71.md)

This proposal allows rETH holders to join a withdrawal queue and defines conditions and rules for selecting minipools and megapool validators for exit to fulfill those requests.  The goal is to make rETH more attractive by giving a liquidity guarantee and minimizing times of rETH trading below peg.

### [RPIP-73: rETH Protection From Underperforming Nodes](RPIP-73.md)

This proposal introduces a permissionless on-chain challenge mechanism to exit significantly underperforming minipools and megapool validators. The goal is to make rETH more attractive by making its yield more competitive.

### [RPIP-79: Faster Withdrawal Credentials Check](RPIP-79.md)

Currently the second deposit (31 ETH) for a validator can only happen once the first deposit (1 ETH) has made it through the Beacon Chain queue, which results in a long waiting time. This RPIP introduces a fraud-proof system using beacon state to protect against withdrawal-credential front-running that avoids this delay.

### [RPIP-80: Exit Requests, Triggering Exits, and Minipool Penalties](RPIP-80.md)

This proposal defines exit requests that are the basis for RPIP-71 and RPIP-73. It also introduces penalties for minipools that fail to exit when requested.

### [RPIP-TBD: Enable pDAO Spending of ETH and ERC-20](RPIP-TBD.md)

This proposal allows pDAO to spend ETH or any ERC-20 in the treasury. This is relevant because pDAO has the ability to collect a share of ETH commission from megapools.

### [RPIP-TBD: Increase Bond Requirement](RPIP-TBD.md)

This proposal increaes bond per megapool validator to allow for more megapools for a given rETH demand. The aim is to get to a megapool-only state sooner and give something back to node operators on minipools that may be exited under RPIP-71.

### [RPIP-TBD: pDAO Settings](RPIP-TBD.md)

This RPIP provides an overview of pDAO parameters, guardrails and reasons for them. (Only include if there are changes to guardrails)

### [RPIP-TBD: On-Chain pDAO Voting (Snapshot Migration)](RPIP-TBD.md)

This RPIP allows the pDAO to use the existing on-chain voting system for pDAO parameters to be used for more general governance proposals and removes the need for Snapshot votes. Snapshot started charging for their service and we are looking to remove that dependency. 


## Deferred/Outstanding

## [RPIP-42: Bond Curves](RPIP_42.md)

This RPIP was included in the [RPIP-49](RPI-49.md) vote and called for lowering the bond requirement for megapool validators to 1.5 ETH after the second one. We want to hold off on this until there is rETH demand that warrants this kind of capacity.

### [RPIP-46: Universal Adjustable Revenue Split](RPIP-46.md)

This RPIP also called for implementation of a revenue share model that was yet to be voted on. Discussed options included [RPIP-45: RPL Burn](RPIP-45.md) and [RPIP-50: RPL Buy & LP](RPIP-50.md). Now we feel like that research and development efforts are better spent on improving rETH and the RPIPs listed above. 

### Penalty System

RPIP-49 also called for researching, drafting and ratifying a penalty system. 

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
