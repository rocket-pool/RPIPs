---
rpip: Unassigned
title: Forcibly Exit Offline Validators
description: Use EIP-7002 to exit validators which are offline for a significant period
author: Mike Leach (GH: VVander | Discord: wander1)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-21
---

# Forcibly Exit Offline Validators

## Abstract

I propose the protocol utilize execution layer triggerable exits as defined in [EIP-7002](https://eips.ethereum.org/EIPS/eip-7002) to forcibly exit offline validators as a first step towards improving rETH's performance.

## Motivation

Currently, node operators are incentivized to keep their validators online through the final splitting mechanism which penalizes their own bonded funds first before affecting rETH. This is generally sufficient to prevent malfeasance, but it does not prevent an impact on rETH from users who are incapacitated or simply do not care about losing their own funds. This proposal is designed to limit protocol losses in these scenarios.

## Specification

First, a new `exit` function is defined in the minipool (or megapool) contract which calls the EIP-7002 pre-compile for the associated validator public key. Calls to this function may optionally provide an address parameter that will receive a keeper fee once the validator funds have been withdrawn from the beacon chain.

Second, the following new protocol parameters are introduced:

- `MAX_KEEPER_VALIDATORS`, initially set to 25
- `MIN_VALIDATOR_BALANCE`, initially set to 31.8 ETH
- `VALIDATOR_KEEPER_FEE`, initially set to 0.01 ETH

Third, any address may call the following protocol function:

```
def ExitValidatorsKeeper(pubKey[] validators):
    i = 0;
    for v in validators
        if i > MAX_KEEPER_VALIDATORS
            return
        if minipools[v].balance < MIN_VALIDATOR_BALANCE
            minipools[v].exit(callingAddress);
        i++
```

To incentivize users calling this function, `VALIDATOR_KEEPER_FEE` defines the amount sent from the balance of the exited validator to be sent to the caller's address (specified as `callingAddress` above) before splitting the remaining balance between rETH and the node operator address as usual.

## Rationale

A keeper-style mechanism incentivizes the regular calling of this function by users who will, in most circumstances, profit slightly while helping to keep RP productive. Although using this mechanism means that rETH will suffer by an additional `KEEPER_FEE` ETH value, this value is minimal compared to the value already lost from an offline validator with a balance below 31.5 ETH, and this feature likely saves rETH from additional value loss. Additionally, a keeper mechanism is advantageous in that additiona required trusted oDAO duties are unnecessary.

31.8 ETH was chosen as the initial `MIN_VALIDATOR_BALANCE` as it requires the accrual of significant offline penalties under typical network conditions (approximately 200 days of missed attestations, not including penalties for missed proposals or sync committees) without punishing new validators who were simply offline after being chosen for a sync committee or proposal. Users which have accrued this much offline time are likely to be unable to recover their validators at all due to incapacitation, lost keys, etc.

0.01 ETH was chosen as the initial `VALIDATOR_KEEPER_FEE` as it adequately incentivizes users to call this function, even under high gas cost environments, while still being small enough to minimally harm the operator who will receive this much less of their funds upon disbersement.

## Risk Analysis

### Forced Exits During Inactivity Leaking

The Ethereum staking formula includes additional penalties for offline validators when large portions of the network are offline. This state of accelerated punishment is called "inactivity leaking". With the RP protocol forcing exits for validators under a 31.9 ETH balance, validators who are offline during an inactivity leaking scenario may be exited even if the validator is only offline for a brief period. In some extreme scenarios, a small number of missed attestations could cause a validator to be ejected from the network. This may be necessary, however, in order to protect the value of rETH during an unusual network state for Ethereum.

## Future Enhancements

EIP-7002 broadens the design space for staking protocols such that several options for enhancement are available. Although this document only proposes a single, simple use-case, I also suggest further review and development for consideration in the future:

- Develop or integrate an oracle such as for determining typical performance such that significantly underperforming validators may also be exited
- Study and consider better values for the parameters introduced above to constrain additional losses where possible while keeping a user-friendly operator experience
