---
rpip: 71
title: rETH Withdrawal liquidity via EIP-7002
description: Enable Rocket Ether stakers to trigger validator exits to access protocol liquidity for unstaking from Rocket Pool.
author: knoshua (@knoshua)
discussions-to: <URL> TODO
status: Draft
type: Protocol
category: Core
created: 2025-05-14
requires (*optional): 44
---

## Abstract

rETH is fully backed by staked ether but currently lacks a direct mechanism for stakers to signal a desire to exit. With the adoption of [EIP-7002](https://eips.ethereum.org/EIPS/eip-7002), Rocket Pool can implement execution-layer-triggered exits that respond to explicit rETH withdrawal requests without requiring coordination with node operators.

This RPIP specifies a Withdrawal Queue and validator selection that avoids introducing a new oDAO duty or a challenge mechanism.
## Motivation

rETH currently exposes stakers to liquidity risk: although their stake can in principle be withdrawn from the beacon chain, there is no direct protocol mechanism by which rETH holders can access this stake. Instead, they must rely on limited in-protocol liquidity or sell rETH on the open market. This creates a dependence on secondary market liquidity and can make rETH less attractive especially for larger investors and users with time-sensitive liquidity needs.

## Specification

This specification introduces the following pDAO protocol parameters:

| Name                             | Type | Initial Value | Guardrail<br> |
| -------------------------------- | ---- | ------------- | ------------- |
| `deposit_pool_collateral_target` | pct  | 1             |               |
| `staking_delay`                  | Days | 28            | < 7           |
| `tournament_size`<br>            |      | 4             | < 20          |


This specification changes the following pDAO protocol parameters:

| Name                             | Type | Current Value | New Value |
| -------------------------------- | ---- | ------------- | --------- |
| `network.reth.collateral.target` | pct  | 1             | 0         |

### Withdrawal Queue

- A Withdrawal Queue contract MUST exist to provide the interface by which pool stakers request exits at the protocol rate.
- Starting 28 days after the upgrade that implements this RPIP, the Withdrawal Queue MUST accept rETH deposits from users to create withdrawal requests.
- The ETH value of the rETH at the time of a withdrawal request MUST be recorded.
- Canceling a withdrawal request and exiting the queue SHALL NOT be possible.

### Fulfilling Withdrawal Requests

- While the Withdrawal Queue is not empty:
	- ETH from new rETH mints SHALL first go towards the Withdrawal Queue.
	- Redemption of rETH outside the queue SHALL be prevented as far as possible.
	- The protocol SHALL allow the Withdrawal Queue to burn rETH.
- `network.reth.collateral.target` SHALL be set to 0 and a new buffer for withdrawals SHALL be implemented in the deposit pool, reserving up to `deposit_pool_collateral_target` percent of ETH backing rETH for rETH burns.
- When megapools distribute rewards, they SHALL send ETH to the deposit pool rather than rETH.
- When a withdrawal request is fulfilled, the Withdrawal Queue SHALL burn the corresponding rETH. The user SHALL receive the stored ETH value at the time of the request or the value at time of rETH burn, whichever is smaller.
- Any remaining ETH corresponding to that rETH burn SHALL be transferred to the deposit pool.

### Tracking Exiting ETH

- Expected user capital is defined as `32 ETH - validator bond`. (This should be fixed under bond curve changes.)
- When `notifyExit` is called for a megapool validator that wasn't requested to exit, a variable `exiting_eth` SHALL be increased by the expected user capital of that validator.
- When `notifyFinalBalance` is called for a megapool validator that wasn't requested to exit, `exiting_eth` SHALL be decreased by the expected user capital. If the megapool validator was requested to exit, `requested_eth` SHALL be decreased instead.

### Validator Exit Selection

For the purposes of this section, the withdrawal shortfall is defined as the remaining ETH required to fully satisfy all pending Withdrawal Queue requests after accounting for:
- `requested_eth` (as defined in RPIP‑80),
- `exiting_eth`,
- ETH held by the rETH contract, and
- ETH held by the deposit pool.

If the withdrawal shortfall is at least 32 ETH, additional megapool validators MUST be selected and added to the exit list as defined in this section.

#### Eligible megapool validator set

With the upgrade that implements this proposal, the protocol SHALL initialize a set of eligible megapool validators from which validators can be selected for exit under this proposal. A megapool validator MUST satisfy all of the following conditions in order to be included in this set:
- `stake` was already called for the validator.
- The validator is not already exiting.

After the upgrade that implements this RPIP:
- When `stake` is called for a megapool validator, the protocol SHALL schedule the addition of that validator to the eligible megapool validator set after a pDAO‑configurable `staking_delay`.
- When an exit request is made for a megapool validator under RPIP‑80, that validator MUST be removed from the eligible megapool validator set.
- When `notifyExit` is called for a megapool validator that is still in the eligible megapool validator set, that validator MUST be removed from it.

#### Tournament-based selection

Let `N` be the current size of the eligible megapool validator set. Let `k` be `tournament_size` (or `N` if `tournament_size > N`). The protocol SHALL allow to repeatedly select validators while:
- the withdrawal shortfall is at least 32 ETH, and
- the eligible megapool validator set is non‑empty.

1. The protocol MUST sample `k` distinct validators uniformly at random without replacement from the eligible megapool validator set.
2. For each sampled validator, the protocol MUST compute an exit score according to the chosen Exit Criterion.
3. The protocol MUST select exactly one validator from the sampled set to request for exit: the validator that is determined to be the highest priority to exit according to the Exit Criterion.
4. The protocol SHALL request this validator to exit as defined by RPIP-80.

## Rationale

### Withdrawal Queue

The Withdrawal Queue opens 28 days after the upgrade containing this RPIP is deployed to give node operators an opportunity to upgrade to the new delegate and update smartnode to a version that supports exit request fulfillment. It also aims to ensure that validators in the eligible set are actively staking.

The protocol is potentially exiting validators to fulfill a withdrawal request. If users could cancel withdrawal requests, they can trigger unneeded exits and reduce rETH APR.

Changing `network.reth.collateral.target` from 1 to 0 is needed so that ETH from exiting validators flows into the deposit pool rather than remaining as excess collateral in the rETH contract. Including Withdrawal Queue demand in `RocketDepositPool.getExcessBalance()` ensures that ETH freed from exits and new deposits can preferentially be used to satisfy withdrawal requests, rather than being available for rETH burns outside the queue.

The redemption rate at which a request is fulfilled is chosen as the minimum between rate at time of request and time of fulfillment. rETH in the Withdrawal Queue cannot keep earning staking rewards or there would be an incentive to always keep rETH in the queue and mint new rETH once it can be fulfilled, as this improves liquidity of the position without losing rewards. At the same time, being in the Withdrawal Queue should not protect against rETH losing value in extreme slashing scenarios.

### Fulfilling Withdrawal Requests

Since the rETH contract is not upgradeable and has built-in redemption via the `burn` method, ensuring that the Withdrawal Queue gets prioritized requires a bit of thought.

Setting the collateral target to 0 ensures that fully withdrawn minipool user ETH goes to the deposit pool. Reward distributions from old minipool delegates will still stay in the rETH contract and can be used to redeem rETH outside the Withdrawal Queue, this is why the specification calls for "as far as possible".

rETH also uses ETH from the deposit pool for `burn`, based on what `RocketDepositPool.getExcessBalance()` returns. The Deposit Pool contract can be upgraded, but we need to ensure that rETH burns are possible for the Withdrawal Queue and at the same time rejected when someone other than the Withdrawal Queue attempts to burn. This may involve setting a state flag in the deposit pool to change the behavior of `RocketDepositPool.getExcessBalance()` during a burn transaction from the Withdrawal Queue.

## Validator Exit Selection

The withdrawal shortfall is defined in terms of `requested_eth`, `exiting_eth`, and the ETH held by the rETH contract and deposit pool so that the selection mechanism only triggers when existing sources of liquidity are insufficient to satisfy the Withdrawal Queue as far as feasible (for example, ETH in minipool contracts is not included here). Requiring additional exits only when the shortfall is at least 32 ETH ensures that the mechanism is not forced to exit a whole validator to cover a residual amount that can reasonably be filled by future rewards or natural inflows.

How to initialize the eligible megapool validator set at upgrade time is left as an implementation detail.

The `staking_delay` applied after the `stake` call is meant to ensure that validators are only added to the eligible set after they made it through the beacon chain queue, to avoid requests that would take a long time to be executed. This also protects new validators from immediately being exited.

The tournament‑based mechanism is chosen because maintaining a full global ordering of all eligible validators by the Exit Criterion may not be feasible. By sampling a small, random subset of eligible validators and exiting the validator with the worst Exit Criterion score among that subset, the protocol achieves a decent approximation of full ordering according to the Exit Criterion. For the initial `tournament_size = 4`, on average we will select someone around the 20%-percentile according to the Exit Criterion (https://gist.github.com/knoshua/c71b78f5b2888b1b10800a0b8c4f45c5).

## Security Considerations

### Fulfilling Withdrawal Requests and rETH Updates

Without further changes to the oDAO balance tracking duty that informs the rETH exchange rate, the proposed mechanism leads to a re-distribution of staking rewards from rETH in the Withdrawal Queue to other rETH holders at the time a withdrawal request is fulfilled. Potentially, this could lead to a very large rETH rate increase with the following update that could become "sandwichable": someone could mint a large amount of rETH right before the update and burn it right after to make a profit at the expense of rETH holders.
However, rETH protects against this type of exploit with a pDAO configurable rETH mint fee that is currently set to 0.05%. This means for example that if 30% of rETH supply are in the Withdrawal Queue for 2 weeks, the resulting rETH rate update still could not be profitably sandwiched when rETH yield for that day is approximately average. If a large request fulfillment coincides with a high yield day for rETH, the threshold for sandwiching to become profitable is lower. For example a day with an 80 ETH MEV block together with 20% of rETH supply in the queue for two weeks can be sandwiched.
If the pDAO wants to protect against even more extreme scenarios, the mint fee could be slightly increased.

### Protection Against Repeated Minting and Withdrawing

A concern is that a malicious actor could lower the yield of rETH by repeatedly minting and withdrawing rETH, because this would put a share of staked ETH constantly into an unproductive state. The design makes this kind of attack both costly and mostly ineffective. 

There already is a mint fee on rETH, so an attacker would (currently) lose 0.05% per round trip. They also would not earn any yield, since they would get the minimum rate between the time they entered queue and the time their withdrawal request is filled.

Furthermore, the new withdrawal buffer in the deposit pool means that a repeatable attack requires more capital than the buffer size (controlled by `deposit_pool_collateral_target`) and only the amount above the buffer size can create validator churn.


## Open Questions

### Withdrawal Queue: NFT and Partial Filling

A position in the Withdrawal Queue could be represented by an NFT. Since the specification does not allow leaving the Withdrawal Queue by canceling a request, users could instead sell their NFT to immediately liquidate their position in the queue.
On the other hand, people that don't want to wait through the Withdrawal Queue already have the option to sell rETH instead, it may be more difficult to find buyers for a non-fungible queue position, and implementing the NFT would create extra gas overhead (~100k gas?) for everyone.

Another question is if users in queue should be able to do partial withdrawals as ETH becomes available. It may increase implementation complexity, especially in combination with NFTs. 
Without partial filling, users would be able to achieve similar behavior with splitting withdrawal requests into multiple smaller ones. Without partial filling, it may also make sense to limit the ETH per request to avoid users having to wait a long time for their request to be completely filled.
### Megapool Exit Criterion

We could simply to random selection, or pick any criterion that can be check on-chain, candidates include:

- lower RPL stake first
- first in, first out
- together with increasing bond requirement, exiting from megapools the furthest below bond requirement

### Liquidity Buffer and Exiting to Fill Buffer

In order to ensure that the Withdrawal Queue can be serviced before rETH redemptions outside the queue, we need to set the liquidity buffer (rETH collateral target) to 0. The current proposal replaces it with a new buffer at the deposit pool level.
A buffer contributes to peg stability and it protects against the validator churn griefing discussed in Security Considerations, but unproductive ETH sitting in it hurts rETH yield. Arguably a buffer is less necessary with a proper withdrawal mechanism.

A related question is if we should exit to fill the Withdrawal Queue or exit to fill the liquidity buffer. Exiting to fill the buffer would lead to a nicer UX for rETH stakers: as long as demand for withdrawals is low, people could instantly redeem rETH at protocol rate, at the cost of reduced rETH APR. 

### Distribution Delay

Minipools don't immediately make ETH available for rETH burns. ETH first needs to be distributed, which initially is exclusive to the node operator and very delayed for others (currently 90 days after starting the distribution window). Therefore, if we are exiting minipools to fill the Withdrawal Queue, it is possible that queue wait time is unexpectedly long. 
Because minipool delegate upgrades are opt-in, this can't be reliably addressed with a delegate upgrade. Potential options include:
- Lowering the delay before permissionless distribution. Security implications would need to be considered.
- Automate distribution in smartnode and introduce a penalty for failing to distribute. Unclear how this could work for Allnodes.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
