---
rpip: 7
title: Withdrawal Address Escrow
description: A smart contract to govern Rocket Pool rewards and distribute RPL and ETH to separate parties.
author: BrassLion (@BrassLion)
discussions-to: https://dao.rocketpool.net/t/withdrawal-address-splitter-contract/563
status: Draft
type: Protocol
category: RPRC
created: 2022-16-04
---

## Abstract
There is a demand for parties to stake ETH with Rocket Pool without it necessary to take a position in RPL. There is also a demand from parties uninterested with running a Rocket Pool node to be able to loan unstaked RPL to other parties and earn yield on their loaned RPL.

This RPIP proposes an escrow smart contract, which will accept all ETH and RPL rewards earned by a node, and then allow the node operator to withdraw only the earned ETH rewards and allow the RPL loaner to withdraw only the earned RPL rewards.

## Motivation
A solution for this idea is already being explored in [RPIP-3](RPIP-3.md), which proposes a trustless approach to partner parties interested in staking ETH only with RPL holders. This would be an ideal long-term solution to the problem, but requires numerous changes to the smartnode codebase and possibly protocol contract upgrades, all of which will delay the ability for more ETH to be staked into Rocket Pool.

In some cases, we can take a less trustless approach where all parties agree to share the node wallet keys. RPL and ETH staking may then be done independently by each party and withdrawals of their respective rewards can be controlled trustlessly by an escrow smart contract.

This approach, while requiring more trust than [RPIP-3](RPIP-3.md), is significantly simpler to achieve and can be written independently to the current Rocket Pool protocol and smartnode, allowing more ETH to be staked into Rocket Pool before it is in place. 

## Specification
A new RocketPoolWithdrawalEscrow smart contract should be created that:

1) Must receive and hold RPL and ETH balances.
2) Must distribute ETH balance only to addresses whitelisted under a "Staker" role.
3) Must distribute RPL balance only to addresses whitelisted under a "Loaner" role. 
4) New addresses must be added to the Staker role only by other Stakers.
5) New addresses must be added to the Loaner role only by other Loaners. 

The proposed workflow of the escrow contracts and staking procedure is as follows:

1) The ETH staker should create and register a new node.
2) The ETH staker should share the wallet private key with the RPL loaner.
3) Either party should deploy a new RocketPoolWithdrawalEscrow contract with agreed upon whitelisted addresses for each party.
4) Ether party should set the node's withdrawal address to point towards the deployed RocketPoolWithdrawalEscrow contract.
5) The RPL loaner should stake their RPL to the node using the shared private key.
6) The ETH staker should deposit their ETH to the node and create minipools.
7) RPL rewards will be sent to the RocketPoolWithdrawalEscrow contract and will be withdrawable by the RPL loaner during staking.
8) On exit, unstaked ETH will be sent to the RocketPoolWithdrawalEscrow contract and will be withdrawable by the ETH staker.

## Rationale
See Motivation for the rationale behind this design.

## Backwards Compatibility
No backwards compatibility issues exist for this contract.

## Test Cases
Tests will be implemented alongside the RocketPoolWithdrawalEscrow contract. It will also be deployed to Prater before publishing. 

## Security Considerations
- This is not a trustless design. Specifically, the ETH staker may hold the RPL loaner's tokens hostage by refusing to exit minipools and release staked RPL to the escrow contract.
- Since the Staker and Loaner roles are self-governing and have the ability to add and remove new addresses to each role, all parties within each role must maintain trust with one another.
- The RocketPoolWithdrawalEscrow should be non-upgradable, so some smart contract risk exists and contracts should be sufficiently audited before release.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
