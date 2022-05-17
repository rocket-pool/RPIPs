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

The Snapshot voting mechanism uses quadratic, staked, and weighted RPL token voting and quadratic rETH token voting.

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

### rETH Voting

All votes are recorded into a `rEthResults` map which utilizes an Ethereum address as the index (`address`). The `result` map's values include:
- a boolean `vote` which indicates a vote of "yea" for `true` or "nay" for `false`
- an integer `power` which contains the ultimate voting power of that node

`power` for `rEthResults` is determined via the following equation for quadratic voting:

`power = (int) √rEth` where `rEth` is the amount of rETH contained in `address` and the result truncated into an integer.

#### RPL Voting 

All RPL votes are recorded into a `rplResults` map which requires a Rocket Pool node address as the index (`address`) and is otherwise structured similarly to the RPL `rEthResults` map.

`power` for `rplResults` is determined in accordance with the following equation: 

`power = (int) √rpl * weight`, where `rpl` is the amount of RPL staked in that node, `weight` is the result of the weight calculation (see below), and the result is truncated into an integer.

`weight` for is determined from the age of the node via:

`weight = min( (registrationDate - currentDate) / 100, 1)`

Total vote power for `rplResults` is summed into `rplResultsPower`

#### Outcome

All `power` values from the `rEthResults` map are summed into two integers based on each vote, `rEthYeaPower` and `rEthNayPower`, and all `power` values from the `rplResults` map are summed into two integers based on each vote, `rplYeaPower` and `rplNayPower`.

These are combined based on the following equations:

`yea = .75 * rplYeaPower / rplResultsPower + .25 * rEthYeaPower / rEthResultsPower`

`nay = .75 * rplNayPower / rplResultsPower + .25 * rEthNayPower / rEthResultsPower`

If `yea` is greater than `nay`, the proposal passes. Otherwise, it fails.

## Rationale

### Snapshot.org

Snapshot.org is a web3-native platform for token voting and provides several pre-strategies for accomplishing this, such as 1-to-1 voting and qudratic voting. This platform also provides the capability to create custom voting strategies, which is necessary for Rocket Pool to include staked RPL, for example. The Snapshot.org platform is widely used and does not require fees.

### Quadratic Voting

Quadratic voting is widely used as a more fair way of capturing community sentiment than 1-to-1 token voting. This method lessens the effects of vote buying on outcomes while being relatively simple to understand and engineer. 

### RPL Stake and Weight

RPL votes are weighted based on age of the node so as to prevent short-term nodes from being created and destroyed for the purposes of vote-buying, which would add unnecessary instability to the protocol.

RPL is required to be staked for voting so as to prevent vote-buying by participants unaffiliated with the protocol's operations. 

### RPL to rETH Vote Weighting

RPL is given a higher proportion of voting power because vote-buying is guarded against via staking and node creation is more sybil-resistant.

## Security Considerations

As with any token-based voting mechanism, even quadratic voting allows large actors to influence the outcome of votes. Although this risk is significantly lower with quadratic voting vs 1-to-1 token votes, it is possible that a large actor may create a proposal which harms the security of the protocol and force it to be adopted via this mechanism.

[ TBD: analysis of RPL supply distribution and the potential attack vectors - how much RPL would need to be purchased to outweigh all other voters? How many people have that much Ethereum?]

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
