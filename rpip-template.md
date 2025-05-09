---
rpip: #<to be assigned>
title: <The RPIP title is a few words, not a complete sentence>
description: <Description is one full (short) sentence>
author: <a comma separated list of the author's or authors' name + GitHub username (in parenthesis), or name and email (in angle brackets).  Example, FirstName LastName (@GitHubUsername), FirstName LastName <foo@bar.com>, FirstName (@GitHubUsername) and GitHubUsername (@GitHubUsername)>
discussions-to: <URL>
status: Draft
type: <Protocol, Meta, or Informational>
category (*only required for Protocol ): <Core or RPRC>
created: <date created on, in ISO 8601 (yyyy-mm-dd) format>
requires (*optional): <RPIP number(s)>
---

This is the suggested template for new RPIPs.

Note that an RPIP number will be assigned by an editor. When opening a pull request to submit your RPIP, please use an abbreviated title in the filename, `rpip-draft_title_abbrev.md`.

The title should be 44 characters or less. It should not repeat the RPIP number in title, irrespective of the category. 

## Abstract
Abstract is a multi-sentence (short paragraph) technical summary. This should be a very terse and human-readable version of the specification section. Someone should be able to read only the abstract to get the gist of what this specification does.

## Motivation
The motivation section should describe the "why" of this RPIP. What problem does it solve? Why should someone want to implement this standard? What benefit does it provide to the Ethereum ecosystem? What use cases does this RPIP address?

## Specification
The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

The technical specification should describe the syntax and semantics of any new feature.

## Rationale
The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work, e.g. how the feature is supported in other languages.

## Backwards Compatibility
All RPIPs that introduce backwards incompatibilities must include a section describing these incompatibilities and their severity. The RPIP must explain how the author proposes to deal with these incompatibilities. RPIP submissions without a sufficient backwards compatibility treatise may be rejected outright.

## Test Cases
Test cases for an implementation are mandatory for RPIPs that are affecting protocol changes.  If the test suite is too large to reasonably be included inline, then consider adding it as one or more files in `../assets/rpip-####/`.

## Reference Implementation
An optional section that contains a reference/example implementation that people can use to assist in understanding or implementing this specification.  If the implementation is too large to reasonably be included inline, then consider adding it as one or more files in `../assets/rpip-####/`.

## Security Considerations
All RPIPs must contain a section that discusses the security implications/considerations relevant to the proposed change. Include information that might be important for security discussions, surfaces risks and can be used throughout the life cycle of the proposal. E.g. include security-relevant design decisions, concerns, important discussions, implementation-specific guidance and pitfalls, an outline of threats and risks and how they are being addressed. RPIP submissions missing the "Security Considerations" section will be rejected. An RPIP cannot proceed to status "Final" without a Security Considerations discussion deemed sufficient by the reviewers.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

# LaTeX Notes - Remove Section Before Submitting

LaTeX rendering between github and the rpips portal at rpips.rocketpool.net can differ. The following recommendations are made so that your equations will appear correctly on both:

LaTeX blocks should be delimited using the below. This should display similarly on both github and the rpips portal.

```math
$$

LaTeX goes here

$$
```

Inline LaTeX is buggy on the portal, and should be avoided. $ LaTeX goes here $ dollar-sign delimiters will work in some simple cases, but not universally.

Note that the $` LaTeX goes here `$ dollar-backtick delimiters do not work on the portal, and should never be used. 
