This is a snapshot of https://github.com/Valdorff/rp-thoughts/blob/main/2024_02_strategy/readme_tier3.md for the RPIPs repository. 

----

## Table of Contents
1. [Choose your depth](#choose-your-depth)
2. [Suggested support](#suggested-support)
2. [Staked RPL... or all RPL](#staked-rpl-or-all-rpl)
3. [Other considerations](#other-considerations)

## Choose your depth
🛑🛑 Please stop for a moment 🛑🛑

This document is the third tier of importance.

If you feel comfortable with everything in the top tier, by all means feel free to continue. But if you don't, please focus on that and ask questions. 

The sections below are tiny details compared to the stuff in higher tiers. Most readers should feel free to ignore this document.

- [Top tier](readme.md)
- [Tier 2](readme_tier2.md)
- [Tier 3](readme_tier3.md)

## Suggested support
- Remove opt-in upgrades
  - They add very little security as we have withdrawals now; meanwhile, they make code maintenance and testing much harder as more states are possible.
- MEV penalties
  - Allow-listed relays
  - No vanilla blocks allowed
  - oDAO-applied penalty based on size of theft (largest bid across relays) plus a little bit (eg, 10%)
    - Note: no penalty for missing, just for using vanilla block or a non-allowed relay
  - Some trickiness around relay API timeliness - don't expect them to be quite realtime
    - If APIs are bad, we may need to remove them from allowlist
  - Security council should be able to remove penalties -- note they can be replaced by oDAO, so this is insufficient
  - pDAO guardian must remain in place to limit total penalties
- rETH protection
  - Outlier underperformers (see ArtDemocrat's proposals) should pay restitution to rETH to meet an adequate number (eg, overall rETH performance).
  - First restitution should be funded from ETH rewards flows
    - Per reward period: EL smoothie, RPL commission share -- both of these can be taken care of when making the merkl tree, so the underperformer never gains dominion over those funds
    - At this point, record any remaining unpaid restitution as debt
    - Debt can be repaid by:
      - EL fee distributor distribution -- anyone can do this
      - CL distribution -- anyone can do this
      - CL distribution during exit -- only owner can do this without a long wait.
        - Note that this exit can be a forced exit
        - Advanced forced exits: if performing badly routinely -- kick. This is better for rETH _and_ the NO.
- Set RPL inflation to 1.5% and end RPL rewards
  - In this structure, we don't need to use RPL inflation to get more people staking RPL to meet rETH demand. Insofar as folks have unstaked, they're at least doing a service to RPL stakers by giving up their share. If they're doing something like LPing, then they're further serving RP.
  - 1.5% means the oDAO and pDAO continue to receive the same amount - this just removes the portion that went back to NOs.

## Support for the future
- Add a commission share for the oDAO (set settings to zero). If we grow large enough, we can get rid of inflation and use this instead.
- Add a commission share for the pDAO (set settings to zero) If we grow large enough, we can get rid of inflation and use this instead.
- Leave support for RPL rewards (set setting to zero). Eg, if we wanted to run a short term NO onboarding blitz, it might make sense to add RPL rewards.

## Other considerations
- It would be great if ETH credit could be cashed out to rETH, perhaps limited to when the deposit pool is <50% to avoid any attempts to abuse with "premium""
- Change initial stake flow. I think it's worth the cost to go to 3 transactions when there's a queue.
  - deposit()
    - _deposit() provides 1 ETH to DP, gives you 1 ETH credit
    - If there is enough ETH in DP to launch all minipools in queue, call initial_stake() 
  - initial_stake() stakes 1 ETH on beaconchain and uses up 1 ETH of credit
  - ((security checks in this time))
  - stake() adds remaining 31 ETH
  - Benefits:
    - Can efficiently use ~all deposited ETH to validate. 1 ETH not going to DP mattes more when deposits are 1.5 ETH than it did when they were 8 ETH.
    - Can exit minipool queue seamlessly. Note this isn't really abusable -- folks lock up ETH and give away any rewards from it. If folks wanna do that repeatedly to hold spots on line, they're welcome to. (this assumes the credit withdarawal thing I mentioned above)
    - Somewhat reduced chance of dissolves. Stake always happens close to an active transaction, so people less likely to forget about it (and messaging can be clear about when something is expected to happen instead of open ended).
  - Downsides:
    - If there isn't enough ETH in DP to launch all minipools in queue, takes 3 transactions instead of 2. Gas and inconvenience.
- It's not clear to me how migrating to megapools should work.
  - Can we have a mix of minipools in a megapool and legacy minipools? I think that should be ok from a technical standpoint...
  - Having worse capital efficiency, zero RPL rewards from inflation, and being gassier should make them pretty unattractive pretty fast imo. 
  - Maybe this is simply "don't shatter if people have both" and the rest will handle itself.
- What if we change our mind about bond curve?
  - I think the curves can essentially be represented by three numbers: number of anchor minipools, anchor minipool size, additional minipool size.
  - If we change our mind, I think that'd be ok. We'd essentially want a way to designate new anchor minipools, or resize the existing ones -- the credit system might make that not-too painful?
- Explicit non-change: there should be unbonding time for staked RPL. We already have that from RPIP-30, and I just want to explicitly note it will continue to apply and apply regardless of whether someone is RPL-only or also an NO with bonded ETH.
- I advocated for a NOs receiving 5% commission as a starting point to start extremely attractive. One alternative is to start quite low (eg, 2%) and increase only if DP isn't staying empty, eg. I think it's important that this _not_ be a bottleneck to growth. If going this route, I'd want something like a multisig that can increase that commission in 1% steps up to 5% without any votes. It'd be fine if that power was later deactivated.