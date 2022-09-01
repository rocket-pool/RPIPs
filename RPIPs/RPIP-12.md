---
rpip: 12
title: Atlas Update
description: Describes the features that will be included in the upcoming Atlas update.
author: Valdorff (@Valdorff), Kane Wallmann (@kanewallmann)
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
- Use queue ETH
- Include queue capacity in maximum deposit size
- Scale assignments with deposit size
- SaaS
- Minor cleanup

## Specification

### LEB8s
Please see [RPIP-8](RPIP-8.md). Note that a vote on RPIP-12 (this RPIP) includes RPIP-8, since that
is one of the several features specified.

### Use queue ETH

Right now the minipool queue holds a fair amount of idle ETH. Instead, that ETH could be used to 
start validators and earn rewards
- When a minipool is created
  - The minimum beacon chain deposit (1 ETH) SHALL be made
  - All remaining ETH SHALL be used to pair against the queue
  - The protocol MUST be able to calculate the amount of ETH in use from the queue; this ETH SHALL
    not be included in the ETH balance used for calculating `RocketTokenRETH.getEthValue` or
    `RocketTokenRETH.getRethValue` 

Notes:
- This means that more validators will be started than would otherwise be possible given the minted
rETH. More NOs will be earning rewards, and some of those rewards will be split to rETH holders.
  - The effect is similar, but directionally opposite to a full deposit pool; rather than being a
    small drag on rETH apr, this will provide a small boost.
- As a benefit, this means that all minipools in the queue will need 31 ETH to launch, regardless
  of how many ETH were deposited. This fact can simplify several pieces of code.

### Include queue capacity in maximum deposit size
- The maximum deposit size SHALL be the sum of:
  - The total space available in the deposit pool
  - The total capacity available in the minipool queue

### Scale assignments with deposit size
- The number of scaling assignments SHALL be the minimum of:
  - 90
  - The number of minipools that can be assigned using the deposit
- The number of socialized assignments SHALL be 2
- The total number of assingments from a deposit SHALL be the minimum of:
  - Scaling assignments + socialized assignments
  - The total queue length

### SaaS

### Minor cleanup
- The Full deposit option SHALL be removed
- Existing queued Half and Full deposit minipools SHALL be assigned before assigning any minipools
  that are created after the Atlas smart contract is in effect

### Removal of total effective RPL stake
With the redstone upgrade replacing the old reward system, we no longer require the total effective RPL stake value. It also removes the requirement for the blocker we currently have on creating/finalising minipools while network is "not in consensus". It is a low effort change to simplify the protocol reducing gas and improving UX with the following changes:

- Total effective RPL stake SHALL be removed from oracle DAO submissions
- All total effective RPL stake calculations and checks SHALL be removed from the smart contracts

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

### Use queue ETH

### Include queue capacity in maximum deposit size

### Scale assignments with deposit size

### SaaS

### Minor cleanup

### Removal of total effective RPL stake
- Remove `_effectiveRplStake` argument from `rocketNetworkPrices.submitPrices` and `rocketNetworkPrices.executePrices` methods
- Remove `inConsensus` method from `rocketNetworkPrices`
- Remove `updateTotalEffectiveRPLStake` method from `rocketMinipoolManager`
- Remove calls to `updateTotalEffectiveRPLStake` from `decrementNodeStakingMinipoolCount` and `incrementNodeStakingMinipoolCount`
- Remove calls to `updateTotalEffectiveRPLStake` in methods `slashRPL`, `withdrawRPL`, `_stakeRPL` in `rocketNodeStaking`
- Remove `calculateTotalEffectiveRPLStake` and `getTotalEffectiveRPLStake` in `rocketNodeStaking`
- Update smartnode software to no longer calculate and submit the value for `_effectiveRplStake`

## Security Considerations

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
