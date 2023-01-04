---
rpip: 
title: Self-limiting Rocket Pool
author: Valdorff (@Valdorff)
status: Draft
type: Meta
discussions-to:  
created: 2022-10-15
---
 
## Abstract
This proposal lays out a set of planned actions to take in response to both degree of dominance and
the staking environment at the time. We believe it's best to capture our ethos early in a concrete
form to make it last well into the future.
 
## Motivation
No entity within Ethereum should have the ability to severely damage the network itself. Despite our
best attempts to be decentralized and secure, we _know_ that, like any entity, there will always be
attack surface. The only thing we can do to ensure the safety of the Ethereum network is to avoid
being a single point of failure by limiting our size.

See also:
- [Lido votes not to self limit](https://snapshot.org/#/lido-snapshot.eth/proposal/0x10abedcc563b66b1adee60825e78c387105110fa4a1e7354ab57bc9cc1e675c2)
  - 99.81% "no, don't self limit"
- [Vitalik suggests a form of self limiting](https://twitter.com/vitalikbuterin/status/1525301234516652032)
- [Superphiz explains their 22% suggestion](https://twitter.com/superphiz/status/1525224461380747268)
- [Stakewise agrees to 22% limit](https://twitter.com/stakewise_io/status/1525225299146944513)
- [Rocket Pool core team agrees with the importance of limiting, but says the decision lies with the community](https://twitter.com/stakewise_io/status/1525225299146944513)

## Specification

### Guiding Principles
- Rocket Pool will act in the best interest of Ethereum health
- Rocket Pool will consider the impacts of its choices on Ethereum, both immediately and long term
- Rocket Pool will prefer to damage itself before endangering the stability of Ethereum
  - Rocket Pool will willingly limit its dominance within staked Ethereum
- Rocket Pool will give its NOs autonomy to make meaningful choices and to leave the RP system if
  they find it unsatisfactory (eg, we currently allow exiting without pre-conditions, and minipool
  upgrades are opt-in)

### Definitions
- RP Dominance - the percentage of staked ETH that is in the Rocket Pool Ecosystem. This counts both
Node Operator ETH (which backs NO share) and user ETH (which backs rETH share).
- Hard limit - turn off the ability to take on more stake (eg, prevent rETH minting)
- Soft limit - discourage taking on more stake and/or encourage more stake elsewhere. For example:
  - Introduce a significant fee to mint rETH
  - Introduce a significant fee to create minipools
  - Decrease marketing budget
  - Decrease incentives budget
  - All the above free up money; this can be used to:
    - Create pDAO value - eg, hold assets in treasur
    - Return value to existing shareholders - eg, burn RPL
    - Further encourage healthy staking - eg, use pDAO funds to provide liquidity or marketing for a
      competing "as good" or "healthier" staking protocol

Staking category definitions:
- Healthier staking - a method of staking that is healthier for Ethereum than RP; for example, solo
  staking is less susceptible to governance attacks, as there is no governance
- As good staking - a method of staking that is similarly healthy to RP; this counts RP itself
- Not as good staking - a method of staking that is not as healthy as RP. For example, it may be more
  likely to act monolithically, may only allow permissioned Node Operators, or their governance may
  be easier to attack
- Unhealthy staking - a method of staking that is seriously dangerous to Ethereum's health; for
  example, a large company that has control of large stake. Their decisions are completely
  centralized and easily subjected to regulation.

In the interest of concreteness, we'll categorize some existing players as they exist today:
- Healthier: solo staking
- As good: Rocket Pool, Stakewise v3? (not yet live)
- Not as good: Frax (permissioned NOs, misleading yield numbers using 2-token system)
- Unhealthy: Coinbase (centralized, 13% of staked ETH), Lido (29% of staked ETH, permissioned NOs, 
  explicitly aiming for winner take all), Kraken (centralized, 7.5% of staked ETH), Binance
  (centralized, 6.4% of staked ETH)

### Other considerations
- There is a danger of a single entity that spans multiple staking protocols. This could easily be
  an example of unhealthy staking, but may be a little harder to see in data. Be careful to think
  through the context fully for this kind of thing.
- Governance power distribution - how many wallets does it take to pass a measure? Are the
  incentives of the voters aligned with Ethereum health? Are there perverse incentives where voters
  must choose between helping themselves and Ethereum?
- Governance structure - who gets to vote? how does voting power scale? what safety nets are in
  place to avoid abuse? What friction is there for changes (time delays, some immutables, etc)? Are
  there any checks or balances?
- Suceptibility to governance attack - how expensive would it be to buy enough vote power to pass an
  arbitrary proposal? how about bribe existing players?
- One consideration: if there aren't healthy competitors and we're dominant, we could spin/fork off
  a competitor. This is technically quite possible. That said, it would be _extremely_ challenging
  to cut ties fully enough to make this healthy. Be careful if considereing this path!
- Beware of security theater or governance theater -- just because something claims to X, doesn't
  mean it achieves X!


### Examples
Scenario: "Healthier staking" and "As good staking" represents ≥2/3 of all staking
- RP SHOULD soft limit starting at 16% RP dominance, and do so more aggressively at greater RP
  dominance
  - Eg: introduce a 3% fee to mint rETH, rising by 2% per percentage point of RP dominance
  - Eg: redirect 10% of pDAO income to supporting small permissionless protocols, rising by 10% per
    percentage point of RP dominance
- RP SHOULD hard limit at 22% RP dominance

Scenario: "Unhealthy staking" represents ≥1/2 of all staking
- RP SHOULD soft limit starting at 22% RP dominance, and do so more aggressively at greater RP
  dominance
  - Eg: introduce a 3% fee to mint rETH, rising by 2% per percentage point of RP dominance
  - Eg: redirect 10% of pDAO income to supporting small permissionless protocols, rising by 10% per
    percentage point of RP dominance
- RP SHOULD hard limit at 33% RP dominance
- The focus in this scenario should be to grow ourselves, then grow healthy alternatives ASAP

## Rationale
67% is the minimum needed for finality.
All the other numbers are based on that and the hope that other healthy parties will work with us,
eg, 22% means that 3 parties of that size would be insufficient to create any desired finality.

## Further considerations
Note that the specification is entirely SHOULD. There are _no_ MUST statements. This is because
context matters, and we cannot foresee the future perfectly.

For example, perhaps the key thing to ETH health is related to MEV and block building, or perhaps
it's related to censoring, or \<fill in your idea from your imagination\>. This document is meant to
serve as a statement of Guiding Principles, along with some basic concrete examples to help anchor
those guiding principles.


## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).