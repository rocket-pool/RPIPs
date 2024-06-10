---
layout: other-content
title: Introduction to the Tokenomics Rework
nav-title: Part 2 - Introduction to the Tokenomics Rework
description: Describes the contents and benefits of the tokenomics rework at a high level in comparison to the original tokenomics. 
author: Valdorff (@Valdorff), LongForWisdom (@LongForWisdom), Paladin (@Paladin147)
custom-next: /tokenomics-explainers/003-rework-foundation
custom-prev: /tokenomics-explainers/001-why-rework
depth: Intro
---

{% assign cPrev=site.pages | where:"url", page.custom-prev | first %}
{% assign cNext=site.pages | where:"url", page.custom-next | first %}

<div class="prev-next-container">
{%if cPrev %}<a href="{{cPrev.url|relative_url}}">Previous - {{cPrev.title}}</a>{%else%}<span>Previous</span>{% endif %}
{%if cNext %}<a href="{{cNext.url|relative_url}}">Next - {{cNext.title}}</a>{%else%}<span>Next</span>{% endif %}
</div>

## Items that directly address tokenomics issues

With the rework, speculation and node operation are separated. It becomes **possible to run a Rocket Pool node with only an ETH bond.**

For the variants of the rework that introduce RPL buy and burn or RPL buy and LP, it **may be sensible to hold RPL as a standalone investment** at protocol maturity.


**Staking RPL alongside ETH bonds still has value** with the rework. 
- This is the only group that has pDAO vote weight.
- This group receives a share of protocol revenue proportional to their vote-eligible RPL.
- Vote-eligible RPL maxes out at 150% of bonded ETH.

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

**RPL issuance will be reduced from 5% to 1.5%**, because the current 3.5% inflation used for RPL rewards to NOs is no longer necessary.

Forced delegate upgrades (with a grace period) will limit the technical debt the Rocket Pool protocol accumulates from previous versions.

## Personas

Introduction to the changes through the lens of each persona:

|Persona|Before|After|
|-|-|-|
|**rETH holder**|My staking rewards were a function of Ethereum issuance and the 14% commission paid to NOs.|My staking rewards are a function of Ethereum issuance and the total commisision determined by pDAO votes on UARS levels, meaning they may go up or down.|
|**Speculator / Passive RPL Holder**|Holding RPL was speculation on the liquid staking narrative (unchanged) and on the utility of RPL as a bond (which assumed TVL growth that has now stalled).|In addition to the narrative, holding RPL is speculation that the tokenomics changes will cause TVL to grow and generate "surplus" ETH returns, which will be used to accrue value to RPL (via buy+burn or buy+LP).|
|**ETH-only Node Operator**|N/A|I don't require RPL to create minipools and have no exposure to it. I receive a 3.5% node operator commission on borrowed ETH. The lower my bond, the higher my rewards relative to solo staking. |
|**Node Operator who stakes RPL**|I needed RPL to create minipools. I had to remain collateralized at 10% of RPL/ETH to receive RPL rewards. My comission was always 14% on borrowed ETH.|I do not need RPL to create minipools. I will a) receive node operator commission paid in ETH (3.5%). If I *do* stake RPL I will b) receive voter share rewards (5.0%) paid in ETH (with no 10% minimum or cliff) and surplus share rewards (5.5%) paid in ETH. I (and other voters) can vote to change these values as part of UARS, meaning my returns in each category may go up or down. As a NO, holding RPL is now speculation that protocol growth, including from ETH-only NOs, will generate sufficient ETH commission to accrue value to RPL via voter share and surplus share.|

<div class="prev-next-container">
{%if cPrev %}<a href="{{cPrev.url|relative_url}}">Previous - {{cPrev.title}}</a>{%else%}<span>Previous</span>{% endif %}
{%if cNext %}<a href="{{cNext.url|relative_url}}">Next - {{cNext.title}}</a>{%else%}<span>Next</span>{% endif %}
</div>

---

[^1]: ROI is calculated using the following formula `(bonded_eth + commission_pct * borrowed_eth) / bonded_eth` 