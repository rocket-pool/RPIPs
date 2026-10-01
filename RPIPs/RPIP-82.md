---
rpip: 82
title: Enable pDAO Spending of ETH and ERC-20
description: Introduce pDAO proposals for one-time and recurring ETH and ERC-20 payments.
author: knoshua (@knoshua)
discussions-to: https://dao.rocketpool.net/t/rpip-82-enable-pdao-spending-of-eth-and-erc-20/3948
status: Review
type: Protocol
category (*only required for Protocol ): Core
created: 2026-05-21
---
## Abstract

RPIP-72 introduced `pdao_share`, which lets the protocol direct part of ETH staking rewards from megapools to the pDAO. This proposal enables the pDAO to spend this ETH by voting on one-time spends and recurring payments in ETH. It also generalizes these mechanisms to payments for any ERC-20.

## Specification

This RPIP extends the functionality of two proposal types of the on-chain pDAO voting system introduced in RPIP-33.

### One-Time Spend

This proposal type deals with one-time spends of ETH or ERC-20 tokens from the protocol’s treasury.

A proposal of this type comprises the following:

1. **Invoice ID:** An arbitrary string associated with the payment used for bookkeeping purposes.
2. **Recipient Address:** The Ethereum address of the beneficiary of this payment.
3. **Type:** Whether this is an ETH spend or a token spend.
4. **Token Address (if type is ERC-20):** In case of a token spend, the token address.
5. **Amount:** An amount of ETH or token to send to the beneficiary.

Upon successful execution of this proposal type, the protocol SHALL send the specified amount of ETH or token from its treasury to the recipient.

### Recurring Payments 

This proposal type creates and modifies recurring ETH or ERC-20 token payment contracts.

A payment contract comprises:

1. **Contract Name:** An arbitrary string associated with this contract used for bookkeeping purposes.
2. **Recipient Address:** The Ethereum address of the beneficiary of this payment.
3. **Period Length:** The length (in seconds) of periods of this contract.
4. **Start Date:** A unix timestamp specifying when payments should begin.
5. **Number of Periods:** The number of periods the protocol will pay the beneficiary.
6. **Type:** Whether the recurring payment is in ETH or an ERC-20 token.
7. **Token Address (if type is ERC-20):** In case of a token spend, the token address.
8. **Amount Per Period:** The amount of ETH or token to send per period.

A proposal of this type can perform one of these actions:

1. **Create:** Creates a new recurring payment contract.
2. **Update:** Modifies an existing recurring payment contract.

The protocol SHALL allow the recipient address of a recurring payment to execute a claim at any time. The protocol SHALL calculate all unpaid payments up to that point and transfer the required amount from its treasury to the recipient.

## Rationale

This proposal aims to enable ETH spending with as little complexity as possible by mirroring the existing implementation of spending RPL from the treasury. Generalizing this mechanism to ERC-20 tokens adds no complexity and is included for future-proofing.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
