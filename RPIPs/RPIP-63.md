---
rpip: 63
title: Houston Hotfix
description: This proposal defines what is included in the Houston hotfix.
author: langers (@langers), knoshua (@knoshua)
discussions-to: https://dao.rocketpool.net/t/rpip-63-houston-hotfix/3269
status: Final
type: Protocol
category: Core
created: 2024-09-09
vote-to: https://vote.rocketpool.net/#/proposal/0x1d75c5247cf457d26fbf7f26622c21de912565256fc3273b92a40aa14dd1af3d
vote-date: 2024-10-14
vote-result: Passed
---

## Abstract

The Houston hotfix does not introduce new features but does include important changes to existing features that:

- Fix a discovered bug with finalising legacy minipools
- Resolves Immunefi bug bounty submissions regarding the onchain voting system
- Improves Protocol DAO quorum default guardrail value (currently set too high)
- Sets the Protocol DAO quorum to **30%**
- Fixes a couple of issues with Protocol DAO parameters
- Modifies how the onchain voting system burns a veto'd proposer bond - now actually burns the RPL rather than transferring it to 0x0 address
- Changes the Scrub Penalty from being RPL based to ETH based
- Miscellaneous housekeeping of parameter names to ensure consistency 

## Motivation

Soon after the Houston launch, an issue was reported with finalising legacy minipools. The issue was introduced by the new onchain voting snapshoting system that ensures proposal quorum information can be accessed for historical blocks onchain. A smart node release was deployed to contain the issue but it was decided that a smart contract hotfix would be released to resolve the issue.

In the meantime, 3x Immunefi bug bounties were raised regarding the onchain voting system.
- A medium bug bounty that enabled a node operator to exit RPL stake via the onchain proposal challenge system.
- Two high bug bounties that enabled someone to steal a proposer's bond by preventing them from responding to challenges. 
While working on the bug bounties we decided to modify how the voting system burns a veto'd proposer bond. We decided to use RPL's burn function rather than transferring to the 0x0 address so that token supply is recorded accurately. 

Additionally, the community discovered that the Protocol DAO quorum guardrail is set too high, as it should be possible to match it with RPIP-4. The community also pointed out some small parameter name inconsistencies. 

There has also been discussion around a potential tokenomics rework prelude [RPIP-62](https://rpips.rocketpool.net/RPIPs/RPIP-62) that uses a combination of setting changes and reward tree changes, to deliver some of the value of Saturn earlier. One issue with rolling out these changes is that they would break the RPL scrub penalty that prevents a potential security exploit. Ultimately the scrub penalty will be removed in Saturn due to an improved proof-based mechanism but in the meantime it is needed. To give us the option to support RPIP-62 and ensure safety if the pDAO was to set minimum RPL stake to 0, the scrub penalty change has been included in this hotfix.

## Specification

### Finalising Legacy Minipools Issue

- `RocketMinipoolManager` SHALL calculate ETH matched correctly and update the network snapshots.
- A function SHALL be provided on the upgrade contract to fix any node operators who have experienced the ETH matched bug

### Parameter Fixes

- The `RocketDAOProtocolSettingsAuction` `Lot Duration` guardrail SHALL use blocks instead of seconds 
- When setting the `RocketDAOProtocolSettingsNode` `MinimumPerMinipoolStake` parameter to 0, `RocketNodeStaking` `getNodeETHMatchedLimit` SHALL NOT revert due to division by 0
- Add `RocketDAOProtocolSettingsAuction` `CreateLotEnabled` SHALL be added to the list of pDAO parameters that can be modified by the Security Council
- Add `RocketDAOProtocolSettingsAuction` `BidOnLotEnabled` SHALL be added to the list of pDAO parameters that can be modified by the Security Council
- Non-functional fixes to improve parameter names and remove typos MAY be included

### Protocol Quorum

- The `RocketDAOProtocolSettingsProposals` `Proposal Quorum` minimum safeguard SHALL be changed from 51% to 15%
- The `RocketDAOProtocolSettingsProposals` `Proposal Quorum` SHALL be set to 30% - there is currently 50k vote power initialised out of 94k so 30% ensures quorum is above the mandated 15%, as per RPIP-4

### Immunefi Bug Bounties

- On a successful challenge 20% of the proposer's bond SHALL be burnt and the remaining distributed to challengers
- Challenge responses SHALL be restricted to the proposer only
- If a node's vote power has not been initialised, when they stake or deposit, their vote power SHALL be automatically initialised (the smart node will make users aware of this) - to ensure consistent network snapshots and proposal challenges can be responded to
- A `RocketNetworkVoting.initialiseVotingFor` function SHALL be present to manually initialise any node operator that may have staked/deposited before initialising their vote power - to ensure consistent network snapshots and proposal challenges can be responded to
- Voting with 0 vote power SHALL be prevented

### Refactor Proposal Veto (RPL Burn)

- `RocketDAOProtocolVerifier` SHALL use the RPL token `burn` function when burning a veto'd proposer's bond

### Scrub Penalty from RPL to ETH

- When a minipool deposit fails the scrub check (mismatched withdrawal credentials), the minipool SHALL be dissolved and the scrub penalty SHALL be applied to the node's ETH bond (not RPL bond)

## Rationale

This proposal resolves post Houston launch issues, opens up roadmap options, and strengthens security around the onchain voting system based on bug bounty feedback.

## Considerations

There are smart node considerations when changing parameter and event names.

The smart node will display a warning when a node operator deposits or stakes and they have not initialised their vote power - as it will be auto initialised. 

## Security

The function to fix node operator matched ETH requires the team to enter the revised values (deltas). There is no way to make this trustless onchain so the changes will be made in a verifiable way. The changes will be applied to the upgrade contract and locked. The community/oDAO can then verify the changes before the upgrade is voted in by the oDAO.

The `RocketNetworkVoting.initialiseVotingFor` function allows anyone to initialise a node's vote power. It is restricted to initialising it to self delegation so cannot be used to delegate vote power to another party. Also calling the function on an initialised node operator would do nothing (does not override delegation). By initialising someone's vote power you are essentially just paying the gas they would have paid. Other mechanisms exist to auto initialise vote power but this specifically allows the team to initialse a node's vote power, if their current state may cause consistency issues with the network snapshots, effectively stopping a proposal challenge from being responded to. 

Burning RPL during veto and sending RPL to 0x0 is equivalent from a security perspective.

On a failed scrub check the oDAO vote to scrub a minipool. When the scrub quorum is reached (51% of oDAO) it is dissolved and the penalty is applied. rETH funds **+ the penalty amount (2.4 ETH)** are recycled back into the deposit pool at the time of dissolve. Consequently, rETH user funds are released and the penalty applied immediately. The remaining funds are claimable by the node operator. The penalty amount is equivalent to the amount that would have been slashed from the node operator bond. rETH users have always benefited from scrub penalties so this does not change, except now it is more direct (doesn't need to go through the auction system). The scrub penalty is defined in the smart contracts (set to 2.4 ETH) and not manipulatable by the oDAO. A scrub penalty can only be applied once to a minipool.

Without changes to the scrub penalty mechanics, the pDAO could set the minimum RPL stake to 0, which would remove the ability to penalize minipools that set incorrect withdrawal credentials. A malicious actor can create many minipools that need to be scrubbed and force the oDAO to scrub all of them without having provided slashable RPL. As long as the oDAO keeps scrubbing minipools, this is purely a griefing attack that is also costly to the attacker. If the oDAO were to run out of gas and not top up their wallets during the scrub period, the attacker could successfully perform a withdrawal credential exploit and steal rETH funds.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).