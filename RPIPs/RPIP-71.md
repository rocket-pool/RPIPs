---
rpip: 71
title: rETH Withdrawal liquidity via EIP-7002
description: Enable Rocket Ether stakers to trigger validator exits to access protocol liquidity for unstaking from Rocket Pool.
author: knoshua (@knoshua)
discussions-to: https://dao.rocketpool.net/t/rpip-71-reth-withdrawal-liquidity/3947
status: Draft
type: Protocol
category: Core
created: 2025-05-14
requires (*optional): 80
---

## Abstract

rETH is fully backed by staked ether but currently lacks a direct mechanism for stakers to signal a desire to exit. With the adoption of [EIP-7002](https://eips.ethereum.org/EIPS/eip-7002), Rocket Pool can implement execution-layer-triggered exits that respond to explicit rETH withdrawal requests without requiring coordination with node operators.

This RPIP specifies a Withdrawal Queue and validator-selection mechanisms for minipools and megapool validators.

## Motivation

rETH currently exposes stakers to liquidity risk: although their stake can, in principle, be withdrawn from the beacon chain, there is no direct protocol mechanism for rETH holders to access it. Instead, they must rely on limited in-protocol liquidity or sell rETH on the open market. This creates a dependence on secondary-market liquidity and can make rETH less attractive, especially for larger investors and users with time-sensitive liquidity needs.

## Specification

This specification introduces the following pDAO protocol parameters:

| Name                             | Type    | Initial Value | Guardrail<br>           |
| -------------------------------- | ------- | ------------- | ----------------------- |
| `withdrawals_enabled`*           | bool    | `true`        |                         |
| `deposit_pool_collateral_target` | pct     | 1             |                         |
| `exit_hysteresis`                | ETH     | 120           | > 32                    |
| `megapool_exit_phase`            | boolean | `false`       | can't be set to `false` |
| `staking_delay`                  | days    | 28            | > 7                     |
| `tournament_size`<br>            | integer | 4             | < 20                    |

A * designates this parameter as being modifiable by the Security Council without a delay.

This specification changes the following pDAO protocol parameters:

| Name                             | Type | Current Value | New Value |
| -------------------------------- | ---- | ------------- | --------- |
| `network.reth.collateral.target` | pct  | 1             | 0         |

### Withdrawal Queue

- A Withdrawal Queue contract MUST exist to provide the interface for pool stakers to request exits at the protocol rate.
- Starting 28 days after the upgrade that implements this RPIP and if `withdrawals_enabled = true`, the Withdrawal Queue MUST accept rETH deposits from users to create withdrawal requests.
- The ETH value of the rETH at the time of a withdrawal request MUST be recorded.
- Canceling a withdrawal request and exiting the queue SHALL NOT be possible.

### Fulfilling Withdrawal Requests

- While the Withdrawal Queue is not empty:
	- ETH from new rETH mints SHALL first go towards the Withdrawal Queue.
	- Redemption of rETH outside the queue SHALL be prevented to the extent possible.
	- The protocol SHALL allow the Withdrawal Queue to burn rETH.
- `network.reth.collateral.target` SHALL be set to 0 and a new buffer for withdrawals SHALL be implemented in the deposit pool, reserving up to `deposit_pool_collateral_target` percent of ETH backing rETH for rETH burns.
- When megapools distribute rewards, they SHALL send ETH to the deposit pool rather than the rETH contract.
- If `withdrawals_enabled = true` the Withdrawal Queue SHALL be able to fulfill withdrawal requests by burning the corresponding rETH. The user SHALL receive the stored ETH value at the time of the request or at the time of the rETH burn, whichever is smaller.
- Any remaining ETH corresponding to that rETH burn SHALL be transferred to the deposit pool.

### Validator Exit Selection

For the purposes of this section, the withdrawal shortfall is defined as the remaining ETH required to fully satisfy all pending Withdrawal Queue requests after accounting for:
- `requested_exit_eth` (defined in RPIP‑80),
- `voluntary_exit_eth` (defined in RPIP-80),
- ETH held by the rETH contract, and
- ETH held by the deposit pool.

If the withdrawal shortfall is at least `exit_hysteresis` ETH, additional megapool validators MUST be selected and added to the exit list as defined in this section.

#### If `megapool_exit_phase = false`

- The protocol SHALL allow the oDAO, by majority vote, to request minipools to exit as specified in RPIP-80. The amount necessary to cover the withdrawal shortfall minus `exit_hysteresis` SHALL act as a hard cap on the possible exit requests.
- The oDAO SHOULD monitor the number of voluntarily exiting minipools and only request exits after accounting for them.
- The oDAO SHOULD select minipools by prioritizing higher-commission minipools and SHOULD ignore any minipool that has received a `did_not_exit_penalty` within the last `did_not_exit_cooldown` (defined in RPIP-80). 
- The oDAO MAY use time until the next validator sweep as a tie-breaker between minipools with equal commission.

#### If `megapool_exit_phase = true`

##### Eligible megapool validator set

The protocol SHALL initialize a set of eligible megapool validators from which validators can be selected for exit under this proposal. A megapool validator MUST satisfy all of the following conditions in order to be included in this set:
- `stake` was already called for the validator.
- The validator is not already exiting.

- When `stake` is called for a megapool validator, the protocol SHALL schedule the addition of that validator to the eligible megapool validator set after a pDAO-configurable `staking_delay`.
- When an exit request is made for a megapool validator under RPIP‑80, that validator MUST be removed from the eligible megapool validator set or the scheduled inclusion MUST be canceled.
- When `notifyExit` is called for a megapool validator that is still in the eligible megapool validator set, that validator MUST be removed or the scheduled inclusion MUST be canceled.

##### Tournament-based selection

Let `N` be the current size of the eligible megapool validator set. Let `k` be `tournament_size` (or `N` if `tournament_size > N`). The protocol SHALL allow repeated selection of validators while:
- the withdrawal shortfall is at least `exit_hysteresis` ETH, and
- the eligible megapool validator set is non‑empty.

1. The protocol MUST sample `k` distinct validators uniformly at random without replacement from the eligible megapool validator set.
2. For each sampled validator, the protocol MUST compute an exit score according to the chosen Exit Criterion.
3. The protocol MUST select exactly one validator from the sampled set to exit request: the one determined to be the highest-priority according to the Exit Criterion.
4. The protocol SHALL request that validator to exit, as defined by RPIP-80.

### Reentry Queue

This section modifies the behavior of the deposit queue defined in [RPIP-59](RPPIP-59.md).

- In addition to the `standard_queue` and `express_queue`, there SHALL be a `reentry_queue`.
- When adding a validator, users SHALL be able to place their deposit in the `reentry_queue` by spending one `reentry_queue_ticket`.
- When matching ETH from the deposit pool to queued deposits, ETH SHALL first be matched to the oldest deposit in the `reentry_queue`.
- If the `reentry_queue` is empty, ETH from the deposit pool SHALL be matched to queued deposits as specified in RPIP-59.
- Node operators SHALL receive a `reentry_queue_ticket` when:
	- A minipool was requested to exit for withdrawal liquidity and that minipool was distributed (either `finalise = true` or `userDistributed = true`).
	- A megapool validator was requested to exit for withdrawal liquidity and `notifyFinalBalance` was called for that validator.
- Node operators SHALL NOT receive a reentry_queue_ticket when a validator exits voluntarily, when a validator is requested to exit for underperformance, or when a minipool is requested to exit for liquidity and does not exit.

### Permissionless Distribution of Minipools

- The protocol SHALL allow anyone to prove that a minipool has been withdrawn and immediately `distributeBalance` for that minipool.
- To achieve this:
	- `RocketDAOProtocolSettingsMinipool.getUserDistributeWindowStart()` SHALL return a value stored in `RocketStorage`.
	- That value SHALL be set to 90 days initially.
	- That value SHALL not be modifiable with a pDAO Proposal.
	- When a withdrawal is proven for a minipool, the value SHALL be temporarily set to 0 until `distributeBalance` is called.
## Rationale

### Withdrawal Queue

The Withdrawal Queue opens 28 days after the upgrade containing this RPIP is deployed, giving node operators an opportunity to upgrade to the new delegate and update their smartnode to a version that supports exit request fulfillment. It also aims to ensure that validators in the eligible set are actively staking.

The protocol is potentially exiting validators to fulfill a withdrawal request. If users could cancel withdrawal requests, they can trigger unneeded exits and reduce rETH APR.

Changing `network.reth.collateral.target` from 1 to 0 is needed so that ETH from exiting validators flows into the deposit pool rather than remaining as excess collateral in the rETH contract. Including Withdrawal Queue demand in `RocketDepositPool.getExcessBalance()` ensures that ETH freed from exits and new deposits can preferentially be used to satisfy withdrawal requests, rather than being available for rETH burns outside the queue.

The redemption rate at which a request is fulfilled is set to the minimum of the rate at the time of the request and the time of fulfillment. rETH in the Withdrawal Queue cannot keep earning staking rewards or there would be an incentive to always keep rETH in the queue and mint new rETH once it can be fulfilled, as this improves liquidity of the position without losing rewards. At the same time, being in the Withdrawal Queue should not protect against rETH losing value in extreme slashing scenarios.

### Fulfilling Withdrawal Requests

Since the rETH contract is not upgradeable and has built-in redemption via the `burn` method, ensuring that the Withdrawal Queue is prioritized requires some thought.

Setting the collateral target to 0 ensures that fully withdrawn minipool user ETH goes to the deposit pool. Reward distributions from old minipool delegates will still stay in the rETH contract and can be used to redeem rETH outside the Withdrawal Queue; this is why the specification calls for "to the extent possible".

rETH also uses ETH from the deposit pool for `burn`, based on the value `RocketDepositPool.getExcessBalance()` returns. The Deposit Pool contract can be upgraded, but we need to ensure that rETH burns are possible for the Withdrawal Queue and rejected when someone other than the Withdrawal Queue attempts to burn. This may involve setting a state flag in the deposit pool to change the behavior of `RocketDepositPool.getExcessBalance()` during a burn transaction from the Withdrawal Queue.

### Exit Hysteresis

The design only allows exits when the withdrawal shortfall is at least `exit_hysteresis` ETH.  This ensures that we are not exiting a whole validator to cover a residual amount that can reasonably be filled by future rewards or natural inflows.

The initial value is chosen based on the expected time between validator exit and ETH becoming available and the expected rewards earned during that time, using the following values:
- 890k active validators
- no beacon chain exit queue
- 380k ETH in rETH
- 2.3% rETH APR

With these values, on average it will take ~5 days for ETH to become available and in that time, rETH earns ~120 ETH in rewards. In other words, if the withdrawal shortfall is below 120 ETH, withdrawals will be filled from rewards before ETH from exited validators becomes available. exit_hysteresis is implemented as a tunable parameter so that pDAO can fine-tune it when the assumptions underlying it change significantly.

### Minipool Exit Phase

This design uses a phased approach, with minipools exited before moving to megapools.

The definition of the withdrawal shortfall aims to trigger exits only when existing sources of liquidity are insufficient to satisfy the Withdrawal Queue, to the extent feasible (for example, ETH rewards in minipool contracts are not included here). While minipools are still around, we rely on the oDAO to track voluntary minipool exits off-chain, because tracking this on-chain would be challenging.

Using the oDAO for minipools also allows us to order exits by commission, which marginally improves the rETH APR in response to rETH demand reduction.

We focus on minipools in the first phase, not only to keep the reliance on the oDAO as short as possible, but also because this aligns with the overall desire to move away from minipools and towards megapools: to support the new RPL tokenomics, the new Universal Adjustable Revenue Split mechanism ([RPIP-46](RPIP-46.md)), and to reduce complexity of future upgrades.

We also considered a trustless design for minipools. This would increase complexity and incur additional gas costs that the protocol or pDAO would either have to fund in some way or add to oDAO's duties. The trusted oDAO duty appears preferable because of its temporary nature, limited risk, reduced complexity and lower gas cost.

The `megapool_exit_phase` toggle allows the pDAO to switch to the second phase once minipools are largely exited or known to be unresponsive. Once the remaining minipool set largely consists of minipools on old delegates that do not respond to exit requests, the withdrawal mechanism stops working as intended. At that point, the pDAO can consider voting on a final penalty for any remaining minipools. The choice of when to switch to megapools and how to handle remaining minipools is left to the pDAO.

### Megapool Exit Phase

Once the protocol is in a state with only megapool validators, it becomes feasible to implement exit selection using a trustless, permissionless proposal-and-challenge mechanism. How to initialize the eligible megapool validator set at upgrade time is left as an implementation detail.

The staking_delay applied after the stake call is meant to ensure that validators are added to the eligible set only after they have made it through the beacon chain queue, to avoid requests that would take a long time to execute. This also protects new validators from immediately being exited.

The tournament‑based mechanism is chosen because maintaining a full global ordering of all eligible validators by the Exit Criterion may not be feasible. By sampling a small, random subset of eligible validators and exiting the validator with the worst Exit Criterion score in that subset, the protocol achieves a decent approximation of a full ordering by the Exit Criterion. For the initial tournament_size = 4, on average we will select someone around the 20th percentile according to the Exit Criterion.

#### Megapool Exit Criterion

No clear consensus on what criterion should be used has emerged yet. Options that have been discussed so far include:

- random
- lower RPL stake first
- first in, first out
- together with increasing bond requirement, exiting from megapools the furthest below bond requirement
- biasing towards larger nodes
- bias towards not hitting same node again (this seems to be ~biasing towards smaller nodes)

The pDAO will vote on an exit criterion before Saturn 2 is deployed.

### Reentry Queue

From the perspective of node operators, the reentry queue is a form of restitution for being chosen for liquidity, with at least some randomness and while meeting the performance threshold set by [RPIP-73](RPIP-73.md). From the protocol's perspective, prioritizing proven node operators that met the performance threshold over unknown (potentially underperforming) node operators is rational.

We also considered a direct ETH payment, financed from rETH in the withdrawal queue that stops earning rewards, as restitution. This appears more challenging to implement and is harder to justify from the protocol's perspective. Withdrawal liquidity exits mean there is less demand than supply and this would be paying the supply side. Contrarily, the benefit from the reentry queue is conditional on demand returning to the previous level. 

### Permissionless Distribution of Minipools

Minipools don't immediately make ETH available for rETH burns. ETH first needs to be distributed, which is initially exclusive to the node operator and is very delayed for others (currently 90 days after starting the distribution window). Therefore, if we are exiting minipools to fill the Withdrawal Queue and node operators don’t distribute themselves, the wait time may be unexpectedly long.

This can’t be reliably addressed with a delegate upgrade, since these are opt-in. Instead, the design allows distribution without delay by temporarily removing the restriction and requiring a proof that the validator has already been withdrawn.

## Security Considerations

### Fulfilling Withdrawal Requests and rETH Updates

Without further changes to the oDAO balance tracking duty that informs the rETH exchange rate, the proposed mechanism leads to a re-distribution of staking rewards from rETH in the Withdrawal Queue to other rETH holders at the time a withdrawal request is fulfilled. Potentially, this could lead to a very large rETH rate increase with the following update that could become “sandwichable”: someone could mint a large amount of rETH right before the update and burn it right after to profit at the expense of rETH holders.
However, rETH protects against this type of exploit with a pDAO-configurable rETH mint fee, currently set to 0.05%. This means, for example, that if 30% of rETH supply is in the Withdrawal Queue for 2 weeks, the resulting rETH rate update still could not be profitably sandwiched when rETH yield for that day is approximately average. If a large request fulfillment coincides with a high yield day for rETH, the threshold for profitably sandwiching is lower. For example, a day with an 80ETH MEV block, together with 20% of the rETH supply in the queue for two weeks, can be sandwiched.
If the pDAO wants to protect against even more extreme scenarios, the mint fee could be slightly increased.

### Protection Against Repeated Minting and Withdrawing

A concern is that a malicious actor could reduce the yield of rETH by repeatedly minting and withdrawing rETH, as this would keep a share of staked ETH constantly in an unproductive state. The design makes this kind of attack both costly and mostly ineffective.

There is already a mint fee on rETH, so an attacker would currently lose 0.05% per round trip. They also would not earn any yield, based on how the redemption rate is defined.

Furthermore, the new withdrawal buffer in the deposit pool means that a repeatable attack requires more capital than the buffer size (controlled by deposit_pool_collateral_target) and only the amount above that buffer can create validator churn.

### oDAO selecting minipools  

The oDAO has discretion in selecting minipools during the initial phase and is responsible for factoring in voluntarily exiting minipools to determine how many to exit. However, the withdrawal shortfall limits the amount of exits. At most, the oDAO could exit unnecessary minipools equal to the currently voluntarily exiting minipools, provided there is sufficient withdrawal demand at the same time. So the exposure here is mostly limited to ordering and a small amount of extra exits. Since the rules are known and the actions are transparent, the oDAO properly executing this duty can be monitored.

### Correlated Slashing Penalties

Ethereum slashes validators for breaking protocol rules that could be part of an attack on the chain. The slashing penalty for a slashed validator increases when many other validators are slashed in the 18 days before and after the slashing. In extreme scenarios (about 5% of validators slashed within a 36 day window), this correlation penalty can lead to the rETH exchange rate going down. Sophisticated rETH holders could anticipate this and use the withdrawal mechanism to avoid any losses, which then would increase losses experienced by rETH holders that are not able to withdraw in time. 

To handle these extreme scenarios fairly, the Security Council is able to temporarily disable withdrawals. This toggle will prevent joining the Withdrawal Queue and redeeming existing withdrawal requests. 

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
