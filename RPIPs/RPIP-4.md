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
Governance in Rocket Pool is currently ad hoc, and there is no way for the community to adopt proposals. We must adopt a governance mechanism, even if imperfect, before any governance actions can be taken legitimately, such as spending pDAO funds.

## Implementation
  
All Rocket Pool governance proposals MUST have an associated post on the governance forums at dao.rocketpool.net.
Promising community sentiment and a corresponding RPIP document are REQUIRED for snapshot voting to be scheduled.

Topics SHOULD approximate community sentiment by including a poll with the following format which is live for at least [7] days:

```
[poll type=regular results=always chartType=bar]
* Support
* Oppose
* Third option
* etc
[/poll]
```

A favorable forum poll which is live for at least [7] days and a corresponding RPIP document are necessary for snapshot voting to be scheduled. Once the requirements have been met, either 1) Darren Langley AKA langers (General Manager - Rocket Pool Pty Ltd) or 2) a party he entrusts with this ability SHOULD create a snapshot vote and publicize it as soon as reasonably possible unless exercizing their veto power (see Vetoing below). 

The snapshot vote will run for [14] days and requires a [15%] voting power quorum to be successful. In the case where no quorum has been met for three of the last four polls, the needed quorum will be reduced by [25%] permanently unless governance action dictates otherwise.

### Snapshot Vote Strategy

#### Eligibility

To be eligible for vote participation, the RPL must be effectively staked in the Rocket Pool protocol as reported by `getNodeEffectiveRPLStake()`.

#### Delegation

Eligible RPL voting power MAY instead be delegated to another wallet. All delegated RPL voting power is added to the total voting power of the voting wallet according to the formula below.

#### RPL Voting 

All RPL votes are recorded into a `results` map which requires a Rocket Pool node address as the index (`address`). The `results` map's values SHALL include:
- an integer `vote` which indicates the chosen vote option
    + Generally, the following mapping SHOULD be used: `0` corresponds to "yea", `1` corresponds to "nay", and `2` corresponds to "abstain"
    + For a case with truly mutually exclusive options, an alternative mapping MAY be used, with the only limit being the maximum integer value
- an integer `power` which contains the ultimate voting power of that node

`results.power` is determined in accordance with the following equation for voting: 

`results.power = (int) √rpl`, where `rpl` is the amount of RPL staked in that node, with the result truncated into an integer for simplicity.

Any delegated vote `results.power` is summed with the voter's own `results.power` to achieve the final vote power for a single voter. Addresses which delegate their vote power are not included in the `results` map, but if a address that delegates its power instead votes directly, the address' `results.power` is no longer contributed towards the delegated address' power and is instead counted for the original address only.

Total vote power for each of the poll choices indicated in each `results.vote` entry is summed into an `rplTotalPower` array containing the total sum of power for each of the poll choices, with the index of each entry mapping to the integer corresponding to the vote choice. This is used to compute the outcome as described below.

#### Outcome

The `rplTotalPower` array is searched for the maximum value. The index of this maximum value corresponds to the winning proposal. If the winning choice corresponds to "abstain", however, then the proposal corresponding to the index of the second highest value is declared as the winner instead.

#### Vetoing
Given that the snapshot voting system is not comfortably secure (see Security Considerations below), there are two safeguard vetoes in place.

The first veto consists of those with the power to bring forward a vote (see Implementation above) refusing to bring such a vote forward for consideration. Although potential changes can be technically implemented by the pDAO guardian regardless of vote, the pDAO guardian MUST NOT do so and any such action is considered illegitimate.

A second, more absolute veto consists of the pDAO guardian refusing to implement a passed proposal.

Veto powers SHALL NOT be used lightly, and SHALL be reserved for cases of vote manipulation, malicious action, or proposals that would result in clear damage to the Rocket Pool project.

When a veto power is excercised, the entity excercising a veto SHALL publish a Veto Explanation Document that describes why this step was taken. If possible, the report SHOULD also suggest potential similar-but-non-damaging votes that could be considered.

## Rationale

### Snapshot.org

Snapshot.org is a web3-native platform for token voting and provides several pre-strategies for accomplishing this, such as 1-to-1 voting and qudratic voting. This platform also provides the capability to create custom voting strategies, which is necessary for Rocket Pool to include staked RPL, for example. The Snapshot.org platform is widely used and does not require fees.

### RPL Stake and Weight

RPL is required to be effectively staked for voting to prevent vote-buying by participants unaffiliated with the protocol's operations.

The algorithm used for calculating power uses a square root and a `0.5` factor for flattening the power contributed by any single node. This helps prevent large node operators from obtaining dominance over the voting process.

### RPL vs rETH Voting

RPL is used in isolation -- not in conjunction with rETH -- because vote-buying is guarded against via staking and node creation is more sybil-resistant. While sybil-resistance is not guaranteed with this RPL-only method, it was chosen as the best compromise among the various options. 

## Security Considerations

As with any token-based voting mechanism, even voting with a power-flattening algorithm such as the one used here allows large actors to influence the outcome of votes. Although this risk is significantly lower with this strategy vs 1-to-1 token votes, it is possible that a large actor may create a proposal which harms the security of the protocol and force it to be adopted via this mechanism.

As of 08-13-2022, the full quorum is computed at 4000 vote power. To singlehandedly pass a governance proposal, an individual or collective would need to amass at least 50.1% of total voting power. Assuming the full quorum of roughly 4,000 voting power participation opposes them and all other votes abstain, a single actor would need to stake just 640,000 RPL across 10 nodes for at least 100 days to reach this threshold. This is well within reach of known protocol participants today as well as many outside actors. **Therefore, this system should be considered insecure and attackable.** The Rocket Pool community and any who c must remain diligent in both its analysis of and participation in proposal votes to prevent governance attacks.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
