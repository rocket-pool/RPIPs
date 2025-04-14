---
rpip: 
Title: Ronin Bridge Info
description: Describing Ronin
author: Valdorff (@Valdorff)
discussions-to:
status: Draft
type: Info
created: 2024-04-14
requires:
vote-to:
vote-date:
vote-result:
---

## Abstract
This document is meant to record how the Ronin bridge is being deployed.

## Context
1. This deployment is, insofar as possible, being treated like current bridged token deployments on L2s
2. Ronin has chosen to use Chainlink's CCIP (cross-chain interoperability protocol) product as their canonical bridge; we will treat this similarly to native bridges on L2s

## Information
1. There are 3 smart contracts involved in a CCIP bridged token
   1. BurnMintERC20 is the representation of rETH on Ronin  
   2. LockReleaseTokenPool on mainnet holds crosschain bridged rETH
   3. BurnMintTokenPool on Ronin burns/mints rETH bridged to Ronin
2. Ownership of the RETH token on Ronin will be burnt after assigning mint rights to the TokenPool. This is in line with the immutable tokens we've used on L2s.
3. Ownership of the LockReleaseTokenPool and  BurnMintTokenPool will remain with Chainlink. This is in line with upgradable native bridges we've used on L2s.
   1. Chainlink is willing to transfer ownership at any time at the request of the Rocket Pool pDAO (via governance vote)

Note, there is a separate RPIP related to fees when using this bridge.

## Security considerations
It's worth to note that the analogy to L2 native bridges is imperfect:
- Users should be aware that Ronin is not an L2 and will not have "escape hatches" to mainnet as a result
- Users should be aware that they are trusting two separate entities to secure their bridged tokens
  - Ronin to secure the chain
  - Chainlink to secure the bridge

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

