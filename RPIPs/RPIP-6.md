---
rpip: TBA
author: Ken Smith (@htimsk) <shtimseht@gmail.com>
status: Draft
type: Protocol
category: Core
discussions-to: https://dao.rocketpool.net/t/unethbonded-ueb-minipools/447 
created: 2022-04-14
---
 
## Abstract
This proposal creates the ability for RP minipools backed solely by 1 ETH and 16.6 ETH worth of RPL. This will allow NO who have a higher percentage of RPL staked to leverage that for greater ETH return by using that RPL as collateral for an unETH-bonded (UEB) minipool. These minipools will have a commission of approximately 1/2 the current commission rate.
 
## Motivation
Currently, there is more demand for node operators than is presently available. Rocket Pool already has more than 1,000 node operators, but they are prevented from forming additional minipools because they lack the financial resources (e.g., 16 ETH). The RPIP has the side effect of reducing in the average node commission.  This occurs because 15 ETH is staked from the deposit pool without the regular stakers having to pay a commission to the node operator.  The commission-free regular ETH stalking helps realign the average RP commission to the targeted 10% commission value. 
At the same time, the RIPIP will benefit NO as it will increased pressure to purchase and stake RPL. The RPOP wlll reward RPL stakers with additional ETH rewards at no cost to the Rocket Pool protocol, as the additional ETH rewards are derived from the beacon chain rewards, not from the RPL inflationary mint. 
 
## Specification
The UEB enabling changes to the smart contract can be implemented using the previous code developed for ON unbonded minipools. The node commission can be applied as 15% to one half (16 ETH) of the minipool even though the deposit pool supplied 32 (or 31) ETH. A small 1ETH deposit might have to be made by the node operator to the deposit pool to register the WC and initiate the srub period.
 
## Rationale
At the same time, there has been discussion that the RP protocol desires to continue to maintain full insurance (or nearly entirely insurance) of the regular stakers contribution by continuing to require that the node operator stake half of the validator's cost. Currently, a NO is required to stake both ETH and RPL in 17.6 ETH.
For example, if your command rocket pool node status returns with: The node has a total stake of 3000 RPL and an effective stake of 3000 RPL, allowing it to run 18 minipool(s) in total. If running with two normal minipool or less, this node would be allowed to run one UEB minipool by using 16.6 of the RPL-permitted minipools as collateral.

## Pros

* It reduces the average node collateral paid by regular stakers. This proposal creates an effective node commission of only 7.74% for the unETH bonded holders.
* Combining these low-commission UEB minipools with the higher-commission regular minipools will help lower the average node commission paid by rETHers.
* One 15% normal minipool (16 ETH) + one 7% UEB minipool (31 ETH) = 10.2% average commission minipool. (Math = 16 ETH at 15% + 16 ETH at 15% + 15 ETH at 0% commission = 10.2%)
* This allows existing node operators to run more minipools.
* It still provides ~100% insurance to back up the regular ETH in the event of a non-validating node that runs the minipool to forced exit status and any envisioned slashing events. 
* It creates even more demand for RPL. Supply and demand should increase the price of RPL.
* UEB minipool might be able to be paired with DVT as well, allowing those with less than 16 ETH worth of RPL to be able to run a partial unETHbonded minipool as well.
* It’s free ETH money to the NO as the RPL will continue to earn RPL staking rewards.
* The NO is compensated for their efforts at the rate of 1/2 the node commission on the minipool. Since this is paid by the core Ethereum protocol there is NO COST to the RP protocol to fund this commission payment.
* No additional CPU load is expected as each node can efficiently run multiple minipools.
* It provides the advantage of unbounded minipools but is not permissioned only to a member of the ON. Any NO with enough RPL staked would be allowed to use that as collateral.

## Cons

* It will have to be post-merge as the focus of the devs on the merge.
* This will have contract changes that will need to be audited.
* We might want to increase the max RPL effective staking to 160% vs. 150%
 
## Backwards Compatibility
This RPIP would be fully compatible with the existing two modalities of RP node formation, the 16-ETH half-minipool, and the 32-ETH half-minipool. 

## Security Considerations
One potential security implication occurs when the node operator performs poorly and exits from the beacon chain with less than 31 ETH. In this edge case, the bonded RPL will need to be auctioned off to recover the lost funds. The amount of ETH paid for the auctioned RPL is dependent on the auction prices, which may or may not be 16 ETH in value. 
 
## Open Questions
* We should evaluate the auction mechanism as the existing one may add risk since it does not guarantee that the auction price of RPL will equal the expected market price of the token.  Since a UEB minipool is backed by  RPL instead of ETH, if we needed to liquidate RPL to cover a UEB minipool that exited with less than 31 ETH would are not guaranteed to cover the market price or RPL to equal the incurred ETH loss. 
* Does this additional demand for RPL threaten RPL liquidity depth? 


## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).