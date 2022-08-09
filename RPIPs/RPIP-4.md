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
- Mike Leach AKA Wander (RPIP Editor, Founder - Bamboo Financial Technologies)
- [others?]

The snapshot vote will run for [14] days and requires a [50.1%] majority to be successful.

### Snapshot Vote Strategy

#### Eligibility

To be eligible for vote participation, the RPL must be staked in the Rocket Pool protocol for at least 24 hours prior to the vote. 

#### Delegation

Eligible RPL voting power may instead be delegated to another wallet. All delegated RPL voting power is added to the total voting power of the voting wallet according to the formula below.

#### RPL Voting 

All RPL votes are recorded into a `results` map which requires a Rocket Pool node address as the index (`address`). The `results` map's values include:
- a boolean `vote` which indicates a vote of "yea" for `true` or "nay" for `false`
- an integer `power` which contains the ultimate voting power of that node

`power` for `results` is determined in accordance with the following equation for voting: 

`power = (int) 0.5 * √rpl * weight`, where `rpl` is the amount of RPL staked in that node, `weight` is the result of the weight calculation (see below), and the result is truncated into an integer.

`weight` for is determined from the age of the node via:

`weight = min( (registrationDate - currentDate) / 100, 1)`

Any delegated vote `power` is summed with the voter's own `power` to achieve the final vote power for a single voter.

Total vote power for `results` is summed into `rplTotalPower`.

#### Outcome

All `power` values from the `rEthResults` map are summed into two integers based on each vote, `rEthYeaPower` and `rEthNayPower`, and all `power` values from the `rplResults` map are summed into two integers based on each vote, `rplYeaPower` and `rplNayPower`.

These are combined based on the following equations:

`yea = rplYeaPower / rplTotalPower`
`nay = .rplNayPower / rplTotalPower`

If `yea` is greater than `nay`, the proposal passes. Otherwise, it fails.

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

[ TBD: analysis of RPL supply distribution and the potential attack vectors - how much RPL would need to be purchased to outweigh all other voters? How many people have that much Ethereum?]

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
