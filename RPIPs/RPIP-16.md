---
rpip: 16
title: Change Protocol Settings for Atlas 
description: Increase deposit pool maximum and decrease ETH held in the rETH contract
author: Ken Smith (@htimsk), Valdorff (@Valdorff), Darren Langley (@darrenlangley)
discussions-to: https://dao.rocketpool.net/t/proposed-protocol-settings-for-atlas-release/1367
status: Draft
type: Protocol
category: Core
created: 2023-01-17
---

## Abstract
This proposal is to modify two settings after the upcoming Atlas release, leading up to Ethereum's
release of withdrawals.

## Motivation
The settings will be changed to:
- allow pent up demand to build up in the deposit pool, in expectation of large supply
- keep maximum drag in mind
- allow easier motion of ETH from the rETH contract to the deposit pool
  - doesn't impact burning much, as that is often bot-driven
  - allows supply side to be paired more rapidly to minimize unproductive protocol ETH

We believe Atlas will increase the supply rate, as we will be more efficient at minting rETH per
unit of Node Operator investment. After withdrawals, we expect a significant one-time increase in
supply, and we'd like to ensure we have sufficient demand such that none of that supply is "lost" to
competing options. The large one-time increase will be driven by migrations internally from
minipools with 16 ETH bonds to LEBs, and externally from solo validator migrations and folks with
funds freeing up that were locked on CEXes.

## Specification
- The pDAO guardian SHALL set `network.reth.collateral.target` pDAO setting to .001 ether (10000000000000000)
- The pDAO guardian SHALL set `deposit.pool.maximum` pDAO setting to 5000 ether (5000000000000000000000)
- The pDAO guardian SHOULD carry out the above actions 10 days before Ethereum withdrawals are expected to go live
- The oDAO and RP dev team SHOULD work together to arbitrage the price difference when the maximum is increased
  - the value from the arbitrage SHALL be given to rETH holders 

## Rationale
Some level of `network.reth.collateral.target` is desirable to allow:
- NO voluntary exit arbitrage when there is a discount
- to allow arb bots to burn rETH for ETH to help restore supply/demand balance (as opposed to using
  that ETH to start more minipools, which further imbalances supply/demand)
- to prevent overflowing the deposit pool at a premium, which damages the NO minipool creation arb

The first only needs 32 ETH for the former to support any possible minipool size. The second just
needs "enough" space to do the arb as rewards come in; note that the largest single reward seen so
far (smoothing pool interval 3) was under 0.1% of rETH TVL, so the chosen size should be sufficient.
Finally, the last purpose can get arbitrarily large. Here we look to balance the use of arb with a
potential griefing method by repeated minipool creation and exiting; by selecting a relatively low
value we limit the potential downside of such griefing.

--

A large `deposit.pool.maximum` is desireable because:
- when full, we can capture rapid supply growth
- when empty, we can capture rapid demand growth
However, a large `deposit.pool.maximum` can cause APR drag when full.

We have previously talked about 5% of rETH TVL as being a reasonable balance as a result. At this
time, we're around 2.7% of rETH TVL. LEB8s in Atlas increase the rETH value minting capacity of each
unit of ETH by 3x (assuming the NO already has RPL; ~2.5x if the RPL needs to be purchased). With
withdrawals, existing NOs will be able to migrate to LEB8s. Additionally, solo stakers or folks with
ETH currently "trapped" on a CEX will be able to join RP. We expect a large single instance of
supply growth, so we (a) value a large deposit pool to capture it and (b) believe it will be a
conservative percentage of TVL after migrations occur.

--

10 days before withdrawals was recommended to have a minimal impact on rETH APR, while providing
enough time for rETH demand to build up before the large jump in supply.

--

When the maximum deposit is increased, there will be the ability to mint rETH from the deposit
contract. rETH can currently be sold at premium market prices, generating a profit. If this profit
is not captured by RP, it _will_ be rapidly captured by arbitrage bots and lucky validators as MEV.
We would prefer that this value be instead captured and returned to rETH holders, as the most likely
"damaged" party by the recent high premium. Recent estimates indicated 20 ETH in profit in the arb,
but note that this is very dynamic.

----------



## Backwards Compatibility
No backwards incompatibility.

## Test Cases
This is a very low risk change. The deposit pool limit setting has previously been used. We
recommend switching to these settings ahead of time on Goerli testnet to ensure it has the desired
effect. A post verification check will be performed after deployment - we will query the Protocol
DAO contract for the values and check Rocket Scan.

## Reference Implementation
Not applicable.

## Security Considerations
Increasing the deposit pool limit mildly increases the possibility for a flashloan style attack but
for this to occur there would have to be an exploit present that yielded some value: no known
exploit exists that would yield value and this has been carefully considered by auditors. It does
make arbitarge opportunities slightly more profitable but the deposit pool is still limited and so
these are very mild concerns.  

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
