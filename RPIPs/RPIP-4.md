---
rpip: 4
title: Community Resolutions and Voting
author: Mike Leach (@VVander) <mike@bamboofin.tech>
discussions-to: https://dao.rocketpool.net/t/snapshot-voting/400
status: Draft
type: Meta
created: 2022-03-21
---

## Abstract
The Rocket Pool community at large agrees to use Snapshot voting for governance proposals via the snapshot.org platform. Snapshot votes which pass mean their proposals are immediately adopted and the RP DAO will endeavor to enact them as soon as reasonably possible. This proposal is not related to the protocol DAO (pDAO), which refers to a planned on-chain protocol governance mechanism.

The Snapshot voting mechanism uses staked and weighted RPL token voting.

## Motivation
Governance in Rocket Pool is currently ad hoc, and there is no way for the community to adopt proposals. We must adopt a governance mechanism, even if imperfect, before any governance actions can be taken legitimately, such as but not limited to spending pDAO funds.

## Implementation
  
All Rocket Pool governance proposals must first be posted as a new topic on the governance forums at dao.rocketpool.net. Topics should include a poll with the following format:

```
[poll type=regular results=always chartType=bar]
* Support
* Oppose
[/poll]
```

A favorable forum poll which is live for at least [7] days and a corresponding RPIP document are necessary for snapshot voting to be scheduled. Once the requirements have been met, one of the following parties will create a snapshot vote and publicize it:

- Darren Langley AKA langers (General Manager - Rocket Pool Pty Ltd)
- Rocket Pool DAO Leadership Council (address 0x123WESHOULDMAKETHIS or rpdao.eth)

Given that the snapshot voting system is considered relatively insecure (see Security Considerations below), the RP DAO Leadership Council may exercise veto power over any proposal by refusing to bring it forward for consideration.

The snapshot vote will run for [14] days and requires a [15%] voting power quorum to be successful. In the case where no quorum has been met for three of the last four polls, the needed quorum will be reduced by [25%] permanently unless governance action dictates otherwise.

### Snapshot Vote Strategy

#### Eligibility

To be eligible for vote participation, the RPL must be staked in the Rocket Pool protocol for at least 24 hours prior to the vote. 

#### Delegation

Eligible RPL voting power may instead be delegated to another wallet. All delegated RPL voting power is added to the total voting power of the voting wallet according to the formula below.

#### RPL Voting 

All RPL votes are recorded into a `results` map which requires a Rocket Pool node address as the index (`address`). The `results` map's values include:
- an integer `vote` which indicates the chosen vote option
    + generally, `0` corresponds to "yea", `1` corresponds to "nay", and `2` corresponds to "abstain", though polls may use these options to instead include more specific choices, with the only limit being the maximum integer value
- an integer `power` which contains the ultimate voting power of that node

`results.power` for is determined in accordance with the following equation for voting: 

`results.power = (int) 0.5 * √rpl * weight`, where `rpl` is the amount of RPL staked in that node, `weight` is the result of the weight calculation (see below), and the result is truncated into an integer.

`weight` for is determined from the age of the node via:

`weight = min( (currentDate - registrationDate) / 100, 1)` where `currentDate` is the date of the snapshot in UTC and `registrationDate` is the date on which the node was registered in UTC.

Any delegated vote `results.power` is summed with the voter's own `results.power` to achieve the final vote power for a single voter. Addresses which delegate their vote power are not included in the `results` map.

Total vote power for each of the poll choices indicated in each `results.vote` entry is summed into an `rplTotalPower` array containing the total sum of power for each of the poll choices, with the index of each entry mapping to the integer corresponding to the vote choice. This is used to compute the outcome as described below.

#### Outcome

The `rplTotalPower` array is searched for the maximum value. The index of this maximum value corresponds to the winning proposal. If the winning choice corresponds to "abstain", however, then the proposal corresponding to the index of the second highest value is declared as the winner instead.

## Rationale

### Snapshot.org

Snapshot.org is a web3-native platform for token voting and provides several pre-strategies for accomplishing this, such as 1-to-1 voting and qudratic voting. This platform also provides the capability to create custom voting strategies, which is necessary for Rocket Pool to include staked RPL, for example. The Snapshot.org platform is widely used and does not require fees.

### RPL Stake and Weight

RPL votes are weighted based on age of the node so as to prevent short-term nodes from being created and destroyed for the purposes of vote-buying, which would add unnecessary instability to the protocol.

RPL is required to be staked for voting so as to prevent vote-buying by participants unaffiliated with the protocol's operations.

The algorithm used for calculating power uses a square root and a `0.5` factor for flattening the power contributed by any single node. This helps prevent large node operators from obtaining dominance over the voting process.

### RPL vs rETH Voting

RPL is used in isolation -- not in conjunction with rETH -- because vote-buying is guarded against via staking and node creation is more sybil-resistant. While sybil-resistance is not guaranteed with this RPL-only method, it was chosen as the best compromise among the various options. 

## Security Considerations

As with any token-based voting mechanism, even voting with a power-flattening algorithm such as the one used here allows large actors to influence the outcome of votes. Although this risk is significantly lower with this strategy vs 1-to-1 token votes, it is possible that a large actor may create a proposal which harms the security of the protocol and force it to be adopted via this mechanism.

As of 08-13-2022, the full quorum is computed at. To singlehandedly pass a governance proposal, an individual or collective would need to amass at least 50.1% of total voting power. Assuming the full quorum of roughly 4,000 voting power participation opposes them and all other votes abstain, a single actor would need to stake just 640,000 RPL across 10 nodes for at least 100 days to reach this threshold. This is well within reach of known protocol participants today as well as many outside actors. **Therefore, this system should be considered insecure and attackable.** The Rocket Pool community and the RP DAO Leadership Council must remain diligent in both its analysis of and participation in proposal votes to prevent governance attacks.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
