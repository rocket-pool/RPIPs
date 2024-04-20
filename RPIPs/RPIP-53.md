---
rpip: 53
title: Rewards Tree v10 Specification
description: Adjusts the rewards file to account for Houston changes, generalizing rewards from nodes to claimers and supporting the RPL withdrawal address
author: Joe Clapis (@jcrtp), Jacob Shufro (@jshufro)
discussions-to: https://dao.rocketpool.net/t/rewards-tree-spec-v10/2937
status: Review
type: Protocol
created: 2024-04-19
---


## Abstract

The incoming Houston upgrade introduces [RPIP-31](https://github.com/rocket-pool/RPIPs/blob/main/RPIPs/RPIP-31.md) - the **RPL withdrawal address**. Unfortunately, node operators using this new feature are **no longer able to claim rewards** if they have an RPL withdrawal address. *Only* the RPL withdrawal address is able to claim their rewards. This includes both RPL *and* Smoothing Pool ETH rewards.

This RPIP proposes to generalize the rewards tree, changing *nodes* to *claimers*. The tree will now specify the amount of RPL and Smoothing Pool ETH available to individual claimers, where:
- If a node does not have an RPL withdrawal address set, their node address is the claimer address (same as today).
- If a node *does* have an RPL withdrawal address set, the node's rewards will be split in two:
  - One claimer will be the node's primary withdrawal address, which will have the node's ETH portion of the rewards.
  - The other will be the node's RPL withdrawal address, which will have the node's RPL portion of the rewards.
  - The node itself will no longer appear in the rewards tree.   

This will improve rewards claiming UX, remove some inconsistencies between the existing ruleset and this new feature, make the rewards system more attractive to users that would actually leverage RPL withdrawal addresses, and will not adversely impact nodes that do not use it.

This **does not** require a contract change (ironically, it's more in-line with how the contracts actually behave in Houston) and can be implemented at any time after Houston has launched.


## Motivation

The Redstone update introduced the Merkle-tree-based rewards system, which allowed users to claim RPL they've earned for both node operating duties and Oracle DAO duties, as well as their share of Smoothing Pool rewards. Since its inception, this system has gone through several revisions to account for fixes and Ethereum upgrades but has fundamentally remained the same: the tree lists rewards earned for individual node operators, and node operators can claim those rewards at any time.

On that note, Houston (or more specifically RPIP-31) has an unfortunate consequence: if you have an RPL withdrawal address set, [you can no longer claim rewards intervals](https://github.com/rocket-pool/rocketpool/blob/a747457fc610aa889af0a13cff5d3d00955525a5/contracts/contract/rewards/RocketMerkleDistributorMainnet.sol#L85). This includes both RPL rewards and Smoothing Pool ETH rewards. Claiming them *must* be done from the RPL withdrawal address. Whether intended or not, the RPL withdrawal address isn't really an RPL withdrawal address in Houston; it's a "claimer address" that can claim rewards intervals from the Merkle distributor contract.

This is problematic for a few reasons. As an example: the node is still going to be listed in the rewards file, and thus the rewards for the corresponding interval are still going to show up as a "claimable interval" in the Smart Node despite the user not being able to claim from it, and not being able to actually *receive* any of the rewards from that interval. It will only disappear from the list once the RPL withdrawal address has claimed from it. This can obviously be worked around in the Smart Node but it's exactly that: a workaround. The file itself is still going to have the node operator listed, so anyone (such as a third party) that wants to use it to view or claim rewards has to know about and implement this workaround as well.

In this RPIP, I'm proposing a small change to the rewards file that ameliorates the problems and provides some helpful benefits for people that would actually take advantage of the feature in the first place.


## Important Clarification

Note that the RPL withdrawal address, as specified in RPIP-31, is **not** the same as the "primary withdrawal address" (previously known as just the "withdrawal address" in Atlas and below). If a node operator has a "regular" withdrawal address set, they will *not be affected by this change*. They can still claim rewards normally. The only nodes affected by this change are ones that explicitly set an **RPL withdrawal address**, which is a different setting than the "primary withdrawal address", after Houston has been deployed.


## Specification

The specification is going to be one of two possibilities:
- Either an amendment to v8 specified in [RPIP-51](https://github.com/rocket-pool/RPIPs/blob/main/RPIPs/RPIP-51.md) by Ramana, or
- An amendment to v9 specified in [candidate RPIP-52](https://github.com/rocket-pool/RPIPs/pull/174) by Patches. 

This change itself is orthogonal to the decision of whether or not v9 is ratified, as v9 is a non-functional format change from JSON to SSZ, but the specification *itself* does depend on whether or not v9 is ratified so it can build upon the proper baseline. I'm calling this "v10" because I anticipate v9 will go through and come first. I'm happy to formalize the specification once the decision has been made. In the mean time, I'll propose the changes here at a high-level.

Alternatively I can adjust both the v8 and v9 specs accordingly, just so we have them ready to go. I defer to the community to decide whether or not that's prudent. Anyway, high-level changes for now:


### Header Changes

- The `RewardsFileVersion` will change from 3 to 4.
- The `RulesetVersion` will change from 8 to 9 (or 10 if RPIP-52 is ratified first).


### Body Changes

The `node_rewards` section of the rewards file, which lists the rewards earned by individual node operators, will be replaced by a more generic `claimer_rewards` section. `claimer_rewards` lists the rewards available to be claimed for the interval *by a claimer*, which may or may not be a node operator.

The structures of `node_rewards` and `claimer_rewards` are identical. It's just a name change; purely a semantic difference. The rules for how elements are added to that list, however, are new.

Claimers are added to the `claimer_rewards` list as follows:
- Calculate the earnings (RPL and Smoothing Pool) of each node in the network normally.
- For each node, determine the `claimer` entities:
  - If the node does *not* have an RPL withdrawal address set at the `targetSlot` of the interval (the slot at which rewards are calculated), there will be one `claimer`: the node address. This is the same as it is today, so if a node doesn't use the RPL withdrawal address feature then its experience is unchanged.
  - If a node *does* have an RPL withdrawal address set at the `targetSlot`, the node's rewards will be split into two claimers:
    - One `claimer` will be the node's primary withdrawal address. The Smoothing Pool ETH portion of the node's rewards will be added to this claimer.
    - One `claimer` will be the node's RPL withdrawal address. The RPL portion of the node's rewards (both staking and Oracle DAO) will be added to this claimer.
- Add the `claimer` entities produced to the `claimer_rewards` list. If the `claimer` does not exist in the list, it is created. If it already exists, the `claimer`'s rewards are added to the existing entry instead. Note there is **one entry per claimer** in the list.

To be clear, "whether or not an RPL withdrawal address is set" looks specifically at the value returned by the [`RocketNodeManager.getNodeRPLWithdrawalAddressIsSet`](https://github.com/rocket-pool/rocketpool/blob/a747457fc610aa889af0a13cff5d3d00955525a5/contracts/contract/node/RocketNodeManager.sol#L121) contract call. It doesn't compare the RPL withdrawal address with the node address or the primary withdrawal address; it simply keys off of the boolean returned by that view.


## Rationale

This change has some obvious benefits:

1. Nodes without an RPL withdrawal address set at the time of `targetSlot` are unaffected by this change.
2. Nodes that earned rewards during an interval, but have an RPL withdrawal address set, will not be present in the file. This naturally makes the Smart Node interpret that as being "ineligible to claim" for the interval and will thus hide it from the node operator in the list of intervals presented when claiming rewards.
3. The RPL withdrawal address is largely targeted at entities that would take advantage of a splitter contract (e.g., whale marriages) or entities building on top of Rocket Pool (e.g., NodeSet). In the situation where multiple nodes have the same RPL withdrawal address assigned, this reduces the number of transactions required for that claimer to claim rewards from one-per-node to just one. It reduces O(n) gas costs down to O(1).
4. This **does not** require any contract changes. It works with the Houston variant of [`RocketMerkleDistributorMainnet`](https://github.com/rocket-pool/rocketpool/blob/a747457fc610aa889af0a13cff5d3d00955525a5/contracts/contract/rewards/RocketMerkleDistributorMainnet.sol) (which is where it would apply).
   1. The contract doesn't actually check if the claimer is a registered node or not, despite what you might think. It just checks if the claimer has an RPL withdrawal address set or not, which is why this whole thing works in the first place.
   2. Ostensibly, this behavior is more in-line with how the contracts actually function than the v8 / v9 system.
5. In the "worst case", where everybody has a unique RPL withdrawal address and a unique primary withdrawal address, the size would be doubled. In the "best case", where everybody has the RPL withdrawal address set to the same address, the tree would only consist of two entries. I expect this proposal won't appreciably change the size of the tree in practice since a majority of node operators will likely not set the RPL withdrawal address.
6. The implementation is actually quite simple. I was able to put the reference implementation together in like half an hour. I wouldn't have any contention about making this the first ruleset after Houston launches once it's cleared the usual cross-implementation testing gamut.

With that being said, there are some important implications that need to be considered:

1. This effectively "locks in" the rewards for an interval to a particular claimer. If you have an RPL withdrawal address set at the time the interval is created, and the address later changes your node's RPL withdrawal address to something else, the new address won't be able to claim the rewards. Only the original RPL withdrawal address will.
   1. This might actually be considered a benefit rather than a drawback, depending on your point of view, but it's worth mentioning nonetheless.
2. In Atlas (as well as in Houston without this proposal), you can use the "Claim and Stake" feature to claim your RPL rewards and restake them for your node within the same operation to save on gas. With this system, if a claimer is an RPL withdrawal address and not a node operator, **it will not be able to stake**. It can only claim. This is because the distributor contract wasn't explicitly designed with this system in mind. It doesn't preclude claiming from non-node-operators thanks to [a backwards compatibiliy quirk](https://github.com/rocket-pool/rocketpool/blob/a747457fc610aa889af0a13cff5d3d00955525a5/contracts/contract/RocketStorage.sol#L107), but it does mean we'll have to do a little cleaning up (both semantically and in terms of revert messages) in Saturn.
   1. On a related note, if a node has an RPL withdrawal address set, the node itself will no longer be able to claim rewards; ETH rewards must be claimed by the node's primary withdrawal address, and RPL rewards must be claimed by the node's RPL withdrawal address.
3. If the node has an RPL withdrawal address set but the primary withdrawal address *unset* (i.e., the node's primary withdrawal address is the node itself), the RPL withdrawal address will have to claim both the ETH rewards *and* the RPL rewards in two separate transactions. The node will have to trust the RPL withdrawal address to do so on its behalf. 
4. If, in some unusual setup, a claimer is both a node address *and* an RPL withdrawal address *for another node*, the claimer will be able to claim and stake both their own rewards and the rewards from the other node onto their own claimer node. This makes the behavior a little more obtuse in odd scenarios such as that so it will have to be documented properly and explained in the Smart Node.
5. If you use these files to calculate your node's total earnings in a vacuum (not total *claimable* earnings, just total earnings produced in general) and you have an RPL withdrawal address set, this will break your workflow. You'll have to calculate your RPL earnings out of band and look at the minipool performance files to determine your Smoothing Pool ETH earned. This is probably more of a dashboard concern than an individual node operator concern.


## Reference Implementation

I've implemented v10 into a Smart Node branch [here](https://github.com/rocket-pool/smartnode/tree/rewards_v10). For comparison, here are the differences between v8 and v10:

- [For the vanilla (non-rolling) generator](https://www.diffchecker.com/WeurMZDP/)
- [For the rolling record generator](https://www.diffchecker.com/WomyRLyw/)

Aside from variable names, the actual functional changes are very small. It just checks to see if a node has an RPL withdrawal address set, and if so, it directs the earnings to the RPL withdrawal address instead of the node address.


## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).