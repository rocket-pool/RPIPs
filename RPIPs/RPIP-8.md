---
rpip: 8
title: LEB8s
description: A new Lower ETH Bond size of minipool with 8 ETH from the node operator
author: Valdorff (@Valdorff)
discussions-to: https://dao.rocketpool.net/t/leb8-discussion-thread/899
status: Final
type: Protocol
category: Core
created: 2022-08-05
---

## Abstract
This proposal:
- Creates a new type of minipool that is launched with 8 Node Operator ETH.
- Sets commission for new minipools to 14% on matched protocol ETH.
- Establishes that the minimum RPL stake to launch a minipool or receive RPL rewards is 10% of
  matched protocol ETH on a per-node basis.
- \<Line to be added based on "max effective" stake vote\>
- Creates a process by which currently existing minipools can be converted.

## Motivation
There are 3 interconnected parties in Rocket Pool: NOs (node operators), RPL token holders, and rETH
token holders.

NOs get greater APR for their investment due to increased capital efficiency.

RPL token holders retain the same relationship between rETH TVL and RPL price that currently
exists. This proposal also supports expanding rETH TVL greatly by (A) being more attractive for NOs
and (B) allowing the same NO investment to mint much more rETH. Instead of requiring a 17.6 ETH NO
investment to mint 16 ETH's worth of rETH, we can now mint 24 ETH's worth of rETH with a 10.4 ETH NO
investment. This is ~2.5x more rETH minted per unit of NO investment. The hope is that this effect,
alongside other RPL efforts, is enough to keep the protocol limited by only rETH demand and not by
NO availability.

rETH holders can have their demand for new rETH met due to increased NO availability and
efficiency. Additionally, a portion of the NOs capital efficiency is being passed on to rETH holders
in the form of (A) reduced commission and (B) incentive for pre-existing high-commission minipools
to switch to a lower commission.

We believe all parties are "winning" with this new option.

## Specification
- The new type of minipool SHOULD be called LEB8.
- LEB8s SHALL require 8 Node Operator ETH to start.
- To launch a minipool, a node MUST have at least 10% of matched Protocol ETH in staked RPL value
  - This applies to all minipools, not just LEB8s.
  - For a node with a single half deposit or full deposit minipool, this would mean 1.6 ETH in 
    staked RPL value.
  - For a node with a single LEB8 minipool, this would mean 2.4 ETH in staked RPL value.
- Nodes MUST have RPL valued at 10% or more of matched Protocol ETH to qualify for RPL rewards.
- \<Line to be added based on "max effective" stake vote\>
- Effectively staked RPL SHALL be used when calculating RPL rewards and voting power. 
- There SHALL be a method to migrate from an existing half or full-deposit minipool to an LEB8.
  - Node operators that migrate SHALL receive an ETH credit that can be used towards starting more
    minipools; that credit SHALL NOT be directly able to be withdrawn.
  - Migration from higher commission minipools SHOULD be prioritized if there is any kind of queue
    for migration.

### Max effective stake vote
Status: not yet voted

#### Table summarizing options
Extended to include hypothetical LEB4s.
| **Option Name**                  | **Max effective RPL stake ** | **Minipool16<br>**(min-max effective) | **LEB8**<br>(min-max effective)       | **Hypothetical LEB4**<br>(min-max effective)       |
|----------------------------------|------------------------------|---------------------------------------|---------------------------------------|----------------------------------------------------|
| Scale with NO ETH                | `1.5*NO_ETH`                 | 1.6-24                                | 2.4-12                                | 2.8-6                                              |
| Flat number of ETH per validator | `24`                         | 1.6-24                                | 2.4-24                                | 2.8-24                                             |
| Scale with Protocol ETH          | `1.5*Protocol_ETH`           | 1.6-24                                | 2.4-36                                | 2.8-42                                             |

#### Suggested Snapshot
```
# META
Title: Max Effective Stake for LEB8s (RPIP-8)
Voting type: Approval voting

Options:
- Scale with NO ETH (12 ETH)
- Flat number of ETH per validator (24 ETH)
- Scale with Protocol ETH (36 ETH)
- Remove LEB8s from Atlas
- Abstain
```

```
This vote is to specify the max effective stake for LEB8s.

Here's a table showing how the proposed strategies look for the minipool sizes that will be available in Atlas:

| **Option Name**                  | **Max effective RPL stake**  | **Minipool16** (min-max effective)    | **LEB8** (min-max effective)          |
|----------------------------------|------------------------------|---------------------------------------|---------------------------------------|
| Scale with NO ETH                | 1.5*NO_ETH                   | 1.6-24                                | 2.4-12                                |
| Flat number of ETH per validator | 24                           | 1.6-24                                | 2.4-24                                |
| Scale with Protocol ETH          | 1.5*Protocol_ETH             | 1.6-24                                | 2.4-36                                |


Note that while this vote is meant to establish our design moving forward, there are other ideas being actively pursued. As with the rest of RP - the DAO may vote for further changes in the future as it sees fit.

This vote uses _approval voting_. This means you can vote for one or more options, with each selection getting your full voting power. The intent is for voters to select all choices that they approve of (if multiple), instead of limiting them to their top preference.

### Further Reading
- Full LEB proposal: [RPIP-8](https://github.com/Valdorff/RPIPs/blob/max_collateral_leb8/RPIPs/RPIP-8.md)
- [An NO ETH centered proposal, with reasoning](https://dao.rocketpool.net/t/max-collateral-for-lebs/1166?u=valdorff)
- [A protocol ETH centered proposal, with reasoning](https://dao.rocketpool.net/t/an-argument-in-favor-of-protocol-eth/1208)
- A flat number of ETH per validator is proposed here as a compromise position between the above two
```

#### Updating this RPIP
After the conclusion of the snapshot:
- The "Specification"/"Abstract" sections SHALL be updated to reflect the chosen option
- Status in "Max effective stake vote" SHALL be updated to reflect the vote result
- This edit SHALL be made to this RPIP, even if the RPIP is already in a "Final" state

After LEB8 implementation(s):
- If the implementation matches the chosen option from the snapshot vote:
  - The "Max effective stake vote" section SHALL be removed and the
- If the implementation does not match the chosen option from the snapshot vote:
  - Status in "Max effective stake vote" SHALL be updated to reflect this mismatch
- This edit SHALL be made to this RPIP, even if the RPIP is already in a "Final" state

## Rationale
Based on the `NO supply growth` model in the 
[discussion summary](../assets/rpip-8/2022-08-03%20LEB%20Discord%20Discussion%20Summary.pdf),
we are likely to face NO supply limitations in the next year, and perhaps ever.

Given that, based on the `Value of rETH as a function of LEB type` model in the
[discussion summary](../assets/rpip-8/2022-08-03%20LEB%20Discord%20Discussion%20Summary.pdf),
we conclude that the relationship between RPL value and rETH TVL is only preserved when the value of
the minimum RPL stake remains at 10% of the matched Protocol ETH.

We are opting for LEB8s, instead of e.g., 4-ETH LEBS, for a few reasons. First, there is much more safety
margin in an adversarial scenario (e.g., MEV theft) -- it's not clear if this is necessary, but it
feels appropriate to err on the side of caution while forced validator withdrawal isn't yet
available. Second, based on the `NO supply growth` model, it appears 8-ETH LEBs are able to meet
the rETH demand we expect, with a significant buffer. Third, implementing a stepped approach will allow us
to confirm our understanding or reevaluate it before moving to a smaller-sized LEB.

Thus far, that gets us to LEBs with "8 Node Operator ETH and at least 2.4 ETH worth of RPL".

Commission had many different perspectives. How much does commission impact real-world LSD buyers?
How much does commission impact potential NOs? Given that we're gaining capital efficiency, is there
a "fair" split of that efficiency between NOs and rETH holders?

In the end, we were driven by a very practical purpose. Per the `Commission to RPL stake
relationship` model in the
[discussion summary](../assets/rpip-8/2022-08-03%20LEB%20Discord%20Discussion%20Summary.pdf),
we find that NOs with 20% commission 16-ETH minipools will get the same ETH APR if they switched to
8-ETH minipools with 13.94% commission. We went with 14% as the maximum that could be decreased
while aligning the self-interest of those NOs with the health of the protocol. Almost 1/3 of all
current minipools have a 20% commission, so this is a significant impact on commission in the near
term. Since this is close to 15%, folks that didn't believe the commission needed to change at all were
also able to agree pragmatically.

The community decided most of the design of LEB8s before snapshot voting.
One thing that was mentioned in the
[discussion document](../assets/rpip-8/2022-08-03%20LEB%20Discord%20Discussion%20Summary.pdf) as a
next step was determining the definition of "effective". Max effective RPL staked had several
viewpoints. Most parties involved wanted to make the transition to LEB8s with the minimum possible
overall impact on the protocol; however, different parties had different perspectives on what that
meant. Scaling preferences ranged from protocol ETH, to NO ETH, to fixed per validator. See the
discussion on the
[forum thread proposing primarily NO ETH scaling](https://dao.rocketpool.net/t/max-collateral-for-lebs/1166?u=valdorff), 
the [forum thread proposing primarily Protocol ETH scaling](https://dao.rocketpool.net/t/an-argument-in-favor-of-protocol-eth/1208),
and in a rawer form on
[discord](https://discord.com/channels/405159462932971535/774497904559783947/1033959349715996702)
both in the thread that links to and in the #governance channel starting at that point. This will be
resolved by Snapshot vote, with this RPIP explicitly saying it should be updated after the vote.

### Future changes
We want to _explicitly_ address that the parameters in this proposal were chosen as best we could, 
based on assumptions and models about the future. Those could be wrong. If they are, we should
be willing to revisit these numbers.

We believe RPL holders will be willing to self-efface the relationship between rETH TVL and RPL
value if doing so is the only way to preserve protocol health. After all, if the protocol is 
unhealthy or heavily outcompeted, that will drop rETH TVL and damage RPL value (even to 0).

To a lesser degree, we believe NOs will be willing to self-efface their APR if doing so is the
only way to preserve protocol health. If they do not do this and rETH value is damaged, there will
be arbitrage opportunities for NOs as they exit. This will either reduce NO supply until it properly
matches rETH demand or in the extreme, mean all NOs exit. Since NOs want to earn APR, this is
something they wish to avoid. Further, since NOs hold RPL, they don't want to damage its value.

In practical terms, what does this mean?

- The commission is a tradeoff between NOs and rETH token holders.
- RPL minimum is a tradeoff between NOs and RPL token holders.
- RPL minimum sets the minimum commission before NOs will do better just solo staking.
- RPL value is also dependent on rETH TVL.

If our NO supply is limited, RPL holders must likely yield some value by reducing the
minimum RPL stake required. Due to our locking mechanism, we would probably need a clever solution 
that allows for the one-time removal of RPL even in the "locked" range.

If we find that LSD holders become highly responsive to the commission, we _must_ react by reducing
commission. If there's plentiful NO supply, this can be done directly. If doing this would threaten
our NO supply, we must _also_ reduce our minimum RPL stake required per the previous paragraph.

### Future related topics
During discussion about the maximum effective RPL stake, there were multiple interesting ideas for
how rewards could be provided differently, including: rewards per minipool (this is similar to 
having the maximum effective stake equal the minimum required stake), rewards scale sub-linearly
from the minimum (eg, square root). There was also thoughts about how to use RPL beyond the minimum
required within the protocol including UEBs (un eth bonded minipools) and for security in case of
black swan events (this would require significantly greater liquidity and access to that liquidity
before the market).

These ideas deserve more thought, but are being deemed out of scope. If we decide to move forward
with one of these ideas it should be a separate RPIP and vote.

## Backwards Compatibility
There are no issues with backwards compatibility.

Please note that this is an _additional_ option, half deposit minipools (16 ETH) will continue to be
available for folks that wish to minimize their exposure to RPL. Note that these will use the new
14% commission from now on.

## Test Cases
TBD

## Security Considerations
Per a [pre-existing model](../assets/rpip-8/Analysis%20of%20LEB%20Minipools.pdf) we have previously
concluded that we are "safe enough" from both long con and lottery attacks at 5.6 ETH of NO deposit.
That said, we have a variant attack that combines those that have not been looked into, and we accept
that our ability to predict future MEV patterns is limited. The current proposal almost doubles the
NO deposit, greatly increasing our safety margin.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
