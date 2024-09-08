---
rpip: 62
title: Tokenomics Rework Prelude
description: This proposal aims to improve RPL tokenomics in the short term to unlock protocol growth.
author: haloooloolo (@haloooloolo), knoshua (@knoshua)
discussions-to: https://dao.rocketpool.net/t/rpip-62-tokenomics-rework-prelude/3238
status: Draft
type: Protocol
category: Core
created: 2024-07-25
---

## Abstract
This proposal aims to improve RPL tokenomics in the short term before the changes of [RPIP-49](RPIP-49.md) can be implemented.
Minipools can be created without a minimum RPL requirement and at 5% contract commission. A temporary (until after [Saturn 1](RPIP-55.md)) dynamic commission boost beyond this value is introduced. Total dynamic commission starts at 10% and scales linearly with RPL stake up to 14% at 10% of borrowed ETH.
The cliff for RPL rewards is <removed by extending rewards linearly below 10% | explicitly maintained at 10%> of borrowed ETH. Scrub penalties are changed to be taken out of the node operator's ETH bond instead of slashing RPL.

## Motivation
With the DAO having voted for the Saturn upgrade, the fundamental value of RPL will primarily be based on megapool TVL.
Short term increases in TVL are beneficial for the protocol as long as they can be expected to convert to megapools.
On the other hand, as competition emerges and an equivalent or higher yield  is accessible without the need to acquire a protocol token, short-term RPL utility is unlikely to continue to significantly support fundamental value.
Therefore, node operation is made more attractive by allowing minipool creation without RPL <and removing the cliff for RPL rewards | >.
Contract commission for these new ETH-only minipools is kept less attractive than megapool validators under [Saturn 1](RPIP-55.md) to encourage migration once dynamic commission is disabled.
In the interest of acting fast, this proposal minimizes smart contract changes. The suggested parameter changes can be enacted immediately after the vote passes and changes to the scrub penalty are implemented. The dynamic commission requires reward tree spec changes, which would be rolled out alongside the parameter changes or shortly thereafter.

## Specification
- `node.per.minipool.stake.minimum` SHALL be set to 0
- `network.node.fee.target` SHALL be set to 5%
- `network.node.fee.minimum` SHALL be set to 5%
- `network.node.fee.maximum` SHALL be set to 5%
- The scrub penalty SHALL be set to 2.4 ether and made to be withheld from an offending minipool's bond
- [Reward Tree Spec v10](../assets/rpip-62/rewards-calculation-spec.md) SHALL be implemented and be used for ongoing reward tree calculations. It consists of the following changes:
  - <Remove | Decouple> the minimum RPL stake to qualify for issuance rewards < | from `node.per.minipool.stake.minimum` by setting it to a constant 10%>
  - For minipools that are opted into the smoothing pool, determine the commission for smoothing pool calculations based on ETH bond and RPL stake:
    - For 8 ETH minipools at reward snapshots prior to [Saturn 1](RPIP-55.md) and the first **4** snaphots thereafter, use `commission = max(contract_commission, 10% + 4% * min(1, percent_of_borrowed_ETH / 10))`
    - For 16 ETH minipools and later snapshots, use `commission = contract_commission`
  - For the same minipools as above, calculate their individual beacon chain rewards during the rewards period and give them a bonus (`node_reward_bonus`) based on `bonus_commission = commission - contract_commission`
  - If the smoothing pool balance is not sufficient to cover the beacon reward bonus for all minipools (`total_reward_bonuses`):
    - Fully credit the adjusted smoothing pool rewards excluding `node_reward_bonus` to all nodes
    - Credit modified reward bonuses as `node_reward_bonus * (remaining_balance / total_reward_bonuses)`
- [Reward Tree Spec v9](RPIP-52.md) or [Reward Tree Spec v8](RPIP-51.md) MAY be used for reward periods ending before [Reward Tree Spec v10](../assets/rpip-62/rewards-calculation-spec.md) is available; this SHOULD affect no more than one reward submission after the vote for this proposal ends

### Snapshot Vote
The following configuration is suggested for a vote:
```
Title: Tokenomics Rework Prelude (RPIP-62)
Voting type: Ranked choice voting

Options:
- For variant A (keep reward cliff)
- For variant B (remove reward cliff)
- Against both variants
```
After the conclusion of the Snapshot vote:
- Sections of this RPIP in the format `<A|B>` SHALL be updated to reflect the chosen option:
  - If `For - Remove RPL Reward Cliff` is chosen, replace `<A|B>` with `A`
  - If `For - Maintain RPL Reward Cliff` is chosen, replace `<A|B>` with `B`
  - If the proposal does not succeed, do not make any edits
- These edits SHALL be made to this RPIP, even if the RPIP is already in a “Final” state

## Rationale
### Megapool Conversions
The base commission (without dynamic commission) is lowered to 5% such that megapool validators introduced in [RPIP-49](RPIP-49.md) remain significantly more attractive. As a reference point, the returns for ETH-only users in [Saturn 1](RPIP-55.md) would match the returns from minipools in this RPIP if `node_operator_commission_share` were set to 2.14%; [Saturn 1](RPIP-55.md) starts `node_operator_commission_share` at 5%. We expect that operators with ETH-only minipools described in this RPIP will eventually migrate to megapools and thus contribute to RPL value capture.

### Dynamic Commission Eligibility
Requiring smoothing pool participation to receive dynamic commission provides a strong expectation that the end-of-interval balance will be sufficient to cover the full reward bonus for all qualifying minipools.

To understand when there wouldn't be enough, we look at a worst case scenario: every minipool is eligible to receive a total commission of 14% due to their parent node's RPL stake while earning a contract commission of only 5%. This makes 86% of the pool's pETH execution yield available to pay out a reward bonus that comprises 9% of pETH consensus yield. In this case, the balance will remain sufficient as long as execution rewards are less than `86/9 = 9.55` times lower than consensus rewards, or more concretely, provide an APR of at least `2.8/9.55 = 0.29%` with annual consensus layer yield at 2.8%. This is less than half of the current [network average](https://explorer.rated.network/network?network=mainnet&timeWindow=30d&rewardsMetric=average&geoDistType=all&hostDistType=all&soloProDist=stake). Even though the expectation is that it won't be used, the specification _does_ provide a fallback definition in case bonus commission cannot be fully covered.

16 ETH minipools are made ineligible for dynamic commission in the interest of further incentivizing rETH capacity.

## Considerations
### Supporting Protocols
- [NodeSet Constellation](https://nodeset.medium.com/project-hyperdrive-4819f22391dc) enables liquid RPL staking (xRPL) and liquid ETH staking with full solo validator yield (xrETH) by building on top of the core protocol. This has the potential to increase node operator TVL by attracting market participants that do not wish to lock their capital in a node. As Constellation's revenue is mainly derived from ETH commission, sustaining viability requires maintaining the ability to earn high ETH commission on minipools. Dynamic commission addresses this by offering 14% total commission at an RPL collateralization level of 10%.
- [Rocket Lend](https://github.com/rocketlend/protocol) is a Rocket Pool specific lending protocol funded by the GMC. It enables NOs to use their ETH bond as collateral to borrow RPL and stake it on their node. This proposal will likely make borrowing RPL less appealing as the marginal ETH yield it unlocks is reduced. However, the proposal may also onboard new node operators that will be interested in participating in the lending market. As such, Rocket Lend can still serve an important role in increasing capital efficiency for both lenders and borrowers.

### Security
- The minimum collateral per minipool is lowered from 10.4 ETH to 8 ETH at time of creation. This is sufficient as it is still significantly higher than the 4 ETH validator bond proposed in [RPIP-49](RPIP-49.md).
- Without changes to the scrub penalty mechanics, setting the per-minipool minimum RPL stake to 0 would remove the ability to penalize minipools that set incorrect withdrawal credentials. A malicious actor can create many minipools that need to be scrubbed and force the oDAO to scrub all of them without having provided slashable RPL. As long as the oDAO keeps scrubbing minipools, this is purely a griefing attack that is also costly to the attacker. If the oDAO were to run out of gas and not top up their wallets during the scrub period, the attacker could successfully perform a withdrawal credential exploit and steal rETH funds. The proposal therefore includes a change that enables the minipool delegate to apply an equivalent penalty to the node operator's ETH bond instead.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
