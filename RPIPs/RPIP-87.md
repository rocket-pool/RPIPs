---
rpip: 87
title: pDAO Parameter Guardrail Revisions
description: Adjust multiple pDAO protocol parameters to remove unnecessary restrictions and improve protocol security.
author: knoshua (@knoshua)
discussions-to: https://dao.rocketpool.net/t/rpip-pdao-parameter-guardrail-revisions/4010
status: Draft
type: Protocol
category (*only required for Protocol ): Core
created: 2026-07-15
---

## Abstract

This RPIP updates the guardrails for several pDAO parameters to reduce systemic risk and increase flexibility for future governance decisions. It introduces a fixed upper bound on RPL inflation of at most 25% annual inflation, widens the allowed ranges for proposal, veto, and upgrade-veto quorums, removes unnecessary upper limits on the rETH collateral target and megapool dissolution delay, and ties the upper guardrail for the reduced bond parameter directly to the last entry of `base_bond_array`.
## Specification

This specification changes the following pDAO protocol parameter guardrails:

| Name                             | Type    | Current Guardrail                                                       | New Guardrail                                                              |
| -------------------------------- | ------- | ----------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| `rpl.inflation.interval.rate`    | uint256 | No greater than 1e16 more than the previous value.<br>Should not be < 1 | >= 1 & <= 1000611539109222917                                              |
| `proposal.quorum`                | uint256 | >= 15% & < 75%                                                          | >= 10% & <= 30%                                                            |
| `proposal.veto.quorum`           | uint256 | >= 20% & < 75%                                                          | >= 10% & <= 75%                                                            |
| `network.reth.collateral.target` | uint256 | <= 50%                                                                  | <= 100%                                                                    |
| `reduced.bond`                   | uint256 | >= 1 ETH & <= 4 ETH<br>divisible by milliwei                            | >= 1.1 ETH & <= base_bond_array[array_length - 1]<br>divisible by milliwei |
| `megapool.time.before.dissolve`  | uint256 | >= 10 days & <= 60 days                                                 | >= 10 days                                                                 |
| `upgradeveto.quorum`             | uint256 | >= 33% & <= 100%                                                        | >= 10% & <= 100%                                                           |

## Rationale

### `rpl.inflation.interval.rate`

This parameter represents the daily inflation factor under compounding: 
```math
$$

1.000133680617113500^{365} - 1 = 5\%

$$
```
The current guardrail allows raising the daily inflation rate to more than 1% in a single step, which translates to 3865% annual inflation. With repeated parameter changes, RPL inflation is practically unlimited. 

This proposal introduces a fixed guardrail that limits RPL inflation to at most 25% annually:
```math
$$


1.000611539109222917^{365} - 1 = 25\%

$$
```

### `proposal.quorum`

This parameter controls the minimum share of the vote power required to participate in a vote for it to pass. It is currently set to 15%, so the lower guardrail of 15% leaves no room to lower it. A lower`proposal.quorum` is not inherently insecure, so this proposal lowers the lower guardrail to 10%. 

On the other hand, if a pDAO vote increases `proposal.quorum`, all future votes, including votes to lower `proposal.quorum` again, will need to meet that higher `proposal.quorum`, so there is a risk of pDAO governance being disabled until this parameter can be fixed with a smart contract upgrade. 75% of vote power participating appears very difficult to reach based on historical vote participation, so this proposal lowers the upper guardrail to 30%. 

### `proposal.veto.quorum`

This parameter controls the vote power necessary to veto a proposal. It is currently set to 20%, so the lower guardrail of 20% leaves no room to lower it. A lower`proposal.veto.quorum` is not inherently insecure, so this proposal lowers the lower guardrail to 10%. 

### `network.reth.collateral.target`

This parameter controls how much ETH is reserved for rETH burns rather than being available to new validators. Setting it to a high value has very limited immediate impact, since it also requires validators to exit en masse. This proposal removes the guardrail introduced from the Cantina audit, because the pDAO should be able to set it to a desired value.

### `reduced.bond`

This parameter controls the bond requirement per validator beyond the first few (currently 2) validators, controlled by `base_bond_array`. This proposal links the upper guardrail directly to the last value of `base_bond_array`. The raised lower guardrail is introduced in [RPIP-79](RPIP-79.md).

### `megapool.time.before.dissolve`

This parameter controls the time that must be waited after a first deposit before a megapool validator can be dissolved. Because the Cantina audit added a 60-day upper guardrail, we had to apply a hotfix to set this to 365 days via a smart contract upgrade shortly after the Saturn 1 launch. This proposal removes the unnecessary upper guardrail.

### `upgradeveto.quorum`

This parameter controls the minimum share of the Security Council necessary to veto a contract upgrade. It is currently set to 33%, so the lower guardrail of 33% leaves no room to lower it. A lower `upgradeveto.quorum` is not inherently insecure, so this proposal lowers the lower guardrail to 10%. 

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
