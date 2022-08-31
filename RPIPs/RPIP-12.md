---
rpip: 12
title: Atlas Update
description: Describes the features that will be included in the upcoming Atlas update.
author: Valdorff (@Valdorff), Kane Wallman (@kanewallman)
discussions-to: TODO
status: Draft
type: Protocol
category: Core
created: 2022-08-31
requires: 8
---

## Abstract
The team has been working on several features for the upcoming Atlas update. We are looking to
define the high level features that the community is looking for as part of this update.

As there are multiple significant features, they'll each get their own subsection within
the Specification, Implementation, and Security Consideration sections.

Atlas will implement the following high-level features:
- LEB8s
- Scaling assignments with deposit size
- Efficiently using queue ETH
- SaaS
- Minor cleanup

## Specification

### LEB8s
Please see [RPIP-8](RPIP-8.md). Note that a vote on RPIP-12 (this RPIP) includes RPIP-8, since that
is one of the several features specified.



### 

## Implementation

### LEB8s
- Migration
  - An existing minipool (16 ETH) may be migrated to an LEB8 immediately.
  - The NO will receive a credit for the difference (8 ETH), which they can use to create future
    minipool(s). Note that creating another minipool is subject to the usual queueing mechanism.
  - While RPIP-8 recommended prioritizing higher commission migration, there was no clear way to do
    this, so it's not being implemented.
  - Minipools with < 32 ETH balance SHALL be prevented from migrating to LEB8s
  - Tracking existing rewards
    - In order to enable migration, we must track what rewards pre-existed on the minipool.
    - There will be a single merkle tree generated at the time of contract deployment. By migrating, 
      an NO forfeits rewards since the tree was generated.
      - Note that this merkle tree _can_ be ugraded with a new contract
      - Note that once rewards skimming is available, we will be able to obsolete this method and
        use a simple migration (because there will be no pre-existing rewards to track).
  - Migration will be _one way_. To prevent rollback abuse, `UserDepositBalance` will be set to a
    high value in its existing storage slot and moved to a new location for the new delegate
    contract; this means if an NO _did_ roll back, their "share" would always be zero.
- Basing RPL stake on Protocol ETH
  - We will track a new `matchedETH` value and use it for `rocketNodeStaking.function`
    - The functions to be updated are `getNodeEffectiveRPLStake`, `getNodeMinimumRPLStake`, and
      `getNodeMaximumRPLStake`
    - `matchedETH` will be initialized to 0
    - When the minipool count is incremented or decremented, if `matchedETH` is 0, it will be set
      properly by using `16 * rocketMinipoolManager.getNodeStakingMinipoolCount` to calculate what
      it was before the increment/decrement and then adjusting appropriately
    - If `matchedETH` is 0 in use, we will fall back to
      `16 * rocketMinipoolManager.getNodeStakingMinipoolCount` which will get the correct value b/c
      all pre-existing minipools have had 16
- Reward distribution will be averaged across a node, instead of being tracked per minipool
  ```
  collateralRatio = protocolSupplied / (nodeSupplied + protocolSupplied)
  userBalance = balance * collateralRatio
  nodeShare = (balance - userBalance) + (userBalance * averageNodeFee)
  userShare = balance - nodeShare
  ```
  - The effect of this is that in a node with both 16-ETH minipools and LEB8s, rewards will be split
    the same regardless of which specific minipool got the rewards. This will be most noticeable for
    rare and high-value events like block proposals and sync committees.
- Queues
  - A new flexible size queue will be created. This will be used to service both LEB8s and future
    16 ETH minipools.
  - The existing queues will have priority over the flexible queue. There will be no way to enter
    these queues anymore, so this is simply a way to empty the legacy queues.

See also the discussion at https://dao.rocketpool.net/t/leb8-discussion-thread/899.

## Security Considerations

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
