---
rpip: 54
title: Upgrade Informational - Houston
description: An unoffical community resource to track contents and information about the Houston upgrade.
author: LongForWisdom (@LongForWisdom)
discussions-to: N/A
status: Living
type: Informational
created: 2024-05-20
requires: 31, 32, 33, 35
---

## Abstract

This unofficial community RPIP lists the estimated contents of the Rocket Pool Houston upgrade. This information has not been reviewed or endorsed by the Rocket Pool core development team at this time. This RPIP should be marked as Final once this upgrade has taken place.

## Motivation

The intention of this Living Informational RPIP is to collate the release-relevant information for this upcoming release in a single location for reference. While it is created as a community resource, the hope is for the Rocket Pool core development team to adopt this template and format to communicate about upcoming releases as and when it makes sense for them to do so.

## Estimated Included RPIPs
* [RPIP-31: RPL Withdrawal Address](RPIP-31.md)
* [RPIP-32: Stake ETH on behalf of node](RPIP-32.md)
* [RPIP-33: Implementation of an On-Chain pDAO](RPIP-33.md)
* [RPIP-35: Time-based Balance and RPL Price Submissions](RPIP-35.md)

## Key Links 
* [Upgrade Documentation](https://docs.rocketpool.net/guides/houston/whats-new)
* Audits
  * [Consensys Diligence](https://consensys.io/diligence/audits/2023/12/rocket-pool-houston/) (Late November to Mid December 2023)
  * [Sigma Prime](https://rocketpool.net/files/audits/sigma-prime-audit-houston.pdf) (Late November 2023, then a second round March 2024)
  * [Chainsafe](https://rocketpool.net/files/audits/chainsafe-audit-houston.pdf) (Mid January to April 2024)
* [Houston Branch](https://github.com/rocket-pool/rocketpool/tree/houston), [Houston Hotfix Branch](https://github.com/rocket-pool/rocketpool/tree/houston-hotfix)
* Upgrade Contract (TBC post mainnet deployment)

## Outstanding Discrepancies

### #3 RPIP-33 - Recurring Treasury Spend Interval Amount

Per [RPIP-33](https://rpips.rocketpool.net/RPIPs/RPIP-33#treasury-contract-change): 

> Amount Per Interval: The amount of RPL to send per reward interval. Specified as a percentage of RPL rewards the pDAO received in a given interval.

Per the [Houston documentation](https://docs.rocketpool.net/guides/houston/participate#creating-a-recurring-treasury-spend) the implementation is for absolute amount of RPL per payment interval, rather than a percentage amount. 

**Current Status:** Outstanding Discrepancy. Coded version may make more sense. Will likely try to resolve via forum sentiment vote to avoid overhead of full pDAO snapshot vote.  
**Estimated Impact:** Medium-Low

## Resolved Discrepancies

<details>
	<summary>Click to expand resolved discrepancies</summary>

### #1 RPIP-31 - Triggering RPL Reward Claims

Per [RPIP-31](https://rpips.rocketpool.net/RPIPs/RPIP-31#claiming-rpl-rewards):

>As the controller of the RPL for a node, I MUST be able to trigger a claim of RPL rewards

In code, this isn't absolute and can be prevented by the node operator (less of an issue with v10, but still exists). Should at least be highlighted under Security Considerations.

Further described by Knoshua [here](https://dao.rocketpool.net/t/rewards-tree-spec-v10/2937/22).

**Current Status:** Resolved. Fixed in Houston contracts via [hotfix](https://github.com/rocket-pool/rocketpool/tree/houston-hotfix) prior to release.  
**Estimated Impact:** Medium

### #2 RPIP-31 - Triggering RPL Reward Claims Valid Sources

Per [RPIP-31](https://rpips.rocketpool.net/RPIPs/RPIP-31#claiming-rpl-rewards):

>If a node's RPL withdrawal address is set, the call MUST come from one of: the node's primary withdrawal address, the current RPL withdrawal address, or the node's address

Per the [Houston documentation](https://docs.rocketpool.net/guides/houston/whats-new#rpl-withdrawal-address), only the current RPL withdrawal address is able to claim in the current implementation. 

Further described by Knoshua [here](https://dao.rocketpool.net/t/rewards-tree-spec-v10/2937/22).

**Current Status:** Resolved. Fixed in Houston contracts via [hotfix](https://github.com/rocket-pool/rocketpool/tree/houston-hotfix) prior to release.  
**Estimated Impact:** Medium-Low

### #4 RPIP-33 - Security Council Quorum Thresholds

Per [RPIP-33](https://rpips.rocketpool.net/RPIPs/RPIP-33#parameter-table): 

>rocketDAOProtocolSettingsSecurity - security.members.quorum ... `>= 51% & < 75%`

Per the Houston audit, the implemented check is `>= 51% and <=75%`

**Current Status:** Resolved. Coded version may make more sense.  
**Estimated Impact:** Very Low

### #5 RPIP-33 - Network Submission Frequency Guard

Per [RPIP-33](https://rpips.rocketpool.net/RPIPs/RPIP-33#parameter-table): 

>rocketDAOProtocolSettingsNetwork - network.submit.balances.frequency ... `> 1 hour`

Per the Houston audit, the implemented check is `>= 1 hour`.

**Current Status:** Resolved. Coded version may make more sense.   
**Estimated Impact:** Very Low
</details>






## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).