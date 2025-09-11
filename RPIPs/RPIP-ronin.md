---
rpip: 
Title: Ronin Bridge Fees
description: Ratifying (or rejecting) the bridge fee when bridging rETH to/from Ronin
author: SSJ2 (@ssj2_spartan)
contributor: Valdorff (@Valdorff)
discussions-to:
status: Draft
type: Meta
created: 2024-04-09
requires:
vote-to:
vote-date:
vote-result:
---

## Abstract
This is a proposal to ratify a small flat fee on rETH bridging to and from Ronin. This proposal builds on using Chainlink’s CCIP bridge, which is Ronin’s canonical bridge.

Chainlink has already been asked to implement a $2 flat fee for bridging rETH to/from Ronin. Due to time constraints, that did not go through full governance. This vote is to ratify and affirm that fee (on vote passing) OR to reject it and ask Chainlink to remove the fee (on vote failing).

## Motivation
Ronin is proposing a partnership with the Rocket Pool DAO to mint $3M in rETH and integrate it into their DeFi ecosystem, using Chainlink's CCIP as a canonical bridge from Ethereum mainnet. Facilitating this will help bring rETH demand, and had to be done expediently (which is why an initial fee for the bridge has already been established).

Given the use of CCIP, there was an opportunity for a new revenue stream for the DAO by charging a fee on rETH transactions to and from the Ronin chain.

## Specification
- We SHALL request a CCIP bridge fee of $2 for transfers exiting Ronin
- We SHALL request a CCIP bridge fee of $2 for transfers entering Ronin
- We SHALL request Chainlink collect the fees and periodically send them to the pDAO treasury wallet with gaps no longer than 5 months
  - A quick informational how-to:
    - Call `rocketVault.transferToken("rocketClaimDAO", tokenContract, amount)` where tokenContract and amount should be based on the token and amount being deposited
    - The `rocketVault` address can be found using the immutable `rocketStorage` contract at `0x1d8f8f00cfa6758d7bE78336684788Fb0ee0Fa46`. There we call `rocketStorage.getAddress(keccak("contract.addressrocketVault"))` = `rocketStorage.getAddress(0x41c30d91bfaf5fa8d610263b0554366f2159a2b6807bf2fdbeb8f2b21a62f17b)` to get the active `rocketVault` address
    - Any ERC20 token can be deposited
    - ETH cannot be deposited, but WETH can be since it's an ERC20 token

# Rationale
 At this time, only flat fees are available (though it is hoped and expected that percentage-based fees will be available at a later time). It is worth noting that a future vote can change what fees we request Chainlink impose on our behalf.

To get an idea of the revenue, https://dune.com/queries/4755318 shows what it would have looked like for Arbitrum between 2024-10-01 and 2025-02-22 (exclusive). We see that would have generated $316 of revenue, which annualizes to ~$807. Ronin is not Arbitrum, so this should be taken only as a rough context.

While the revenue expected from these fees is modest, several community members believe it’s important that the pDAO start experimenting with and getting comfortable with alternative revenue sources. To that end, this is a great low-risk entry point for the pDAO.

## Security Considerations
The fees don’t add security considerations to principal balances, or bridging.

That said, the fees themselves have some security considerations. It is technically possible for Chainlink to (a) set fees differently from the RP pDAO’s request, and (b) not send collected fees to the pDAO. Since no party retains control of the rETH contract, it would be very difficult or impossible to technically force Chainlink to act as we wish, or to switch to a different bridging provider (though social pressure may be a powerful tool in such a scenario). This situation is seen as quite unlikely and a reasonable amount of risk given the values involved.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

