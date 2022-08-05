---
rpip: 8
title: LEB8s
description: A new Lower ETH Bond size of minipool with 8 ETH from the node operator
author: Valdorff (@Valdorff)
discussions-to: TODO - get URL from forum
status: Draft
type: Protocol
category: Core
created: 2022-08-05
---

## Abstract
This proposal:
- Creates a new type of minipool that is launched with 8 Node Operator
- Sets commission for new minipools to 14% on matched Protocol ETH
- Establishes that effective RPL stake is calculated as 10-150% of matched Protocol ETH on a
  per-node basis
- Creates a process by which currently existing minipools can be converted

## Motivation
There are 3 interconnected parties in Rocket Pool: NOs (node operators), RPL token holders, and rETH
token holders.

NOs get greater APR for their investment due to increased capital efficiency.

RPL token holder retain the same relationship between rETH TVL and RPL price that currently
exists. This proposal also supports expanding rETH TVL greatly by (A) being more attractive for NOs
and (B) allowing the same NO investment to mint much more rETH. Instead of requiring 17.6 ETH NO
investment to mint 16 ETH's worth of rETH, we can now mint 24 ETH's worth of rETH with a 10.4 ETH NO
investment. This is ~2.5x more rETH minted per unit of NO investment. The hope is that this effect,
alongside other RPL efforts, is enough to keep the protocol limited by only rETH demand, and not by
NO availability.

rETH holders are able to have their demand for new rETH met due to increased NO availability and
efficiency. Additionally, a portion of the NOs capital efficiency is being passed on to rETH holders
in the form of (A) reduced commission and (B) incentive for pre-existing high-commission minipools
to switch to a lower commission.

We believe all parties are "winning" with this new option.

## Specification
- The new type of minipool SHOULD be called LEB8
- LEB8s SHALL require 8 Node Operator ETH to start
- To launch a minipool, a node MUST have at least 10% of matched Protocol ETH in staked RPL value
  - This applies to all minipools, not just LEB8s
  - For a node with a single half deposit or full deposit minipool, this would mean 1.6 ETH in 
    staked RPL value
  - For a node with a single LEB8 minipool, this would mean 2.4 ETH in staked RPL value
- RPL between 10% of matched Protocol ETH and 150% of matched protocol ETH (inclusive) SHALL be
  considered effectively staked (e.g. for RPL rewards and governance)
- There SHALL be a method to migrate from an existing half or full deposit minipool to an LEB8
  - Node operators that migrate SHALL receive an ETH credit that can be used towards starting more
    minipools; that credit SHALL NOT be directly able to be withdrawn
  - Migration from higher commission minipools SHOULD be prioritized if there is any kind of queue
    for migration

## Rationale
Based on the `NO supply growth` model in the 
[discussion summary](../assets/rpip-8/2022-08-03%20LEB%20Discord%20Discussion%20Summary.pdf),
we do not believe we are likely to face NO supply limitations in the next year, and perhaps ever.

Given that, based on the `Value of rETH as a function of LEB type` model in the
[discussion summary](../assets/rpip-8/2022-08-03%20LEB%20Discord%20Discussion%20Summary.pdf),
we conclude that the relationship between RPL value and rETH TVL is only preserved when the value of
the minimum RPL stake remains at 10% of the matched Protocol ETH.

We are opting for 8-ETH LEBs, instead of e.g. 4, for a few reasons. First, there is much more safety
margin in an adversarial scenario (e.g. MEV theft) -- it's not clear if this is necessary, but it
feels appropriate to err on the side of caution while forced validator withdrawal isn't yet
available. Second, based on the `NO supply growth` model, it appears 8-ETH LEBs are able to meet
the rETH demand we expect, with a significant buffer. Third, doing a stepped approach will allow us
to confirm our understanding and/or reevaluate it before moving to a smaller sized LEB.

Thus far, that gets us to LEBs with "8 Node Operator ETH an at least 2.4 ETH worth of RPL".

Commission had many different perspectives. How much does commission impact real-world LSD buyers?
How much does commission impact potential NOs? Given that we're gaining capital efficiency, is there
a "fair" split of that efficiency between NOs and rETH holders?

In the end, we were driven by a very pragmatic purpose. Per the `Commission to RPL stake
relationship` model in the
[discussion summary](../assets/rpip-8/2022-08-03%20LEB%20Discord%20Discussion%20Summary.pdf),
we find that NOs with 20% commission 16-ETH minipools will get the same ETH APR if they switched to
8-ETH minipools with 13.94% commission. We went with 14% as the maximum that could be decreased
while align the self-interest of those NOs with the health of the protocol. Almost 1/3 of all
current minipools have a 20% commission, so this is a significant impact on commission in the near
term. Since this is close to 15%, folks that didn't believe commission needed to change at all were
also able to pragmatically agree.

### Future changes
We want to _explicitly_ address that the paramaters in this proposal were chosen as best we could, 
based on assumptions and models about the future. Those could be wrong. If they are, we should
be willing to revisit these numbers.

We believe RPL holders will be willing to self-efface the relationship between rETH TVL and RPL
value, if doing so is the only way to preserve protocol health. After all, if the protocol is 
unhealthy or heavily outcompeted, that will drop rETH TVL and damage RPL value (even to 0).

To a lesser degree, we believe NOs will be willing to self-efface their APR, if doing so is the
only way to preserve protocol health. If they do not do this and rETH value is damaged, there will
be arbitrage opportunities for NOs as they exit. This will either reduce NO supply until it properly
matches rETH demand, or in the extreme mean all NOs exit. Since NOs want to earn APR, this is
something they wish to avoid. Further, since NOs hold RPL, they don't want to damage its value.

In practical terms, what does this mean?

- Commission is a tradeoff between NOs and rETH token holders
- RPL minimum is a tradeoff between NOs and RPL token holders
- RPL minimum sets the minimum commission before NOs will do better just solo staking
- RPL value is also dependent on rETH TVL

If we find that our NO supply is limited, RPL holders must likely yield some value by reducing the
minimum RPL stake required. Due to our locking mechanism, we would likely need a clever solution 
that allows for one-time removal of RPL even in the "locked" range.

If we find that LSD holders become extremely responsive to commission, we _must_ react by reducing
commission. If there's plentiful NO supply, this can be done directly. If doing this would threaten
our NO supply, then we must _also_ reduce our minimum RPL stake required per the previous paragraph.

## Backwards Compatibility
There are no issues with backwards compatibility.

Please note that this is an _additional_ option, half deposit minipools (16 ETH) will continue to be
available for folks that wish to minimize their exposure to RPL. Note that these will use the new
14% commission going forward.

## Test Cases
TBD

## Security Considerations
Per a [pre-existing model](../assets/rpip-8/Analysis%20of%20LEB%20Minipools.pdf) we have previously
concluded that we are "safe enough" from both long con and lottery attacks at 5.6 ETH of NO deposit.
That said, we have a variant attack that combines those that has not been looked into, and we accept
that our ability to predict future MEV patterns is limited. The current proposal almost doubles the
NO deposit, which greatly increases our safety margin.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
