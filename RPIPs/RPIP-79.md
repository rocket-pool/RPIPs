---
rpip: 79
title: Faster Withdrawal Credentials Check
description: Instead of requiring state proofs for every validator, use a fraud-proof system to protect against malicious withdrawal credentials.
author: knoshua (@knoshua)
discussions-to: https://dao.rocketpool.net/t/rpip-79-faster-withdrawal-credentials-check/3943
status: Draft
type: Protocol
category (*only required for Protocol ): Core
created: 2026-04-01
---
## Abstract
This RPIP proposes to extend the beacon state proof mechanism that protects megapool validator creation from withdrawal-credential front-running with a fraud-proof system using beacon state. This will reduce time in queue for new megapool validators and instead of requiring a proof for every new validator, the protocol would only require one in the event of a withdrawal-credential exploit.

## Motivation
The Saturn 1 upgrade removed the oDAO scrub check duty and replaced it with a beacon state proof about the validator. Since EIP-7251 (implemented in Electra), the deposit signature is validated and the pubkey is compared to existing validators only after a validator makes it through the `pending_deposits` queue. This means that the state proof, and therefore the second deposit (31 ETH) for a validator can only happen once the first deposit (1 ETH) has made it through. This proposal speeds up this process and removes the dependency on the entry queue. Additionally, proofs would only need to be submitted in case of an attempted withdrawal credential exploit and, in the common case of honest node operators and correct withdrawal credentials, no proof is required.

We expect [RPIP-73](RPIP-73.md) to lead to significant validator churn, which makes minimizing the time new validators spend in the beacon queue a priority.

## Specification
This specification introduces the following pDAO protocol parameters:

| Name                        | Type  | Initial Value | Guardrail |
| --------------------------- | ----- | ------------- | --------- |
| `prestake_challenge_period` | Hours | `12`          | `≥12`     |

This specification changes the following pDAO protocol parameter guardrails:

| Name           | Type | Current Guardrail | New Guardrail   |
| -------------- | ---- | ----------------- | --------------- |
| `reduced_bond` | ETH  | >= 1 and <= 4     | >= 1.1 and <= 4 |

- There SHALL be a method `stake_without_state_proof` that performs the remaining 31 ETH stake without requiring a validator state proof.
- After `prestake` and before `stake` or `stake_without_state_proof` has been called for a megapool validator, the protocol SHALL allow anyone to provide either:
	1. A beacon state proof showing that `validators` contains a validator with a matching `pubkey` and incorrect `withdrawal_credentials`, or
	2. A beacon state proof showing that `pending_deposits` contains a validator with a matching `pubkey`, incorrect `withdrawal_credentials`, and a valid `signature`.
- If one such proof is submitted, the rETH share of the remaining 31 ETH SHALL be returned to the deposit pool. The remaining node operator share SHALL be awarded to the provider of the proof directly, or it MAY be awarded in the form of credit while the full 31 ETH are sent to the deposit pool.
- If no proof is provided within `prestake_challenge_period`, the protocol SHALL allow anyone to call `stake_without_state_proof`, which SHALL stake the remaining 31 ETH for the validator.
## Rationale

The BLS12-381 signature of the validator that is part of a deposit is not verified by the deposit contract, and it is also not verified before a deposit is added to `pending_deposits`. As a result, anyone could “fake front-run” a legitimate validator with a deposit of 1 ETH using the same pubkey together with a fake signature. Such a deposit would be part of the `pending_deposits`, so the signature has to be validated in that case to avoid a griefing vector where someone could spend 1 ETH to have legitimate Rocket Pool validators dissolved. [EIP-2537](https://eips.ethereum.org/EIPS/eip-2537) introduced precompiles for BLS12-381 curve operations that allow signature verification.

The lower guardrail for `reduced_bond` is raised to 1.1 ETH to ensure at least a 0.1 ETH reward for a challenger.

With the proposed specification, a deposit with a valid signature and incorrect withdrawal credentials that happens after a correct first deposit can be dissolved. While this scenario wouldn't actually result in incorrect withdrawal credentials being set for the validator, there is no legitimate reason for such deposits to exist. Dissolving them is a conscious choice to not leave any room for unforeseen exploits.

The existing beacon state proof system remains in place and allows short-circuiting the challenge period in cases where there is no beacon chain queue.

We considered using the Beacon Chain deposit contract in a fraud-proof system instead of beacon state proofs. While generating proofs against the deposit contract may be more stable under future Ethereum hard forks, generating those fraud proofs would require storing every deposit ever made.

## Security Considerations
A fraud proof only succeeds if another deposit with the same pubkey and a valid signature exists. Compared to the legacy oDAO scrub system and the beacon state proof system, the threat model is therefore essentially unchanged: an adversary who cannot sign as the validator cannot cause dissolution. The main caveat introduced by this proposal is that a deposit with valid signature and incorrect withdrawal credentials that happens after a correct first deposit also leads to dissolution.

Incorrect implementation of the fraud-proof verifier could allow either false positives (dissolving honest validators) or false negatives (failing to detect prior valid deposits).

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
