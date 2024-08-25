---
rpip: #<to be assigned>
title: Tokenomics Rework Prelude
description: This proposal aims to improve RPL tokenomics in the short term to unlock protocol growth. 
author: knoshua (@knoshua), haloooloolo (@haloooloolo)
discussions-to: <URL>
status: Draft
type: Protocol
category: Core
created: 2024-07-25
---

## Abstract
This proposal aims to improve RPL tokenomics in the short term before the changes of RPIP-49 can be implemented. 
Minipools can be created without a minimum RPL requirement and at 5% contract commission. A temporary (until [Saturn 1](RPIP-55.md), in reduced form until [Saturn 2](RPIP-56.md)) dynamic commission boost beyond this value is introduced. Total dynamic commission starts at 10% and scales linearly with RPL stake up to 14% at 10% of borrowed ETH.
The cliff for RPL rewards is removed by extending rewards linearly below 10%. Scrub penalties are changed to be taken out of the node operator's ETH bond instead of slashing RPL.

## Motivation
With the DAO having voted for the Saturn upgrade, fundamental value of RPL will primarily be based on megapool TVL.
Short term increases in TVL are beneficial for the protocol as long as they can be expected to convert to megapools.
On the other hand, as competition emerges and equivalent or higher yield  is accessible without the need to acquire a protocol token, short term RPL utility is unlikely to still significantly support fundamental value.
Therefore, node operation is made more attractive by allowing minipool creation without RPL and removing the cliff for RPL rewards.
Contract commission for these new ETH-only minipools is kept less attractive than megapools under [Saturn 1](RPIP-55.md) to encourage migration once dynamic commission is disabled.
In the interest of acting fast, this proposal minimizes smart contract changes. The suggested parameter changes can be enacted immediately after the vote passes and changes to the scrub penalty are implemented. The dynamic commission requires reward tree spec changes that may only be rolled out shortly after.

## Specification
- `node.per.minipool.stake.minimum` SHALL be set to 0
- `network.node.fee.target` SHALL be set to 5%
- `network.node.fee.minimum` SHALL be set to 5%
- `network.node.fee.maximum` SHALL be set to 5%
- The scrub penalty SHALL be set to 2.4 ether and made to be withheld from an offending minipool's bond
- [Reward Tree Spec v10](../assets/rpip-xx/rewards-calculation-spec.md) SHALL be implemented, which consists of the following changes
  - For minipools that are opted into the smoothing pool, determine the commission for smoothing pool calculations based on RPL stake
    - Before [Saturn 1](RPIP-55.md): `commission = max(contract_commission, 10% + 4% * min(1, percent_of_borrowed_ETH / 10))`
    - After [Saturn 1](RPIP-55.md), before [Saturn 2](RPIP-56.md): `commission = max(contract_commission, 5% + 9% * min(1, percent_of_borrowed_ETH / 10))`
    - After [Saturn 2](RPIP-56.md): `commission = contract_commission`
  - For the same minipools as above, calculate their individual beacon chain rewards during the rewards period and give them a bonus (`node_reward_bonus`) based on `bonus_commission = commission - contract_commission`
  - If the smoothing pool balance is not sufficient to cover the beacon reward bonus for all minipools (`total_reward_bonuses`):
    - Fully credit the adjusted smoothing pool rewards excluding `node_reward_bonus` to all nodes
    - Credit modified reward bonuses as `node_reward_bonus * (remaining_balance / total_reward_bonuses)`
- [Reward Tree Spec v9](RPIP-52.md) or [Reward Tree Spec v8](RPIP-51.md) MAY be used for reward periods ending before [Reward Tree Spec v10](../assets/rpip-xx/rewards-calculation-spec.md) is available. This SHOULD affect no more than one reward submission after the vote for this proposal ends

## Rationale
### Megapool Conversions
The base commission (without dynamic commission) is lowered to 5% such that megapools introduced in [RPIP-49](RPIP-49.md) remain significantly more attractive (an ETH-only LEB4 achieves equivalent returns with `no_share` of 2.14%). We expect that operators with these new ETH-only minipools will eventually migrate to megapools and contribute to RPL value capture.

### Dynamic Commission Eligibility
Requiring smoothing pool participation to receive dynamic commission provides strong guarantees that the end-of-interval balance will be sufficient to cover the full reward bonus for all qualifying minipools. In a worst case scenario, every minipool is eligible to receive a total commission of 14% due to their parent node's RPL stake while earning a contract commission of only 5%. This makes 86% of the pool's pETH execution yield available to pay out a reward bonus that comprises 9% of pETH consensus yield. Even so, the balance will remain sufficient as long as execution rewards are less than `86/9 = 9.55` times lower than consensus rewards, or more concretely, provide an APR of at least `2.8/9.55 = 0.29%` with annual consensus layer yield at 2.8%. This is less than half of the current [network average](https://explorer.rated.network/network?network=mainnet&timeWindow=30d&rewardsMetric=average&geoDistType=all&hostDistType=all&soloProDist=stake).

## Considerations
### Supporting Protocols
- [NodeSet Constellation](https://nodeset.medium.com/project-hyperdrive-4819f22391dc) enables liquid RPL staking (xRPL) and liquid ETH staking with full solo validator yield (xrETH) by building on top of the core protocol. Both of their products have the potential to increase node operator TVL by attracting market participants that do not wish to lock their capital in a node. As Constellation's revenue is mainly derived from ETH commission, sustaining viability requires maintaining the ability to earn high ETH commission on minipools. Dynamic commission addresses this by offering 14% total commission at an RPL collateralization level of 10%.
- [Rocket Lend](https://github.com/rocketlend/protocol) is a Rocket Pool specific lending protocol funded by the GMC. It enables NOs to use their ETH bond as collateral to borrow RPL and stake it on their node. This proposal will likely make borrowing RPL less appealing as the marginal ETH yield it unlocks is reduced. However, the proposal may also onboard new node operators that will be interested in participating in the lending market. As such, Rocket Lend can still serve an important role in increasing capital efficiency for both lenders and borrowers.

### Security
- The minimum collateral per minipool is lowered from 10.4 ETH to 8 ETH at time of creation. This is sufficient as it is still significantly higher than the 4 ETH validator bond proposed in [RPIP-49](RPIP-49.md).
- Without changes to the scrub penalty mechanics, setting the per-minipool minimum RPL stake to 0 would remove the ability to penalize minipools that set incorrect withdrawal credentials. A malicious actor can create many minipools that need to be scrubbed and force the oDAO to scrub all of them without having provided slashable RPL. As long as the oDAO keeps scrubbing minipools, this is purely a griefing attack that is also costly to the attacker. If the oDAO were to run out of gas and not top up their wallets during the scrub period, the attacker could successfully perform a withdrawal credential exploit and steal rETH funds. The proposal therefore includes a change that enables the minipool delegate to apply an equivalent penalty to the node operator's ETH bond instead.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
