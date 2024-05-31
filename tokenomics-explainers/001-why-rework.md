---
layout: other-content
title: Why Rework Rocket Pool's Tokenomics?
nav-title: Part 1 - Why Rework Rocket Pool's Tokenomics?
description: Describes the intentions of the original Rocket Pool tokenomics and their observed weaknesses since the protocol launched.
author: Valdorff (@Valdorff), Samus (@Samus), Paladin
custom-next: /tokenomics-explainers/002-rework-intro
custom-prev: none
---

{% assign cPrev=site.pages | where:"url", page.custom-prev | first %}
{% assign cNext=site.pages | where:"url", page.custom-next | first %}

<div class="prev-next-container">
{%if cPrev %}<a href="{{cPrev.url}}">Previous - {{cPrev.title}}</a>{%else%}<span>Previous</span>{% endif %}
{%if cNext %}<a href="{{cNext.url}}">Next - {{cNext.title}}</a>{%else%}<span>Next</span>{% endif %}
</div>

## Introduction

The current bonding style tokenomics can be traced back to the original Rocket Pool whitepaper and ICO in 2017 where RPL was suggested as a requirement for future node operators. Many stakers flocked to this idea, especially after the release of the Rocket Pool Investment Thesis. The vision was realized by the Fire Eyes team in the much-hyped at the time [Article 3](https://medium.com/rocket-pool/rocket-pool-staking-protocol-part-3-3029afb57d4c). Some modifications would follow, but the overall value capture mechanism of the RPL token has remained unchanged since 2017. 

However, what many thought was a good idea in 2017 has not turned out exactly as anticipated as protocol size has stalled and even reversed. This rework aims to establish a sustainable future direction for the RPL token, stem the outflow of node operators, and provide a path to reach the protocol’s rETH supply targets.

## Current idealized concept
In broad strokes, the current setup is meant to be something like:  
  
**Ideal 1** - RPL stake serves as an entry ticket to ETH commission via the minimum requirement to start a minipool (10% of borrowed ETH)  
**Ideal 2** - RPL rewards are attractive enough to make everyone want to top off, so we expect folks stay at or above the minimum  
**Ideal 3** - In the long run, the fundamental value of RPL trends to what’s needed to get all minipools collateralized to the minimum (ie, RPL mcap should be ~10% of rETH TVL)  

## Issues seen in reality
In reality, we have learned the hard way that these don’t actually play out.
1. Despite **Ideal 1**, RPL is primarily speculative. Running minipools is also primarily speculative.
    - Financial success in running minipools is overwhelmingly dominated by RPL performance
    - Example ETH ROI assuming a minimum-RPL LEB8, 3.8% solo APY, and net RPL performance being flat vs ETH: 9% boost from solo[^1]
    - Same with -5% net RPL performance: -21% boost from solo
    - Same with +5% net RPL performance: 40% boost from solo
2. Unlike **Ideal 2**, not everyone tops off
    - As shown in Figure 1 below, many people simply opt out of topping off
    - The metric used for the RPL reward threshold is "RPL staked as a percentage of borrowed ETH", which means there are two ways to requalify for rewards. Some node operators have opted to exit minipools to lower their borrowed ETH, rather than topping off with more RPL.


<img src="../assets/tokenomics-explainers/001-figure-1.png" alt="Figure 1" width="1000px"></br>
_Figure 1 - Rocket Pool Minipool Collateralization - [Source](https://harpocryptes.github.io/)_


3. **Ideal 3** fails repeatedly:
    - If any large node (or many small nodes) decides to stop topping up their RPL, the fundamental value calculation breaks". Eg, if at maturity the average collateral is 5% of borrowed ETH, then RPL’s value would be half the value of a situation that’s identical except the average collateral is 10% of borrowed ETH. As shown in the plot above, this effect is definitely present.
    - rETH TVL is twice limited
      - Folks that exit some of their minipools to get back over the RPL reward threshold damage our ability to supply rETH and grow its TVL
      - Folks that would like to operate for RP but are unwilling to speculate on RPL are simply unable to participate because running minipools is currently quite speculative.
      - Note that speculation is exacerbated at smaller bond sizes. A 10% borrowed-ETH requirement forces greater RPL exposure as ETH bond size is reduced:
        - 16 ETH bond + 1.6 ETH worth of RPL (~9% RPL)
        - 8 ETH bond + 2.4 ETH worth of RPL (~23% RPL)
        - 4 ETH bond + 2.8 ETH worth of RPL (~41% RPL)
        - 1.5 ETH bond + 3.05 ETH worth of RPL (~67% RPL)
4. The system is brittle. The 10% borrowed-ETH minimum is something the protocol tries to impose upon the market, but fails to make economic sense at maturity.
    - Consider a mature case with 3% solo APY and 1.5% RPL value loss per year due to supply inflation: ```((8+.14*24)*.03-2.4*.015)/(8+2.4)=2.93%``` (-2.3% boost from solo). In such a case, there is no reason to make minipools if you can afford solo validators.
      - This does present a solution as well – the minimum staked RPL can be reduced. Note that this would reduce the RPL to rETH TVL relationship per **Ideal 3**, which means that the cost to avoiding this brittleness is letting go of a relationship that can be known ahead of time.
    - Note that the market has no way to say that they’re interested with 8% borrowed ETH, but not 10% borrowed ETH. All we’d see is that the market simply stops making minipools (or even starts exiting them) when the overall package is seen as unattractive (including the perceived risk of holding RPL).
      - We might be seeing this happen today, as shown by the supply of node operator ETH in Figure 2.

<img src="../assets/tokenomics-explainers/001-figure-2.png" alt="Figure 2" width="1000px"></br>
_Figure 2 - Supply of Node Operator ETH -  [Source](https://dune.com/invis/rp-neth)_

<div class="prev-next-container">
{%if cPrev %}<a href="{{cPrev.url}}">Previous - {{cPrev.title}}</a>{%else%}<span>Previous</span>{% endif %}
{%if cNext %}<a href="{{cNext.url}}">Next - {{cNext.title}}</a>{%else%}<span>Next</span>{% endif %}
</div>

---

[^1]: ROI is calculated using `((bonded_eth + commision_pct*borrowed_eth)*solo_apy + staked_rpl*net_rpl_performance)/(bonded_eth + staked_rpl)`, where `net_rpl_performance = rpl_reward_apy + rpl_ratio_change`