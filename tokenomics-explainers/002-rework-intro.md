---
layout: other-content
title: Introduction to the Tokenomics Rework
nav-title: Part 2 - Introduction to the Tokenomics Rework
description: Describes the contents and benefits of the tokenomics rework at a high level in comparison to the original tokenomics. 
author: Valdorff (@Valdorff), Samus (@orangesamus), Paladin
custom-next: none
custom-prev: /tokenomics-explainers/001-why-rework
---

{% assign cPrev=site.pages | where:"url", page.custom-prev | first %}
{% assign cNext=site.pages | where:"url", page.custom-next | first %}

<div class="prev-next-container">
{%if cPrev %}<a href="{{cPrev.url}}">Previous - {{cPrev.title}}</a>{%else%}<span>Previous</span>{% endif %}
{%if cNext %}<a href="{{cNext.url}}">Next - {{cNext.title}}</a>{%else%}<span>Next</span>{% endif %}
</div>

## Items directly addressing tokenomics issues

(See [Issues seen in reality](/tokenomics-explainers/001-why-rework#issues-seen-in-reality))

* Speculation and node operation are separated
    * It’s possible to run a node with **only an ETH bond**
    * For some variants (burn, LP), it **may be sensible to hold RPL as a standalone investment** at maturity
    * Note that there are still benefits to staking RPL alongside ETH bonds: this group can vote, and a voter share of revenue is accessible to this group.
* **We do not impose a value onto the market** – the market can freely value node operation and RPL’s revenues as they wish; this is realized using UARS (Universal Adjustable Revenue Split)
* **No RPL reward threshold/cliff**, so we avoid value destruction from folks that exit to achieve it


## Items addressing other issues
* Smaller bonds **dramatically increase capital efficiency**
    * This is arguably the most important single item
    * Most changes impact how revenue is distributed; this change is special because it changes how much revenue each unit of capital is able to generate (aka, capital efficiency)
    * Consider validators receiving 3.5% commission on borrowed ETH with various bonds[^1]:
        * 8 ETH bond: 10.5% boost from solo
        * 4 ETH bond: 24.5% boost from solo
        * 1.5 ETH bond: 71.2% boost from solo
* Megapools enable multiple validators to share the same Ethereum withdrawal address – this **reduces gas costs** both to add validators and to distribute consensus layer rewards
* RPL inflation is reduced from 5% to 1.5%, because 3.5% inflation to NOs is no longer necessary
* Node-level penalties help make smaller ETH bonds safe enough to use
* Forced validator exits help make smaller ETH bonds safe enough to use
* Forced delegate upgrades (with a grace period) limit how much technical debt RP accumulates

<div class="prev-next-container">
{%if cPrev %}<a href="{{cPrev.url}}">Previous - {{cPrev.title}}</a>{%else%}<span>Previous</span>{% endif %}
{%if cNext %}<a href="{{cNext.url}}">Next - {{cNext.title}}</a>{%else%}<span>Next</span>{% endif %}
</div>

---

[^1]: ROI is calculated using the following formula `(bonded_eth + commission_pct*borrowed_eth)/bonded_eth` 