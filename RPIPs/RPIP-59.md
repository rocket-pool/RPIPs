---
rpip: 59
title: Deposit Mechanics
description: Describes the mechanics of node operator deposits and validator creation, including standard and express queues.
author: knoshua (@knoshua), Valdorff (@Valdorff)
contributor: LongForWisdom (@LongForWisdom), Samus (@orangesamus), Sckuzzle (@sckuzzle)
discussions-to: https://dao.rocketpool.net/tag/tokenomics-rework
status: Living
type: Protocol
category: Core
created: 2024-03-08
requires: 43, 44
vote-to: https://vote.rocketpool.net/#/proposal/0xb0e1c82fdf83f31de7b8f84767a092fddfb21abb71d81ea3aeec9acdcf43902d
vote-date: 2024-08-21
vote-result: Passed
tags: tokenomics-2024, tokenomics-content
---

## Abstract

Defines the queue structures that govern validator deposits and the overall deposit process for Rocket Pool node operators. The standard and express queue structures give mild priority to existing Rocket Pool node operators and initial node operator deposits through a ticket system.

This proposal also changes the deposit mechanics: In case of a queue, the initial stake transaction happens only once ETH is assigned. This makes it possible to exit from the queue and receive ETH credit up until the validator is dequeued.

## Motivation

These changes are intended to strengthen the protocol by 1) rewarding existing node operators who have shown amazing loyalty to the protocol 2) minimizing governance churn from RPL staked node operators being stuck in queue without voting privileges 3) prioritizing initial deposits to allow easier onboarding of people trialing Rocket Pool before deciding to migrate in full 4) maximizing Rocket Pool's major decentralization advantage over other LSTs - a huge set of small node operators.

This RPIP is part of a set of proposals motivated by a desire to rework Rocket Pool's tokenomics to ensure the protocol’s continued value, development, and longevity. For more details, see the supporting documentation [here](../tokenomics-explainers/001-why-rework.md).

## Specification

### Deposit Queue Specification
This specification introduces the following pDAO protocol parameters:

| Name                                   | Type | Initial Value |
|----------------------------------------|------|---------------|
| `express_queue_rate`                   |      | `2`           |
| `express_queue_tickets_base_provision` |      | `2`           |

ETH from the deposit pool SHALL be matched with validator deposits from queues as follows:
- There SHALL be a `standard_queue`
  - When adding a validator, users SHALL be able to place their deposit in the `standard_queue`  
- There SHALL be an `express_queue`
  - When adding a validator, users SHALL be able to place their deposit in the `express_queue` by spending one `express_queue_ticket`
- When matching ETH from the deposit pool to queued deposits:
  - First, ETH SHALL be matched to the oldest deposit in the `express_queue`; this is repeated `express_queue_rate` times
  - Next, ETH SHALL be matched to the oldest deposit in the `standard_queue`
  - This sequence SHALL be repeated indefinitely until both queues are empty
    - If one queue is empty, those matches SHALL be skipped
- Each node SHALL be provided `express_queue_tickets` equal to `express_queue_tickets_base_provision` (this includes newly created nodes)
- Each node SHALL be provided additional `express_queue_tickets` equal to `(bonded ETH in legacy minipools)/4` (this will always be zero for newly created nodes)

### Deposit Mechanics Specification
This specification introduces the following pDAO protocol parameters:

| Name                   | Type  | Initial Value | Guardrail |
|------------------------|-------|---------------|-----------|
| `time_before_dissolve` | Days  | `14`          | `≥2`      |

A node operator MUST take 2 actions to start a validator: `deposit` and `stake`

#### `deposit` Transaction
- `deposit` SHALL place the node operator ETH in the deposit pool (where it can be used in validators as needed) and place the validator in a queue as described [above](#deposit-queue-specification)
- The following values for the validator SHALL be stored on chain:
    - the public key of the validator
    - the BLS signature over the public key, the withdrawal credentials (the megapool address of the node operator), and the amount (1 ETH) as required by the deposit contract
- The transaction SHALL validate that:
  - the public key has length 48
  - the BLS signature has length 96
- If possible, `deposit` SHALL assign one validator as described below

#### Assigning ETH from the Deposit Pool
- ETH from the deposit pool SHALL be assigned to the validator at the front of the queue by sending 32 ETH to the associated megapool contract
  - rETH mints SHALL assign `floor(ETH_deposit / 32)` validators
  - There MUST be a permissionless function to execute assignments
- The assignment SHALL execute the `prestake` transaction, staking 1 ETH to the beacon chain using the values provided in the step above
- The assignment SHALL remove the validator from the queue

#### `stake` Transaction
- `stake` SHALL stake the remaining 31 ETH to the beacon chain to make a complete validator
- If `stake` is not called within `time_before_dissolve` after the ETH was assigned, the validator SHALL be dissolved, returning the unstaked balance to the deposit pool
  - If a validator is dissolved the bonded value SHALL be recoverable. This MAY require further action from the node operator. This MAY temporarily require additional ETH from the node operator.

#### Exiting Queue
- Until ETH is assigned to a validator, it SHALL be possible to exit the queue and receive ETH `credit` for it

#### Social Assignments
- The `deposit.assign.socialised.maximum` setting SHALL be set to 0


## Rationale

### Deposit Queue
- The express queue is meant to favor (a) small node operators and (b) existing node operators. The end goal in both cases is to support multiple values enshrined in [RPIP-23](RPIP-23.md) (the pDAO charter): decentralization, protocol safety, and the health of the Ethereum network.
  - The `express_queue_tickets_base_provision` is enough to get started, and currently matches the length of `base_bond_array`
  - The tickets from `(bonded ETH in legacy minipools)/4` are enough to fully migrate to 4-ETH deposits during Saturn 1 using the express queue OR to partly migrate to 1.5-ETH deposits after Saturn 2
  - Tickets may be used or not at a node operator's discretion and do not expire! This is because a node operator joining during a time when we don't have a node operator queue is helping fill a protocol need, and it therefore doesn't make sense for us to require them to incur the additional cost of using up a ticket. This means they get to keep their express queue benefit for a later time if they wish.
- Validators with `base_bond` deposits are prioritized to promote decentralization; new or smaller node operators can get up to `base_bond_array.length` validators launched ahead of larger node operators adding `reduced_bond` validators.

### Deposit Mechanics
- `prestake` is moved back from `deposit` to assignment to allow for exiting of the queue
- To allow anyone to execute `prestake`, the validator specific data is stored
- Since adding `prestake` to assignments potentially  increases gas cost for rETH deposits, social assignments are deactivated. node operators will be able to assign to themselves when at the front of the queue. Additionally, the pDAO may fund keepers that execute assignments automatically.

## Security Considerations

### Deposit Queue
- `express_queue_tickets_base_provision` means that the queue favors sock puppets. It's important to disincentivize small sock puppets as they get higher vote power than desired and would most damage the protocol if they took part in MEV theft ([RPIP-42 security considerations](RPIP-42.md#security-considerations) discusses this further).
  - Immediately with Saturn 1 there is a drawback to sock puppets in the form of increased gas cost. They need to deploy multiple megapool smart contract instances and they need multiple transactions to claim beacon chain rewards.
  - With Saturn 2, we update `reduced_bond` to 1.5 ETH, which further disincentivizes sock puppets
- Setting `express_queue_rate` to 0 disables the express queue and people currently in it are stuck indefinitely. Similarly, setting `express_queue_rate` to a very high value effectively disables the `standard_queue`. However, node operators would be able to exit the queue for `credit`.

### Deposit Mechanics
- Validation of provided data in `deposit` matches the validation that the beacon chain deposit contract does and therefore ensures that the `prestake` using the data will succeed
- Disabling social assignments increases the likelihood of ETH accumulating in the deposit pool while validators are waiting in the queue. But the node operators in the queue that would be assigned ETH have a direct incentive to execute the assignments. In addition, we recommend that the pDAO funds (for example through the GMC) development and execution of assignment bots that assign ETH in the deposit pool to the queue at a reasonable gas price.
- Assigning ETH from the deposit pool based on ETH deposit size could allow for gaming if the gas cost of multiple <32 ETH deposits is lower than a single deposit
  - To reduce gas of the single deposit, `prestake` could be decoupled from assignment. This would result in 3 necessary transactions to start a validator: `deposit`, `prestake` and `stake`. Analogous to assignments, `prestake`s could be subsidized by the pDAO.  
  - Alternatively, assignments from ETH deposit could be deactivated entirely. This would mean that all assignment and prestake transactions would have to be pDAO reimbursed and/or executed by the node operators.
- Node operators are still able to perform arbitrage in case of a full deposit pool and a premium on rETH price, since the `deposit` transaction would assign to the validator immediately
- The coupling of `deposit` and assignment also prevents indirect minting of rETH (by depositing, exiting the queue for credit and redeeming it for rETH) while the deposit pool is full
- There is potential for moderate griefing by creating n validators and never calling `stake`. The griefer keeps n\*31 protocol ETH idle for 2 weeks at the cost of keeping n\*ETH_deposit of their own ETH idle and paying gas to `deposit` and reclaim funds from `dissolve`. The text notes that reclaiming funds MAY require further action from the node operator and MAY require additional temporary ETH. Friction could be added in either of those steps should this variety of griefing prove problematic. Alternatively, a small fee to reclaim funds would also be effective (though that would require a new specification etc.).


## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
