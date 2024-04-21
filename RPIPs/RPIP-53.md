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

[Rewards spec v9](https://github.com/rocket-pool/RPIPs/pull/174) is required for this proposal, as it builds on top of v9.


## Motivation

The Redstone update introduced the Merkle-tree-based rewards system, which allowed users to claim RPL they've earned for both node operating duties and Oracle DAO duties, as well as their share of Smoothing Pool rewards. Since its inception, this system has gone through several revisions to account for fixes and Ethereum upgrades but has fundamentally remained the same: the tree lists rewards earned for individual node operators, and node operators can claim those rewards at any time.

On that note, Houston (or more specifically RPIP-31) has an unfortunate consequence: if you have an RPL withdrawal address set, [you can no longer claim rewards intervals](https://github.com/rocket-pool/rocketpool/blob/a747457fc610aa889af0a13cff5d3d00955525a5/contracts/contract/rewards/RocketMerkleDistributorMainnet.sol#L85). This includes both RPL rewards and Smoothing Pool ETH rewards. Claiming them *must* be done from the RPL withdrawal address. Whether intended or not, the RPL withdrawal address isn't really an RPL withdrawal address in Houston; it's a "claimer address" that can claim rewards intervals from the Merkle distributor contract.

This is problematic for a few reasons. As an example: the node is still going to be listed in the rewards file, and thus the rewards for the corresponding interval are still going to show up as a "claimable interval" in the Smart Node despite the user not being able to claim from it, and not being able to actually *receive* any of the rewards from that interval. It will only disappear from the list once the RPL withdrawal address has claimed from it. This can obviously be worked around in the Smart Node but it's exactly that: a workaround. The file itself is still going to have the node operator listed, so anyone (such as a third party) that wants to use it to view or claim rewards has to know about and implement this workaround as well.

In this RPIP, I'm proposing a small change to the rewards file that ameliorates the problems and provides some helpful benefits for people that would actually take advantage of the feature in the first place.


## Important Clarification

Note that the RPL withdrawal address, as specified in RPIP-31, is **not** the same as the "primary withdrawal address" (previously known as just the "withdrawal address" in Atlas and below). If a node operator has a "regular" withdrawal address set, they will *not be affected by this change*. They can still claim rewards normally. The only nodes affected by this change are ones that explicitly set an **RPL withdrawal address**, which is a different setting than the "primary withdrawal address", after Houston has been deployed.


## Specification

The specification is an amendment to v9 specified in [candidate RPIP-52](https://github.com/rocket-pool/RPIPs/pull/174) by Patches. That proposal is **required** to be accepted prior to / at the same time as this one. 

The formal specifications for rewards calculation, tree generation, and the corresponding rewards fileare included in [the assets folder](../assets/rpip-53/). More specifically:
- The [rewards calculation spec](../assets/rpip-53/rewards-calculation-spec.md)
- The [rewards tree spec](../assets/rpip-53/merkle-tree-spec.md)
- The [SSZ rewards file spec](../assets/rpip-53/rewards-file-spec.md)

Here is a capture of what's changed:


### File Structure Changes

- The `node_rewards` section of the rewards file, which lists the rewards earned by individual node operators, has been renamed to the more generic `claimer_rewards` section.
- The `NodeReward` class has been renamed to `ClaimerReward`.
- Both of these are semantic changes that do not impact the SSZ file itself, as field and struct names are not part of the file. Accordingly, tooling can use existing v9 rewards files and new v10 files without modification.

As of this ruleset, **the JSON format for rewards trees is now deprecated** and will likely no longer be produced when v11 inevitably arrives. JSON files can still be produced but for compatibility with the existing files, the fields will not be renamed as described above; they will retain their legacy names. As there is no formal specification for the JSON variant of rewards files, there is nothing to revise with respect to specifications but retaining the field names will at least ensure compatibility with existing tooling that was built around them. This will help ensure a smoother transition as existing tooling moves away from JSON and towards SSZ.


### Calculation Changes

To determine the contents of `claimer_rewards`, here is [the relevant excerpt](../assets/rpip-53/rewards-calculation-spec.md#determining-claimers-and-claimer-rewards) from the rewards calculation spec:

Start by creating a new, empty list of claimers. For each node determined to be eligible for rewards using the above process:
- Determine if the node has an RPL withdrawal address set:
  - ```go
    isRplWithdrawalAddressSet := RocketNodeManager.getNodeRPLWithdrawalAddressIsSet(nodeAddress)
    ```
- If the node **does not** have an RPL withdrawal address set, there will be a single claimer with the following details:
  - The address will be the address of the node.
  - The network will be the rewards network selected by the node. 
  - The RPL rewards will be the amount of RPL earned by the node for this rewards period.
  - The ETH rewards will be the amount of ETH earned by the node for this rewards period.
- If the node **does** have an RPL withdrawal address set, there will be two claimer leaf nodes:
  - One (the "RPL" claimer) will have the following details:
    - The address will be the address of the node's RPL withdrawal address.
    - The network will be the rewards network selected by the node.
    - The RPL rewards will be the amount of RPL earned by the node for this rewards period - the combined rewards for minipool operator and Oracle DAO rewards.
    - The ETH rewards will be 0.
  - One (the "ETH" claimer) will have the following details:
    - The address will be the address of the node's primary withdrawal address.
    - The network will be the rewards network selected by the node.
    - The RPL rewards will be 0.
    - The ETH rewards will be the amount of ETH earned by the node for this rewards period - its share of earnings from the Smoothing Pool.
  - There will not be a claimer for the node itself directly, though its address may incidentally match at least one of the above claimers; in that case it is represented indirectly.
- Add the claimers determined in this step to the cumulative list of claimers:
  - If the claimer had 0 RPL rewards and 0 ETH rewards, ignore it as though it didn't exist and continue.
  - If an entry **does not exist** with the claimer's address and network, add this claimer to the list.
  - If an entry **already exists** with the claimer's address and network, add this claimer's rewards to the existing entry instead.

This condensed list is now the formal list of claimers that will be used to construct leaf nodes in the tree.


## Rationale

This change has some obvious benefits:

1. Nodes without an RPL withdrawal address set at the time of `targetSlot` are unaffected by this change.
2. Nodes that earned rewards during an interval, but have an RPL withdrawal address set, will not be present in the file. This naturally makes the Smart Node interpret that as being "ineligible to claim" for the interval and will thus hide it from the node operator in the list of intervals presented when claiming rewards.
3. The RPL withdrawal address is largely targeted at entities that would take advantage of a splitter contract (e.g., whale marriages) or entities building on top of Rocket Pool (e.g., NodeSet). In the situation where multiple nodes have the same RPL withdrawal address assigned, this reduces the number of transactions required for that claimer to claim rewards from one-per-node to just one. It reduces O(n) gas costs down to O(1).
4. This **does not** require any contract changes. It works with the Houston variant of [`RocketMerkleDistributorMainnet`](https://github.com/rocket-pool/rocketpool/blob/a747457fc610aa889af0a13cff5d3d00955525a5/contracts/contract/rewards/RocketMerkleDistributorMainnet.sol) (which is where it would apply).
   1. The contract doesn't actually check if the claimer is a registered node or not, despite what you might think. It just checks if the claimer has an RPL withdrawal address set or not, which is why this whole thing works in the first place.
   2. Ostensibly, this behavior is more in-line with how the contracts actually function than the v8 / v9 system.
5. In practice I don't expect this will change the size of the rewards tree file much.
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

Aside from variable names, the actual functional changes are very small. It just checks to see if a node has an RPL withdrawal address set, and if so, it directs the RPL earnings to the RPL withdrawal address and the ETH earnings to the primary withdrawal address instead.


## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).