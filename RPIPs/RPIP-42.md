---
rpip: 42
title: Bond Curves
description: Introduce smaller ETH bonds to improve capital efficiency for Node Operators while maintaining the security of the Rocket Pool protocol.
author: Valdorff (@Valdorff)
discussions-to: TBD
status: Draft
type: Protocol
category: Core
created: 2024-03-08
requires: 43, 44
tags: tokenomics-2024, tokenomics-content
---

## Abstract
This proposal dramatically increases the LTV used in the protocol (loan to value; the amount of ETH that can be borrowed per unit of bonded ETH). It does this safely by:
- Enabling megapool-level penalties to mitigate/discourage MEV theft
- Using forced exits as needed
- Starting with lower LTV at lower total bonded ETH to mitigate/discourage MEV theft
- Retaining sufficient bond per validator regardless of total stake to mitigate against slashing and abandonment
- Providing increased capital efficiency with greater bond to encourage an NO to stake as large nodes instead of many small nodes

This proposal also explicitly tries to benefit the smallest NOs in a few ways, in line with the pDAO charter values of decentralization and prioritizing Ethereum health (see [RPIP-23](./RPIP-23.md)):
- We willingly take on somewhat more MEV-theft risk for the smallest NOs (see [Rationale](#rationale))
- We give precedence to small nodes staking some number of initial validators in the Node Operator queue (see [RPIP-59](RPIP-59))
- There is a small but tangible financial benefit for large stakers that stake as few large nodes instead of many small nodes -- this (alongside our vote power, which scales with the square root of vote-eligible RPL) helps preserve the strong governance voice of small NOs

## Motivation

The primary motivation for the introduction of smaller ETH bonds in this form is twofold. First, lower ETH bonds allow for the same amount of bonded ETH to support a larger volume of rETH, this helps unlock growth for the protocol, as it has been primarily limited by lack of Node Operators in the months preceding this proposal. Second, lower ETH bonds increase the profitability of Rocket Pool as a protocol, allowing it to grow and compete favorably with alternative liquid staking providers.

The innovation of a bond curve is motivated by the need to maintain enough stake to mitigate MEV theft, slashing penalties, and abandonment; while meeting the core goal of improved capital efficiency.

This RPIP is part of a set of proposals motivated by a desire to rework Rocket Pool's tokenomics to ensure the protocol’s continued value, development, and longevity. For more details, see the supporting documentation [here](../tokenomics-explainers/001-why-rework).

## Specification
This specification introduces the following pDAO-configurable settings:

| Name                           | Type    | Initial Value             |
|--------------------------------|---------|---------------------------|
| `base_bond_array`              | `ETH[]` | `[4, 8]`                  |
| `maximum_megapool_eth_penalty` | `ETH`   | `612` [PLACEHOLDER VALUE] |
| `reduced_bond`                 | `ETH`   | `4`                       |

Array indexing in this section is zero-based.

- The oDAO SHALL be able to penalize stake at the megapool level when a [Penalizable offense](#penalizable-offenses) is committed by increasing the megapool's `debt` variable defined in [RPIP-43](./RPIP-43.md/#debt-variable)
  - The oDAO SHALL NOT be able to apply more than `maximum_megapool_eth_penalty` worth of penalties in 50,400 consecutive slots
  - The pDAO SHALL NOT be able to set `maximum_megapool_eth_penalty` lower than 300 ETH [PLACEHOLDER VALUE]
  - Note that this does not replace RPIP-58, which applies to legacy minipools
- Prior to creating a validator, there MUST be no `debt` on the megapool
  - There MAY be a convenience function to use a single ETH payment to pay off existing `debt` and create an additional validator
- When a Node Operator creates a validator, with `i` validators in the megapool prior to adding: 
  - If `i < base_bond_array.length`: the required `user_deposit` is the amount of additional ETH to bring the user's total bond up to `base_bond_array[i]`.
  - If `i ≥ base_bond_array.length`:, the required `user_deposit` is `reduced_bond` per validator.
- When a Node Operator removes a validator, with `i` validators in the megapool prior to removing:
  - If `i > base_bond_array.length`: the Node Operator share before `debt` is `reduced_bond`.
  - If `i ≤ base_bond_array.length` and `i > 1`: the Node Operator share before `debt` is the amount of ETH that would bring the user's total bond down to `base_bond_array[i-2]`.
  - If `i==1`: the Node Operator share before `debt` is the amount of ETH that would bring the user's total bond down to 0 ETH.
- Bulk validator creation/removal functions MAY be provided. If they are, they SHALL behave the same as multiple individual transactions.
- If an NO has more total bonded ETH in their megapool than would be necessary based on the current settings (eg, `reduced_bond` is reduced) and they have no `debt`, it SHALL be possible to reduce their bonded ETH and receive ETH `credit` for it
- `credit` MUST be usable to create validators in a megapool
- `credit` MAY be usable to mint rETH to the NO's primary withdrawal address
- `base_bond_array` SHALL NOT be allowed to be set to `[]`

## Specification taking effect with Saturn 2
- Update `reduced_bond` to 1.5 ETH

## Penalizable offenses
This portion of the RPIP SHALL be considered Living. It may be updated by a DAO vote following the existing rules and conventions for RPIP modifications.

| Offense   | Penalty              | Added      | Updated    |
|-----------|----------------------|------------|------------|
| MEV theft | theft size + 0.2 ETH | 2024-03-29 | 2024-03-29 |

## Implementation thoughts
- When showing legacy node status, there is not a trivial way to get the node index for a given address. That said, the other direction is trivial using `RocketNodeManager.getNodeAt`, so the work can get moved off-chain. Eg, one can iterate across all possible node indices and then pass in the node index that matches the node's address; the smart contract can confirm the match to demonstrate legacy node status.

## Rationale
The bond curve is an attempt to get close to maximizing capital efficiency while maintaining safety. It balances capital efficiency with slashing, leakage, and MEV theft risks as described in [Security considerations](#security-considerations). 

"Bond reduction" for credit is explicitly supported so that NOs that deposited under higher requirements are not permanently disadvantaged. At the same time, there is no equivalent support for "bond increase". This is because those validators are already running and it would need a heavy-handed approach, such as force exiting, to enforce bond increase. Since the bond curve is based on total bonded ETH when depositing, NOs adding new validators would need to add enough ETH bond to match the "bond increased" curve.

## Security considerations
- Bond sizes were originally ideated per [prior work](../assets/rpip-42/bond_curves.md).
  - `base_bond_array` is chosen to "sufficiently" dissuade MEV theft as a strategy
  - `reduced_bond` is chosen to "sufficiently" guard against slashing or abandonment risks
- Follow-on work was done in [discord](https://discord.com/channels/405159462932971535/1228753782402318427/1228914436924772352)
  - The plots below show `base_bond_array`=[4, 8] and `reduced_bond`=1.5. As we can see, MEV theft always increases yield and the impact is heightened at low commission. The reality is that we've seen very little of this type of behavior. We may have to change our approach if we see MEV theft increase or if we wish to support NO commission share under 2.5%.
  - A moderate step would be to change `base_bond_array` to a curve that reduces MEV theft advantage in the current context (commission, MEV landscape...) at the cost of user complexity, eg `[4.2, 6.8. 9.2. 11.4. 13.5. 15.5. 17.4]`
  - A larger step would be to pass EL rewards to NOs and charge them for the benefit. See eg: <https://github.com/Valdorff/rp-thoughts/tree/main/leb_safety#negative-commission-aka-assign-execution-layer-rewards-to-nos> or <https://dao.rocketpool.net/t/reimagining-large-block-theft/2146>
  
| 2.5% commission                                 | 4% commission                                |
|-------------------------------------------------|----------------------------------------------|
| ![img.png](../assets/rpip-42/theft_2.5pct.png)  | ![img.png](../assets/rpip-42/theft_4pct.png) |

- Minimum size sock puppets are most damaging to the protocol (ie, have the greatest drag), so it's important that they are not incentivized. Note that the bond curve has been chosen to ensure that this size is _not_ the most efficient for theft. They get the most _advantage_ from theft, but not the highest overall yield from a dishonest strategy.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
