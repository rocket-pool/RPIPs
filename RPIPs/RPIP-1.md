---
rpip: 1
title: RPIP Purpose and Guidelines
status: Living
type: Meta
author: Mike Leach (@Wander) <mike@bamboofin.tech>
discussions-to: https://dao.rocketpool.net/t/rpip-1-formalizing-protocol-changes/367
created: 2022-03-17
---

## Summary: What is an RPIP?

RPIP stands for Rocket Pool Improvement Proposal. An RPIP is a design document providing information to the Rocket Pool community or describing a new feature for Rocket Pool or its processes or environment. The precise format for RPIPs is detailed below, and [a template is located here](https://github.com/VVander/RPIPs/blob/main/RPIPs/RPIP-template.md).

Only RPIPs in the "Finalized" state are eligible for adoption and must be reviewed by editors before marked as "Finalized". If you want to contribute to an existing "Draft" RPIP, coordinate with the original author(s) to submit a PR. If you want to work on a new RPIP, contact any of the following editors:

Mike Leach (@VVander) <mike@bamboofin.tech>

**The RPIP author is responsible for building consensus within the community and documenting dissenting opinions.**

---

# RPIP Details

## RPIP Rationale

We intend RPIPs to be the primary mechanisms for proposing new features, for collecting community technical input, and for documenting the design decisions of Rocket Pool. Because the RPIP are maintained as text files in a versioned repository, their revision history is the historical record of the feature proposal.

## RPIP Types

There are three types of RPIP:

- A **Protocol RPIP** describes any change that affects the core Rocket Pool protocol as is currently defined via the smart contract implementations. Protocol RPIPs consist of a design specification and an implementation. Furthermore, Protocol RPIPs can be broken down into the following categories:
    - **Core**: improvements relevant to core protocol design.
    - **RPRC**: application-level standards and conventions, including non-core contract standards such as RPRC-3.

- A **Meta RPIP** describes a process surrounding Rocket Pool or proposes a change to (or an event in) a process. They may propose an implementation, but not to Rocket Pool’s codebase. Examples include procedures, guidelines, changes to the decision-making process (i.e. governance).

- An **Informational RPIP** describes a Rocket Pool design issue, or provides general guidelines or information to the Rocket Pool community, but does not propose a new feature.

Note that there is no Type reserved for Smart Node changes. Improvements to the Smart Node are not part of the RPIP process and are best discussed via an Issue and/or PR on the rocket-pool/smartnode repository.

## RPIP Work Flow

### Shepherding an RPIP

Parties involved in the process are you, the champion or RPIP author, the RPIP editors, and the Rocket Pool developers.

Before you begin writing a formal RPIP, you should vet your idea. Ask the Rocket Pool community on the Discord server or DAO Forum first if an idea is original to avoid wasting time on something that will be rejected based on prior research.

Once the idea has been vetted, your next responsibility will be to present an RPIP to the editors, developers, and the community for feedback. You should try and gauge whether the interest in your RPIP is commensurate with both the work involved in implementing it and how many parties will be affected by it. Negative community feedback will be taken into consideration and may prevent your RPIP from moving past the Draft stage.

It is highly recommended that a single RPIP contain a single key proposal or new idea. The more focused the RPIP, the more successful it tends to be.

### RPIP Process

The following is the standardization process for all RPIPs in all tracks:

**Draft** - The first tracked stage of an RPIP in development. A Draft RPIP is merged by an RPIP Editor into the RPIP repository once properly formatted.

**Review** - Reached once an RPIP Author marks an RPIP as ready for Review.

**Final** - A Final RPIP exists in a state of finality and should only be updated to correct errata and add non-normative clarifications.

**Stagnant** - Any RPIP in Draft or Review which is inactive for too long is moved to Stagnant. An RPIP may be resurrected by simply requesting this from an editor.

**Withdrawn** - The RPIP Author(s) have withdrawn the proposed RPIP. This state has finality and can no longer be resurrected using this RPIP number. If the idea is pursued at later date it is considered a new proposal.

**Living** - A special status for RPIP that are designed to be continually updated and not reach a state of finality. This includes most notably RPIP-1.

Only Final and Living RPIPs are eligible for official adoption.

## What belongs in a successful RPIP?

An RPIP must must be a clear and complete description of the proposed enhancement. The enhancement must represent a net improvement. The proposed implementation, if applicable, must not complicate the protocol unduly.

## RPIP Formats and Templates

RPIPs should be written in markdown format.

**RPIP Header Preamble**

Each RPIP must begin with an RFC 822 style header preamble, preceded and followed by three hyphens (---). This header is also termed “front matter” by Jekyll. The headers must appear in the following order.

`rpip`: RPIP number (this is determined by the RPIP editor)

`title`: The RPIP title is a few words, not a complete sentence

`author`: The list of the author’s or authors’ name(s) and/or username(s), or name(s) and email(s). Details are below.

`discussions-to`: The url pointing to the official discussion thread on dao.rocketpool.net

`status`: Draft, Review, Final, Stagnant, Withdrawn, Living

`type`: One of Protocol, Meta, or Informational. If Protocol, please include the category (`Core` or `RPRC`).

`created`: Date of RPIP  number assignment in yyyy-mm-dd format, e.g. 2001-08-14

`withdrawal-reason`: An optional sentence explaining why the RPIP was withdrawn.

Headers that permit lists must separate elements with commas.

Headers requiring dates will always do so in the format of ISO 8601 (yyyy-mm-dd).

### `author` header

The `author` header lists the names, email addresses or usernames of the authors/owners of the RPIP. Those who prefer anonymity may use a username only, or a first name and a username. The format of the `author` header value is:

    Random J. User (@username) <address@dom.ain>

Although authors are not required to provide a GitHub username or email address, at least one author must use a GitHub username in order to get notified on change requests and to approve or reject them.

## Linking to External Resources

Links to external resources **SHOULD NOT** be included. External resources may disappear, move, or change unexpectedly.

## Linking to other RPIPs

References to other RPIPs should follow the format `RPIP-N` where `N` is the RPIP number you are referring to. Each RPIP that is referenced in an RPIP **MUST** be accompanied by a relative markdown link the first time it is referenced, and **MAY** be accompanied by a link on subsequent references. For example, you would link to this RPIP with `[RPIP-1](/RPIP/rpip-1)`.

## Auxiliary Files

Images, diagrams and auxiliary files should be included in a subdirectory of the `assets` folder for that RPIP as follows: `assets/rpip-N` (where N is to be replaced with the RPIP number). When linking to an image in the RPIP, use relative links such as `[Image Title](../assets/rpip-1/image.png)`.

## RPIP Editors

The current RPIP editors are

    Mike Leach (@VVander) <mike@bamboofin.tech>

## RPIP Editor Responsibilities

For each RPIP, an editor does the following:

    Read the RPIP to check if it is ready: sound and complete. The ideas must make technical sense, even if they don’t seem likely to get to final status.
    The title should accurately describe the content.
    Check the RPIP for language (spelling, grammar, sentence structure, etc.), markup (GitHub flavored Markdown), code style

If the RPIP isn’t ready, the editor will send it back to the author for revision, with specific instructions.

Once the RPIP is ready for the repository, the RPIP editor will:

    Assign an RPIP number (generally the PR number, but the decision is with the editors)
    Merge the corresponding pull request
    Send a message back to the RPIP author with the next step.

The editors don’t pass judgment on RPIPs. We merely do the administrative & editorial part.

## Style Guide

### RPIP numbers

When referring to an RPIP by number, it should be written in the hyphenated form RPIP-X where X is the RPIP’s assigned number.

### RFC 2119

RPIPs are encouraged to follow RFC 2119 for terminology and to insert the following at the beginning of the Specification section:

    The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

## History

This document was derived heavily from Ethereum's EIP-1 written by [Martin Becze](mailto:mb@ethereum.org), [Hudson Jameson](mailto:hudson@ethereum.org), et al. which is in turn derived from Bitcoin’s BIP-0001 written by Amir Taaki which in turn was derived from Python’s PEP-0001. In many places text was simply copied and modified. Although the PEP-0001 text was written by Barry Warsaw, Jeremy Hylton, and David Goodger, they are not responsible for its use in the Rocket Pool Improvement Process, and should not be bothered with technical questions specific to Rocket Pool or the RPIP. Please direct all comments to the RPIP editors.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
