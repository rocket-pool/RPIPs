---
rpip: 
Title: Ronin Bridge Fees
description: Set the bridge fee when bridging rETH to/from Ronin using CCIP
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
A proposal to charge a small flat fee on rETH bridging to and from Ronin. This proposal builds on using Chainlink’s CCIP bridge, which is Ronin’s canonical bridge.

## Motivation
Ronin is proposing a partnership with the Rocket Pool DAO to mint $3M in rETH and integrate it into their DeFi ecosystem, using Chainlink's CCIP as a canonical bridge from Ethereum mainnet. This partnership could create a new revenue stream for the DAO by charging a small flat fee on rETH transactions to and from the Ronin chain. This is to help bring more demand for rETH minting and provide a new revenue stream for the DAO.

## Specification
- We SHALL request a CCIP bridge fee of $5 for transfer exiting Ronin
- We SHALL request a CCIP bridge fee of $1 for transfer entering Ronin
- We SHALL request Chainlink collect the fees and periodically send them to the pDAO treasury wallet with gaps no longer than 5 months
  - A quick informational how-to:
    - The current method is to call `rocketVault.transferToken("rocketClaimDAO", tokenContract, amount)` where tokenContract and amount should be based on the token and amount being deposited
    - The `rocketVault` address can be found using the immutable `rocketStorage` contract at `0x1d8f8f00cfa6758d7bE78336684788Fb0ee0Fa46`. There we call `rocketStorage.getAddress(keccak("contract.addressrocketVault"))` = `rocketStorage.getAddress(0x41c30d91bfaf5fa8d610263b0554366f2159a2b6807bf2fdbeb8f2b21a62f17b)` to get the active `rocketVault` address
    - Any ERC20 token can be deposited
    - ETH cannot be deposited, but WETH can be since it's an ERC20 token

## Rationale
While the revenue expected from these fees is modest, a number of community members believe it’s important that the pDAO start experimenting with and getting comfortable with alternative revenue sources. To that end, this is a great low-risk entry point for the pDAO.

To get an idea of the revenue, https://dune.com/queries/4755318 shows what it would have looked like for Arbitrum between 2024-10-01 and 2025-02-22 (exclusive). We see that would have generated $422 of revenue, which annualizes to ~$2018.

## Security Considerations
The fees don’t add security considerations to principal balances, or bridging.

That said, the fees themselves have some security considerations. It is technically possible for Chainlink to (a) set fees differently from the RP pDAO’s request, and (b) not send fees to the pDAO. Since no party retains control of the rETH contract, it would be very difficult or impossible to technically force Chainlink to act as we wish, or to switch to a different bridging provider (though social pressure may be a powerful tool in such a scenario). This situation is seen as quite unlikely and a reasonable amount of risk given the values involved.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

