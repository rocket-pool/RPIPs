---
rpip: 35
title: Time-based Balance and RPL Price Submissions 
description: Changes Balance and RPL Price submissions to be time-based 
author: Darren Langley (@langers), Kane Wallmann (@kanewallmann), Joe Clapis (@jcrtp)
discussions-to: https://dao.rocketpool.net/t/rpip-35-time-based-balance-and-rpl-price-submissions/2419
status: Final
type: Protocol
category: Core
created: 2023-11-10
vote-to: https://vote.rocketpool.net/#/proposal/0x09a416966b94c739a46a6c886399acb61fe3378429a80193e75dd219fe2d6c2c
vote-date: 2023-12-20
vote-result: Passed
---

## Abstract
A proposal to change the Balance (used in rETH exchange calculation) and RPL Price submissions to be time-based, rather than block-based. 

## Rationale
Nodes that have under 10% collateral in terms of RPL staked are not eligible for rewards. This value, called their "effective stake", is calculated at the time of the interval's end and is a function of the ETH:RPL ratio, which is reported by the Oracle DAO. This means the last price update prior to the end of the interval determines whether or not a node is above 10% collateral and eligible for rewards at that interval. If a node is *not* eligible, the owner has until the end of the interval to "top up" by staking enough RPL to bring the node back above 10%. The issue here is that this "top up window" (the time between the final RPL report and the end of the interval) is somewhat uncertain and fluctuates from interval to interval. One of the more common pain points we see in the community is from people who are close to the 10% mark (or under it) and ask what the window is for the current interval; unfortunately, it's not easy to give them a direct answer due to the way the price and balance reporting periods are currently calculated.

The problem stems from the fact that price and balance intervals are currently specified in a number of **blocks**, where it is assumed that each block will take 15 seconds on average (and thus the interval should reflect approximately 24 hours on mainnet). This was valid prior to the Merge, but no longers works properly after the Merge. As of today, blocks are instead added to the chain at fixed 12 second intervals (corresponding to slots on the Beacon chain). One natural assumption might be to simply adjust the reporting period to match this new timing, but it still leaves an element of randomness because blocks corresponding to missed slots are skipped (thus, the time between two sequential Execution layer blocks could be 12 seconds, or 24, or 36, or so on).

This change removes the uncertainty associated with this timing by shifting price and balance intervals from a number based on blocks to a number based on seconds, which has parity with the way rewards intervals are calculated today. Essentially it means prices and balances will be reported at the same time each day (assuming the interval is set to `86400`, the number of seconds in 24 hours) without exception. It standardizes the timing of both functions to make them easier to predict.

Furthermore, the coupling of the rewards period to this interval means that the "top up window" will now be a fixed quantity directly controllable by the protocol; it will start at the beginning of the final balances reporting interval of the rewards period, and end with the rewards period end. Thus, if the balances period is set to 1 day, users will consistently have exactly 24 hours to top up from the final price update of the interval. The start and end times can be reliably provided so the guess-work is removed.

*(As a side-node, this also makes the implementation of the watchtower easier and more forgiving (computation-wise) on the Execution Client; balances are calculated by "approximating" the rewards distribution of the Smoothing Pool at each balances report, and this change means balance reporting can use the same network snapshot as rewards calculation so the watchtower won't need to generate the state twice for the same slot.)*

## Specification

- As the oDAO watchtower, I MUST be able to read the next time-based balance submission interval from the Rocket Pool protocol
- *Balance submission default interval time will be `86400 secs` (225 epochs)*
- As the oDAO watchtower, I MUST be able to read the next time-based RPL price submission interval from the Rocket Pool protocol  
- *Price submission default interval time will be set to `86400 secs` (225 epochs)*

## Backwards Compatibility
There is a slim chance that DeFi protocols that rely on the rETH exchange rate calculation may be affected, as the timing will change. That said, it is unlikely that DeFi protocols are reliant on the specific timing. Going forward, it makes DeFi integrations more intuitive because the rETH exchange rate changes more consistently.

## Security Considerations
A full audit process will cover any security considerations but there are no known considerations.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).