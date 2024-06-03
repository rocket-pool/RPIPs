---
layout: other-content
title: Introduction to the Tokenomics Rework
nav-title: Part 2 - Introduction to the Tokenomics Rework
description: Describes the contents and benefits of the tokenomics rework at a high level in comparison to the original tokenomics. 
author: Valdorff (@Valdorff), LongForWisdom, Samus (@orangesamus), Paladin
custom-next: /tokenomics-explainers/003-rework-foundation
custom-prev: /tokenomics-explainers/001-why-rework
---

{% assign cPrev=site.pages | where:"url", page.custom-prev | first %}
{% assign cNext=site.pages | where:"url", page.custom-next | first %}

<div class="prev-next-container">
{%if cPrev %}<a href="{{cPrev.url|relative_url}}">Previous - {{cPrev.title}}</a>{%else%}<span>Previous</span>{% endif %}
{%if cNext %}<a href="{{cNext.url|relative_url}}">Next - {{cNext.title}}</a>{%else%}<span>Next</span>{% endif %}
</div>

## Choose your depth

- Intro: [Why Rework Rocket Pool's Tokenomics?](./001-why-rework.md)
- Intro: [Introduction to the Tokenomics Rework](./002-rework-intro.md)
- Core: [Foundation of the Tokenomics Rework](./003-rework-foundation.md)
- Intermediate: [Supporting components of the Tokenomics Rework](./004-rework-support.md)
- Advanced: [RPIP-49](../RPIPs/RPIP-49.md) tokenomics spec overview and linked RPIPs

## Items that directly address tokenomics issues

With the rework, speculation and node operation are separated. It becomes **possible to run a Rocket Pool node with only an ETH bond.**

For the variants of the rework that introduce RPL buy and burn or RPL buy and LP, it **may be sensible to hold RPL as a standalone investment** at protocol maturity.

**Staking RPL alongside ETH bonds still has value** with the rework. 
- This is the only group that has pDAO vote weight.
- This group receives a share of protocol revenue proportional to their vote-eligible RPL.

With the rework, **we do not impose a value onto the market**. Node operators can freely value providing an RPL stake alongside their ETH bond. If the revenues make the speculation worthwhile, node operators can speculate but are not forced to do so.

There is **no RPL reward threshold/cliff** with the rework, so we avoid losing rETH supply from node operators exiting minipools to maintain the staking threshold.

## Items addressing other issues

With the rework, smaller ETH bonds will be possible. This **dramatically increases capital efficiency.** Arguably, this is the most important single item in the rework; while other changes impact how revenue is distributed, this change increases how much revenue each unit of capital can generate.

Consider ETH-only validators receiving a 3.5% commission on borrowed ETH with various bonds[^1]:
* 8 ETH bond: **10.5% boost** compared to solo staking rewards.
* 4 ETH bond: **24.5% boost** compared to solo staking rewards.
* 1.5 ETH bond: **71.2% boost** compared to solo staking rewards.

Node-level penalties and forced validator exits are also included in the rework and are key to making smaller ETH bonds safe enough to use.

Megapools will enable multiple validators to share the same Ethereum withdrawal address – this **reduces initial and ongoing gas costs** for node operators.

**RPL inflation will be reduced from 5% to 1.5%**, because the current 3.5% inflation used for RPL rewards to NOs is no longer necessary.

Forced delegate upgrades (with a grace period) will limit the technical debt the Rocket Pool protocol accumulates from previous versions.

<div class="prev-next-container">
{%if cPrev %}<a href="{{cPrev.url|relative_url}}">Previous - {{cPrev.title}}</a>{%else%}<span>Previous</span>{% endif %}
{%if cNext %}<a href="{{cNext.url|relative_url}}">Next - {{cNext.title}}</a>{%else%}<span>Next</span>{% endif %}
</div>

---

[^1]: ROI is calculated using the following formula `(bonded_eth + commission_pct * borrowed_eth) / bonded_eth` 