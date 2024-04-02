---
rpip: 44
title: Integrating Execution Layer Triggerable Exits
description: Describes when EL triggerable exits will be used within RP
author: Valdorff (@Valdorff), Mike Leach (@VVander)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-05
requires: eip-7002
---

## Abstract
This proposal specifies when [execution layer triggerable exits as defined in EIP-7002](https://eips.ethereum.org/EIPS/eip-7002)  will be used by RP contracts. There are two enumertated use cases. The first is that the Node operator may freely request exits of their validators. The second is a keeper mechanism that allows and incentives any user to forcibly exit a Node Operator's validators if that Node Operator owes the protocol a threshold amount. This helps keep rETH performant and minimizes funds lost by a badly performing Node Operator. 

## Specification
- An `exit` function SHALL be included in the Megapool delegate
  - This function SHALL take in an array of validator public keys
  - This function SHALL NOT allow base validators to be exited if there are still satellite validators active and not in the array
  - This function MAY be freely called by the Node Operator
  - This function MAY be called by any user if the Node has a `deficit` greater than `exit_deficit`
    - It MUST NOT be possible to exit more than the minimum number of validators needed to cover `deficit`
    - The caller SHALL receive `keeper_reward`, which is deducted from the Node Operator's share
- `deficit` is `balance_deficit + penalties - expected_exit_balance`
  - `balance_deficit = 32-balance`; this represents the amount of validator principal lost due to Ethereum penalties
  - `penalties`; this represents the amount of ETH owed to the protocol per [RPIP-42's penalties](RPIP-42.md#penalizable-offenses)
  - `expected_credit_from_exit`; this represents the amount of ETH the NO expects to receive as their share from validator exits that have begun but not yet completed
    - The protocol SHALL update `expected_credit_from_exit` when exits are begun, and subtract the minimum 1 ETH if the validator was slashed
    - The protocol SHALL update `expected_credit_from_exit` when exits are completed 
- The protocol SHOULD use `rewards` and/or `credit` held by the NO to decrease `debt` by the same amount prior to taking action on exits
- The protocol SHOULD use staked RPL held by the NO to decrease `debt` by the corresponding amount prior to taking action on exits
- The initial settings SHALL be:
  - `exit_deficit`: 0.2 ETH
  - `keeper_reward`: 0.01 ETH + approximate gas refund based on precalculated gas and the BASEFEE opcode

## Rationale
- `exit_deficit` is about months covering leakage at 4% apy, using the rule of thumb that losses accrue at half the speed of gains `32*(.04/2)*(3/12) = .21 ETH`. Users that have accrued this much offline time are likely to be unable to recover their validators at all due to incapacitation, lost keys, etc.
- `keeper_reward` is set to ensure that users are incentivized to call this function when possible
- `expected_credit_from_exit` has a known potential shortfall in cases of correlated slashing; that said, upon the completion of the exits, the numbers will be correctly updated. This means there may be a second round of exits in such a case.

## Risk Analysis

### Forced Exits During Inactivity Leaking

The Ethereum staking formula includes additional penalties for offline validators when large portions of the network are offline. This state of accelerated punishment is called ["inactivity leaking"](https://ethereum.org/en/developers/docs/consensus-mechanisms/pos/rewards-and-penalties/#inactivity-leak). With the RP protocol forcing exits for validators, NOs who are offline during an inactivity leaking scenario may be exited even if the validator is only offline for a brief period. In some extreme scenarios, a small number of missed attestations could cause a validator to be ejected from the network. This may be necessary, however, in order to protect the value of rETH during an unusual network state for Ethereum.

## Future Enhancements

EIP-7002 broadens the design space for staking protocols such that several options for enhancement are available. Although this document only proposes a single, simple use-case, the following is suggested further review and development for consideration in the future:

- Develop or integrate an oracle such as for determining typical performance such that significantly underperforming validators may also be exited
- Study and consider better values for the parameters introduced above to constrain additional losses where possible while keeping a user-friendly operator experience

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
