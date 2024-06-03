---
layout: other-content
title: Supporting components of the Tokenomics Rework
nav-title: Part 4 - Supporting components of the Tokenomics Rework
description: Describes the contents and benefits of the tokenomics rework at a high level in comparison to the original tokenomics. 
author: Valdorff (@Valdorff), LongForWisdom, Samus (@orangesamus), Paladin
custom-next: none
custom-prev: /tokenomics-explainers/004-rework-support
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

Glossary

**Bonded ETH:** The portion of a validator’s ETH that is provided by the Node Operator. Sometimes called NO ETH or nETH.

**Borrowed ETH:** The portion of a validator’s ETH that is provided by the protocol (ultimately from rETH holders). Sometimes called protocol ETH or pETH.

**Cliff:** Description of RPL rewards going suddenly to 0 (as opposed to gradually declining) when you fall below 10% borrowed ETH worth of staked RPL. Aka RPL reward threshold.

**Forced Validator Exit:** The ability for the protocol to exit a validator (eg, for bad behavior, such as MEV theft). Currently only the node operator can trigger an exit, or the Ethereum protocol itself, which it does when the validator balance reaches 16 ETH.

**LEB, or x-ETH Bond:** Originally minipools used a 16 ETH Bond. As a result, smaller bonds were referred to as  “Lower ETH Bond” minipools, or LEBs. The Atlas release introduced LEB8s. The new proposal suggests 4-ETH bond validators and 1.5-ETH bond validators.

**Megapools:** A single RP contract serving as the withdrawal address for multiple validators.

**MEV Theft:** When a node operator steals execution-layer rewards for profit. This requires a sophisticated, unethical, and lucky operator. See Node Level Penalties.

**Node Level Penalties:** The ability to penalize a Megapool in order to disincentivize MEV theft. The ability to penalize across validators means a malicious node operator can only profit when an opportunity is larger than their total bond across validators, rather than just their bond on one validator.

**NO:** Node Operator

**Top off:** To (buy and) stake more RPL to keep above the RPL reward threshold. (See Cliff).

**rETH TVL:** Total value locked in rETH. Consider it the “size” of the liquid staking token.

**Solo staking APY:** The yield for staking on a full 32-ETH validator (inclusive of consensus rewards, tips, and MEV rewards)


<div class="prev-next-container">
{%if cPrev %}<a href="{{cPrev.url|relative_url}}">Previous - {{cPrev.title}}</a>{%else%}<span>Previous</span>{% endif %}
{%if cNext %}<a href="{{cNext.url|relative_url}}">Next - {{cNext.title}}</a>{%else%}<span>Next</span>{% endif %}
</div>