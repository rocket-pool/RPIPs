---
rpip: 52
title: Rewards Tree v9 Specification
description: Specifies how RPL Inflation and Smoothing Pool rewards are allocated to node operators
author: Jacob Shufro (@jshufro), Ramana Kumar (@xrchz), Joe Clapis (@jcrtp)
discussions-to: https://dao.rocketpool.net/t/rewards-tree-spec-v9/2928
status: Final
type: Protocol
created: 2024-04-15
vote-to: https://vote.rocketpool.net/#/proposal/0xf9196fa7e0e82418bbfa69cbd06f0bad796218eac4538b9153cbc78fe924994f
vote-date: 2024-05-23
vote-result: Passed
---

## Abstract
This RPIP modifies the Rewards Tree Specification described in [RPIP-51](RPIP-51.md) to add formal specifications for the format of the Rewards Tree file.
This file previously had an unspecified format- the Oracle DAO simply produced equivalent files by way of using the same implementation.

If this RPIP is approved, the Oracle DAO will instead produce files in the manner that it describes.

## Motivation
Currently, the Oracle DAO arrives at consensus by submitting both a Merkle Root and an IPFS CID for each interval.
The IPFS CID is determined by the implementation, which means the Oracle DAO must all use the same implementation.
By specifying the format of Rewards Files, we enable future efforts to diversify Oracle DAO Rewards Tree implementations.


## Specification

The Rewards Calculation Specification is unmodified from RPIP-51, as is the Merkle Tree Specification.

* [Rewards Calculation Specification](../assets/rpip-51/rewards-calculation-spec.md)
* [Merkle Tree Specification](../assets/rpip-51/merkle-tree-spec.md)

The Oracle DAO SHALL additionally implement the following specification for creating a SSZ file and computing its CID.

* [Rewards File Specification](../assets/rpip-52/rewards-file-spec.md)

The Oracle DAO SHALL use this CID in its submissions.

The Oracle DAO SHOULD produce and make available four files during a transitionary phase:
* The SSZ file
* A JSON representation of it
* A JSON Minipool Performance File
* A compressed copy of the SSZ File

The Oracle DAO SHOULD upload the SSZ File to IPFS.

The Oracle DAO SHOULD upload all four files to a publicly accessible webserver, where individual Node Operators can retrieve them.

The Oracle DAO MAY discontinue publishing the JSON Rewards File after a transitionary period to allow tooling to adopt the new format.

## Rationale
Currently, the Oracle DAO generates a _Minipool Performance File_ and a _Rewards File_ in JSON.
Because the JSON is large, the Oracle DAO also compresses these files using a Golang ZSTD library.
The library they currently use doesn't produce a stable output across versions, which means that the IPFS CID the Oracle DAO comes to consensus on is dependent on use of a specific version of a specific Golang ZSTD library.
In order to correct this, we specify a format that does not require compression.
SSZ was chosen for being compact and an Ethereum native format.

Uncompressed SSZ files are about the same size as compressed JSON files for this purpose, with the added benefits of being faster to parse and serialize.

## Reference Implementation
This specification (Rewards Calculation v9) is implemented by [Treegen](), which is the reference implementation.
It is also implemented by [Rockettree]().

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
