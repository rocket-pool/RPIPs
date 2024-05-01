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

The incoming Houston upgrade introduces [RPIP-31](https://github.com/rocket-pool/RPIPs/blob/main/RPIPs/RPIP-31.md) - the **RPL withdrawal address**. This is a supplementary setting, different from the "regular" withdrawal address used in Atlas and below, that node operators can opt into. It's intended to specify a "secondary" withdrawal address that the node's RPL rewards will be sent to, whereas the node's ETH rewards will be sent to its "regular" withdrawal address (known in Houston as a **primary** withdrawal address).

Realistically, most node operators won't end up using this feature. It's only really helpful for node operators that don't supply their own RPL when creating a minipool. If you don't fall into this camp, rest assured that *this proposal will have no impact on you*.

Unfortunately, node operators that *do* use this new feature can **no longer trigger rewards claims** from the Merkle rewards system. More specifically they can distribute minipool rewards from the Beacon Chain, but can't claim the rewards produced every 28 days via the Merkle rewards system. That's not a problem for RPL rewards, since the RPL withdrawal address gets those anyway, but the issue is this includes Smoothing Pool ETH rewards as well. Neither the node nor the address that would get the Smoothing Pool ETH (the primary withdrawal address) can actually trigger a claim; *only* the RPL withdrawal address will be able to do it.

This can't be fixed for nodes that opt into setting an RPL withdrawal address if they also use their node as their primary withdrawal address because of technical reasons described in the [Rationale](#rationale) section of the RPIP. However, it *can* be fixed for nodes that opt into it and use an address *other* than their node as the primary withdrawal address. I anticipate that most nodes opting into the RPL withdrawal address feature will fall into this camp.

This RPIP proposes to achieve this by generalizing the rewards tree, changing *nodes* to *claimers*. The tree will now specify the amount of RPL and Smoothing Pool ETH available to individual claimers, where:
- If a node does not have an RPL withdrawal address set, **or** if it has one but its primary withdrawal address is the same as the node address, their node address is the claimer address (same as today).
- If a node has an RPL withdrawal address set and its primary withdrawal address is different from the node address, the node's rewards will be split in two:
  - One claimer will be the node's primary withdrawal address, which will have the node's ETH portion of the rewards.
  - The other will be the node's RPL withdrawal address, which will have the node's RPL portion of the rewards.

This will improve rewards claiming UX, remove some inconsistencies between the existing ruleset and this new feature, make the rewards system more attractive to users that would actually leverage RPL withdrawal addresses, and will not adversely impact nodes that do not use it.

This **does not** require a contract change (ironically, it's more in-line with how the contracts actually behave in Houston) and can be implemented at any time, even before Houston has launched if relevant. The spec handles what to do if it hasn't been deployed yet.

[Rewards spec v9](https://github.com/rocket-pool/RPIPs/pull/174) is required for this proposal, as it builds on top of v9.


## Motivation

The Redstone update introduced the Merkle-tree-based rewards system, which allowed users to claim RPL they've earned for both node operating duties and Oracle DAO duties, as well as their share of Smoothing Pool rewards. Since its inception, this system has gone through several revisions to account for fixes and Ethereum upgrades but has fundamentally remained the same: the tree lists rewards earned for individual node operators, and node operators can claim those rewards at any time.

Houston (or more specifically RPIP-31) introduces a new feature, the **RPL withdrawal address**, that changes those fundamentals.

In Atlas, Rocket Pool sends all of the node's rewards (both from the Beacon Chain during a `minipool distribute` and from the Merkle rewards system via `node claim-rewards`) to the node's withdrawal address. In Houston, this is having its name changed to **primary withdrawal address**. The new RPL withdrawal address is a supplemental, opt-in setting that allows a node to specify a "secondary" withdrawal address. When set, the intent is that the *primary* withdrawal address will receive all of the node's ETH rewards, and the *RPL* withdrawal address will receive all of the RPL rewards.

While making sense on paper, the implementation has an unfortunate consequence. If you're a node operator and you have an RPL withdrawal address set, [you can no longer claim rewards](https://github.com/rocket-pool/rocketpool/blob/a747457fc610aa889af0a13cff5d3d00955525a5/contracts/contract/rewards/RocketMerkleDistributorMainnet.sol#L85). This doesn't affect Beacon Chain earnings, but does impact both RPL rewards and Smoothing Pool ETH rewards. Claiming them *must* be done from the RPL withdrawal address. Whether intended or not, the RPL withdrawal address isn't just an RPL withdrawal address in Houston; it's also a "claimer address" that can exclusively claim rewards intervals from the Merkle distributor contract.

This is problematic for a few reasons.
- First, it means the RPL withdrawal address has dominion over claiming Smoothing Pool ETH that it won't ever recieve. The node (or more accurately the node's primary withdrawal address), which *will* receive the ETH, can't trigger the claim.
- Second, the node is still going to be listed in the rewards file, and thus the rewards for the corresponding interval are still going to show up as a "claimable interval" in the Smart Node despite the user not being able to claim from it, and not being able to actually *receive* any of the rewards from that interval. This can obviously be worked around in the Smart Node but it's exactly that: a workaround. The file itself is still going to have the node operator listed, so anyone (such as a third party) that wants to use it to view or claim rewards has to know about and implement this workaround as well.

In this RPIP, I'm proposing a small change to the rewards file that ameliorates the problems when possible and provides some helpful benefits for people that would actually take advantage of the feature in the first place.


## Important Clarification

To be clear, the RPL withdrawal address is **not** the same setting as the primary withdrawal address. If a node has a "regular" withdrawal address set in Atlas, it *will not immediately be affected by this change*. Its current withdrawal address will become its primary withdrawal address and it can still claim rewards normally.

The only nodes affected by this change are ones that explicitly set an **RPL withdrawal address** after Houston has been deployed, which is a different setting than the primary withdrawal address.


## Specification

The specification is an amendment to v9 specified in [candidate RPIP-52](https://github.com/rocket-pool/RPIPs/pull/174) by Patches. That proposal is **required** to be accepted prior to / at the same time as this one. 

The formal specifications for rewards calculation, tree generation, and the corresponding rewards file are included in [the assets folder](../assets/rpip-53/). More specifically:
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
  ```go
  isRplWithdrawalAddressSet := RocketNodeManager.getNodeRPLWithdrawalAddressIsSet(nodeAddress)
  ```
  - *(Note: if this function is not present in `RocketNodeManager`, then the network deployment has not been upgraded to Houston yet. Set `isRplWithdrawalAddressSet` to `false`.)*
  - If set, get the RPL withdrawal address:
    ```go
    rplWithdrawalAddress := RocketNodeManager.getNodeRPLWithdrawalAddress(nodeAddress)
    ```
- Determine the node's primary withdrawal address:
  ```go
  primaryWithdrawalAddress := RocketStorage.getNodeWithdrawalAddress(nodeAddress)
  ```
- Determine the node's rewards network setting:
  ```go
  rewardNetwork := RocketNodeManager.getRewardNetwork(nodeAddress)
  ```
- If `isRplWithdrawalAddressSet` is `false`, **or** if `primaryWithdrawalAddress` is equal to `nodeAddress`, there will be a single claimer:
  - The address will be `nodeAddress`.
  - The network will be `rewardNetwork`. 
  - The RPL rewards will be the amount of RPL earned by the node for this rewards period.
  - The ETH rewards will be the amount of ETH earned by the node for this rewards period.
- For all other nodes, there will be two claimers:
  - One (the "RPL" claimer) will have the following details:
    - The address will be `rplWithdrawalAddress`.
    - The network will be `rewardNetwork`.
    - The RPL rewards will be the amount of RPL earned by the node for this rewards period - the combined rewards for minipool operator and Oracle DAO rewards.
    - The ETH rewards will be 0.
  - One (the "ETH" claimer) will have the following details:
    - The address will be `primaryWithdrawalAddress`.
    - The network will be `rewardNetwork`.
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
2. Nodes that earned rewards during an interval, but have an RPL withdrawal address set and a primary withdrawal address different from their node, will not be present in the file. This naturally makes the Smart Node interpret that as being "ineligible to claim" for the interval and will thus hide it from the node operator in the list of intervals presented when claiming rewards.
3. The RPL withdrawal address is largely targeted at entities that would take advantage of a splitter contract (e.g., whale marriages) or entities building on top of Rocket Pool (e.g., NodeSet). In the situation where multiple nodes have the same RPL withdrawal address assigned and their primary withdrawal addresses differ from their node addresses, this reduces the number of transactions required for that claimer to claim rewards from one-per-node to just one. It reduces O(n) gas costs down to O(1).
4. This **does not** require any contract changes. It works with the Houston variant of [`RocketMerkleDistributorMainnet`](https://github.com/rocket-pool/rocketpool/blob/a747457fc610aa889af0a13cff5d3d00955525a5/contracts/contract/rewards/RocketMerkleDistributorMainnet.sol). If Houston isn't deployed yet, it works fine with the Atlas variant since the behavior doesn't change from v8 and v9.
   1. The contract doesn't actually check if the claimer is a registered node or not, despite what you might think. It just checks if the claimer has an RPL withdrawal address set or not, which is why this whole thing works in the first place.
   2. Ostensibly, this behavior is more in-line with how the contracts actually function than the v8 / v9 system.
5. In practice I don't expect this will change the size of the rewards tree file much.
6. The implementation is actually quite simple. I was able to put the reference implementation together in like half an hour. I wouldn't have any contention about enacting this at the same time as v9 (even if that occurs before Houston), as long as it's passed the testing gamut.

With that being said, if you have an RPL withdrawal address set and a primary withdrawal address different from your node at the time of `targetSlot`, there are some important implications that need to be considered:

1. You will no longer be able to claim rewards via your node for that interval. Only your primary withdrawal address can claim your ETH rewards and only your RPL withdrawal address can claim your RPL rewards. In other words, this effectively "locks in" the rewards for an interval to your primary and RPL withdrawal addresses. They're going to be the only addresses that can claim the rewards for that interval, even if your withdrawal addresses change in the future or if you unset the RPL withdrawal address.
2. Your RPL withdrawal address will not be able to use the "Claim and Stake" function introduced in Atlas to stake RPL on behalf of your node within a single transaction. It will only be able to claim its RPL.
3. Setting the RPL withdrawal address to a node address may have odd, and sometimes not immediately obvious, consequences due to the way rewards are consolidated. **Be very careful if you intend to do this.**
4. If you use these files to calculate your node's total earnings in a vacuum (not total *claimable* earnings, just total earnings produced in general), this will break your workflow. You'll have to calculate your RPL earnings out of band and look at the minipool performance files to determine your Smoothing Pool ETH earned. This is probably more of a dashboard concern than an individual node operator concern.

As a note, the rationale for the **or** condition in the logic is to mitigate an incentivization conflict. If a node without a primary withdrawal address followed the same "breakout" rule that resulted in two sets of claimers, it wouldn't actually be able to claim its own ETH rewards; it has an RPL withdrawal address set, and that's what the contracts key on when determining whether or not a node is eligible to claim. The RPL withdrawal address would have to claim for that node twice: once for itself (the RPL portion) and once for the node's ETH. However, the RPL withdrawal address is actually *disincentivized* from triggering the second claim; it doesn't receive anything from doing so, *and* it has to pay for the gas cost. Thus, to mitigate the incentive issue, nodes with an RPL withdrawal address set but no primary withdrawal address set are excluded from the split-claimer logic.

While this condition is necessary for Houston, it could be revised or even eliminated in Saturn if the Protocol DAO expresses interest in fixing it at the contract level.


## Reference Implementation

I've implemented v10 into a Smart Node branch [here](https://github.com/rocket-pool/smartnode/tree/rewards_v10). For comparison, here are the differences between v8 and v10:

- [For the vanilla (non-rolling) generator](../assets/rpip-53/v8-v10-vanilla-diff.html)
- [For the rolling record generator](../assets/rpip-53/v8-v10-rolling-diff.html)

Aside from variable names, the actual functional changes are very small. It just checks to see if a node has an RPL withdrawal address set, and if so, it directs the RPL earnings to the RPL withdrawal address and the ETH earnings to the primary withdrawal address instead.


## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).