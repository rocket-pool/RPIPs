---
rpip: 
title: Self-limiting Rocket Pool
author: Valdorff (@Valdorff)
status: Draft
type: Meta
discussions-to: https://dao.rocketpool.net/t/self-limiting-principles/1396/17
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
- [Rocket Pool core team agrees with the importance of limiting, but says the decision lies with the community](https://twitter.com/langerstwit/status/1525343785219018752)

## Specification

### Guiding Principles
- Rocket Pool will act in the best interest of Ethereum health
- Rocket Pool will consider the impacts of its choices on Ethereum, both immediately and long term
- Rocket Pool will prefer to damage itself before endangering the stability of Ethereum
  - Rocket Pool’s growth should be improving the decentralization of Ethereum, and RP should limit itself if it detracts from it
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
- Healthier staking - a method of staking that is significantly healthier for Ethereum than RP
- As good staking - a method of staking that is roughly as healthy for Ethereum as RP
- Not as good staking - a method of staking that is significantly less healthy for Ethereum than RP
- Unhealthy staking - a method of staking that is seriously dangerous to Ethereum's health

In the interest of concreteness, we'll categorize some existing players as they exist today:
- Healthier: solo staking
- As good: Rocket Pool, Stakewise v3? (not yet live)
- Not as good: Frax (permissioned NOs, misleading yield numbers using 2-token system)
- Unhealthy: Coinbase (centralized, 13% of staked ETH), Lido (29% of staked ETH, permissioned NOs, 
  explicitly aiming for winner take all), Kraken (centralized, 7.5% of staked ETH), Binance
  (centralized, 6.4% of staked ETH)

### Judging staking categories
Here are some things to consider when judging "healthiness". In a case where we're getting close to
a relevant dominance level, we should do our best to judge ourselves and other protocols on a level
playing field. One possible sanity check is asking trusted Ethereum community members to participate
in rating as well, to ensure that we are not swayed too strongly by our community's biases.

- Governance power distribution - how many wallets does it take to pass a measure? Are the
  incentives of the voters aligned with Ethereum health? Are there perverse incentives where voters
  must choose between helping themselves and Ethereum?
- Governance structure: Who gets to vote? How does voting power scale? What safety nets are in
  place to avoid abuse and can they be bypassed? What friction is there for changes (time delays,
  some immutables, etc)? Are  there any checks or balances?
- Governance takeover risk: How expensive would it be to buy enough vote power to pass an arbitrary
  proposal? How expensive would it be to bribe existing players?
- Hidden large entities: What methods can be used to detect large entities within the protocol if
  they try to avoid detection? What methods can be used to detect large entities operating
  **across** multiple protocols?†
- Smart contract risk: This includes auditing, Lindy, etc. 
- Centralization risk: Who can participate as a staker? Who can participate as a Node Operator? Who
  _does_ participate as an NO/staker? Are there entities or groups with extra powers? What are the
  bounds on those powers, how big are the groups, what are the incentives?
- Regulatory risk: Can regulatory action damage the protocol? What safety measures are in place to
  avoid serious damage? Consider all parties here: staff, NOs, stakers, etc.
- Incentive alignment: Who has incentives for what? Examples from RP include: the ETH bond, which,
  eg, makes it unprofitable to hold a validator hostage to slashing; the requirement to hold RPL,
  which helps align Node Operators (the voting set at this time) with the protocol as a whole.
- Soft factors: This includes things like past behavior, principles (such as this document), etc.

† Note that spanning protocols isn't just theoretical - for example, Allnodes currently operate
over 1k validators for RP and over 7k validators for Lido. They do so openly, but that need not be
the case with all future entities.

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

A couple more thoughts:
- Most of this is written in terms of limiting on the rETH minting side. It would also be possible
  to limit on the minipool creation side.
- An option that has been floated if there aren't healthy competitors and we're dominant, is that we
  could spin/fork off a competitor. This is technically quite possible. That said, it would be
  _extremely_ challenging to cut ties fully enough to make this healthy. Be careful if considering
  this path!
- Beware of security theater or governance theater -- just because something claims to X, doesn't
  mean it achieves X!


## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).