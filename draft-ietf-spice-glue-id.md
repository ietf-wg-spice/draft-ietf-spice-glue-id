---
title: "GLobal Unique Enterprise (GLUE) Identifiers"
abbrev: "SPICE GLUE"
lang: en
category: std

docname: draft-ietf-spice-glue-id-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Security"
workgroup: "Secure Patterns for Internet CrEdentials"
keyword:
 - URI
 - URN
 - Enterprise
 - Entity Identifiers
venue:
  group: "Secure Patterns for Internet CrEdentials"
  type: "Working Group"
  mail: "spice@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/spice/"
  github: "ietf-wg-spice/draft-ietf-spice-glue-id"
  latest: "https://ietf-wg-spice.github.io/draft-ietf-spice-glue-id/draft-ietf-spice-glue-id.html"

author:
 -
    fullname: Brent W. Zundel
    email: brent.zundel@gmail.com
    country: United States

 -
    fullname: Pamela Dingle
    organization: Microsoft Corporation
    email: pamela.dingle@microsoft.com
    country: United States

 -
    fullname: Michael B. Jones
    ins: M.B. Jones
    organization: Self-Issued Consulting
    email: michael_b_jones@hotmail.com
    uri: https://self-issued.info/
    country: United States

normative:
  RFC3986:
  RFC5234:
  GLN:
    title: Global Location Nymber (GLN)
    target: https://www.gs1.org/standards/id-keys/gln
  DUNS:
    title: D-U-N-S Numbers
    target: https://www.dnb.com/duns.html
  LEI:
    title: Legal Entity Identifier (LEI)
    date: 2020
    target: https://www.iso.org/standard/78829.html
  PEN:
    title: Private Enterprise Numbers
    target: https://www.iana.org/assignments/enterprise-numbers
  ISO6523:
    title: "ISO/IEC 6523-1:2023. Information technology — Structure for the identification of organizations and organization parts, Part 1: Identification of organization identification schemes"
    date: 2023
    target: https://www.iso.org/standard/82246.html
  LEI-IANA:
    title: LEI Namespace Identifier
    target: https://www.iana.org/assignments/urn-formal/lei

informative:
  RFC8126:
  RFC8141:
  RFC9371:
  IANA.URN:
    title: Formal URN Namespaces
    target: https://www.iana.org/assignments/urn-namespaces/

---

--- abstract

This specification establishes a URN namespace for
GLobal Unique Enterprise (GLUE) Identifiers.
This enables URN identifiers to be used for businesses and organizations.
It enables organizational identities from existing authorities to be represented within this URN namespace.

--- middle

# Introduction

There are myriad entity identifier types for businesses and organizations.
With the increasing use of digital credentials, there is a need for a common
methodology for expressing these identifiers such that claims about and by such
entities can be made in a consistent and interoperable manner.

This specification establishes a URN namespace that standardizes the expression of
existing organizational entity identifiers by providing a common representation format.
It also establishes an IANA registry for managing how existing organizational entity
identification mechanisms relate to this namespace.

Any organizational entity identifier whose identification mechanism has been registered
as an Authority Identifier in the registry may be represented as a GLUE URN.

## Requirements Notation and Conventions

{::boilerplate bcp14-tagged}

## Terminology

This specification uses the following terms:

GLUE URN:
: a URN that uses the GLUE URN namespace established in this specification.

Authority Identifier:
: identifier for the External Authority responsible for assigning the External Identifiers used in GLUE URNs over which it has jurisdiction.

External Identifier:
: identifier assigned by an External Authority to identify a particular organization within GLUE URNs with Authority Identifier(s) over which it has jurisdiction.

External Authority:
: an organization that allocates External Identifiers over which it has jurisdiction that are used in GLUE URNs with its Authority Identifier(s).

# Core Concepts

Every GLUE URN MUST
contain the following components:

- The Authority Identifier

- The External Identifier

## Uniqueness and Namespacing

Each GLUE URN is globally unique identifier.
An organization can be identified by multiple GLUE URNs,
but each distinct GLUE URN can only refer to a single organization.
If multiple External Authorities have issued identifiers for the same organization
(e.g., the organization has both a Dun & Bradstreet number and a Private Enterprise Number),
then the same organization can be identified by GLUE URNs differentiated by the Authority Identifier.

It is assumed that most registered organizational entity identification schemes
already handle any namespacing necessary to achieve uniqueness as part of the External Identifier.
However, if collisions are possible within the set of possible external
identifiers for an Authority Identifier scheme, then further namespacing is
necessary at the GLUE URN level.
Such namespacing could be done by allocating multiple Authority Identifiers.
The combination of the Authority Identifier and the External Identifier
MUST result in a unique GLUE URN.

For example, assume there is an External Authority FEA that provides identifiers
for organizations in Singapore and South Korea. The identifiers issued in
Singapore are unique within Singapore, and the identifiers issued in South Korea
are unique within South Korea, but there is no guarantee that an organization in
Singapore will not be assigned the same identifier as an organization in South
Korea. Upon registration of FEA as an Authority Identifier, it would be
necessary to separately register two different Authority Identifiers (e.g.,
FEA-SG and FEA-KR) to provide differentiation between the two sets of External
Identifiers.

# GLUE URNs {#glue-urns}

GLUE URNs comply with [RFC3986].
They begin with "urn:glue:" and are followed by an Authority Identifier,
a colon character (":"), and the External Identifier allocated by the authority.

Authority Identifiers consist of a sequence of characters beginning with a
letter or digit and followed by any combination of letters, digits, plus ("+"),
hyphen ("-"), or period (".").
Although Authority Identifiers are case-insensitive, the canonical form is
lowercase and documents that specify Authority Identifiers must do so with
lowercase letters. An implementation should accept uppercase letters as
equivalent to lowercase in Authority Identifier names (e.g., allow "EXAMPLE" as
well as "example") for the sake of robustness but should only produce lowercase
Authority Identifier names for consistency. There is a limit of 50 characters
for the length of an Authority Identifier.
The ABNF [RFC5234] for Authority Identifiers is:

```
authority-identifier = (ALPHA/DIGIT) *49( ALPHA / DIGIT / "+" / "-" / "." )
```

External Identifiers consist of a sequence of characters beginning with a letter
or digit or hyphen ("-") and followed by any combination of letters, digits,
plus ("+"), hyphen ("-"), or period (".").
A digit or hyphen is allowed as the first character to permit the case where the
External Identifier is the representation of a number. It is specific to the
Authority Identifier whether the External Identifiers are case-insensitive or
case-sensitive. When they are case-insensitive, the canonical form is lowercase
and documents that specify External Identifiers MUST do so with lowercase letters.
Always using the canonical form of these URNs means that code performing comparisons
need not be aware of whether External Identifiers are case-sensitive or not;
case-sensitive comparisons can always be performed
on the namespace specific string.

While the original representation of some External Identifiers may contain
characters that are ignored, these MUST be omitted when used in GLUE URNs.
For instance, Dun & Bradstreet {{DUNS}} identifiers sometimes contain
dash characters that are ignored.
The DUNS numbers 12-345-6789 and 123456789 are considered to be equivalent.
The latter form without the ignored characters MUST be used
as the External Identifier in GLUE URNs.

Finally, documents that define a new Authority Identifier type
MUST NOT allow the representation of an External Identifier
to contain the colon character.
Specifications MUST define a substitute character,
such as period, that is used in place of a colon in External Identifiers.
Any substitution can be specified in the Transformation Rules
in the IANA registration for an Authority Identifier; see {{GLUE-Authority-Reg}}.

There is a limit of 1000 characters for an External Identifier.
The ABNF [RFC5234] for External Identifiers is:

```
external-identifier = ( ALPHA / DIGIT / "-" ) *999( ALPHA / DIGIT / "+" / "-" / "." )
```

Combining these, the ABNF [RFC5234] for a GLUE URN is:

```
glue-urn = "urn:glue:" authority-identifier ":" external-identifier
```

For example, the following is a GLUE URN using the Authority Identifier "pen"
and the External Identifier "32473". This example uses the Enterprise Number "32473" reserved for documentation in {{?RFC5612}}.

```
urn:glue:pen:32473
```

A GLUE URN is defined over the restricted US-ASCII syntax specified in this
section. Percent-encoding is not permitted. Consequently, GLUE URNs do not
support representation of External Identifiers that use
non-ASCII characters. This specification is therefore limited to identifier
systems whose representations can be expressed fully within the permitted character set.

The Authority Identifier MUST be registered in the GLUE URN Authority Identifier registry
defined in {{GLUE-Authority-Reg}}.
The External Identifier MUST be the identifier assigned to the organization
by the External Authority.

# GLUE Authority Identifiers {#authority-identifiers}

This section defines the GLUE Authority Identifiers listed in {{glue-def}}.

| Organization     | Authority Identifier | External Authority Specification |
|:-----------------|:-------|:------------------------------------------|
| GS1              |  gln   | https://www.gs1.org/standards/id-keys/gln |
| GLEIF            |  lei   | https://www.iso.org/standard/78829.html   |
| Dun & Bradstreet |  duns  | https://www.dnb.com/duns.html             |
| Private Enterprise Numbers | pen | https://www.iana.org/assignments/enterprise-numbers |
| ISO/IEC 6523 | iso6523 | https://www.iso.org/standard/82246.html |
{: #glue-def title='Defined GLUE Authority Identifiers'}

These are registered in the GLUE Authority Identifier URN Registry in {{GLUE-Authority-Reg}}.

## Equivalence to Similar URNs

A GLUE URN is an identifier in a distinct URN namespace. By default, a GLUE URN
is not equivalent to any other URN, including a URN defined by the referenced
authority's own namespace. Equivalence between a GLUE URN and a non-GLUE URN
exists only when explicitly specified for a given Authority Identifier.
Implementations and relying parties MUST NOT assume equivalence between GLUE
URNs and non-GLUE URNs unless such equivalence is explicitly defined by the
authority and the party understands that particular Authority Identifier
and the specified equivalance.

### LEI URNs

[LEI-IANA] registers a URN namespace for Legal Entity Identifiers (LEIs).
This means that LEIs can be
represented as URNs in at least two ways. Therefore there is an equivalence
between a GLUE URN with an "lei" Authority Identifier and an LEI URN, provided
the 20-digit LEI Code of the LEI URN is identical to the External Identifier
of the GLUE URN. For example, "urn:lei:inr2ejn1eran0w5zp974" is equivalent to
"urn:glue:lei:inr2ejn1eran0w5zp974".

# Security Considerations

The security considerations inherent to using URNs apply.
Security considerations for URNs can be found in {{RFC8141}}.

The global uniqueness of GLUE URNs prevents situations in which
the same identifier is allocated in different local namespaces
and cannot be disambiguated when used.
For instance both Canadian and Singaporean organization registries might use
the local identifier "42", but with these referring to different organizations.
Embedding these local identifiers in GLUE URNs enables disambiguation.

# Privacy Considerations

## Private Identifiers as Corporate Identifiers

There are some corporate identifiers that make use of personal identifiers. For
example, this is the case for some registered sole-proprietor businesses in the
United States, where the Tax ID may be the same as the Social Security Number
(SSN) of the business owner. Where the Tax ID uniquely identifies the business,
the SSN uniquely identifies an individual.

It is possible for such business identifiers to be represented as GLUE URNs. An
identifier's expression as a GLUE URN does not change the privacy
characteristics of that identifier. The same cautions and concerns need to be
taken with the GLUE URN representation as with the original identifier.

Implementers storing or evaluating GLUE URNs are encouraged to be aware the
privacy characteristics of each identification scheme represented by an
Authority Identifier and to appropriately handle any GLUE URN which violates
privacy policies.

# IANA Considerations

## GLUE Authority Identifier Registry {#GLUE-Authority-Reg}

This specification establishes the
IANA "GLUE Authority Identifier" registry
creating a URN namespace for Authority Identifiers for
GLobal Unique Enterprise (GLUE) Identifiers.

Each entry registers an Authority Identifier within the
"urn:glue:" namespace.
The organization responsible for the Authority Identifier is recorded.

IANA is requested to create the
"GLobal Unique Enterprise (GLUE) Identifiers"
registry group located at
https://www.iana.org/assignments/glue-identifiers/
and place this registry there.

Values are registered on a Specification Required ({{Section 4.6 of RFC8126}})
basis after a two-week review period on the spice-ext-review@ietf.org
mailing list, on the advice of one or more Designated Experts.
However, to allow for the allocation of values prior to publication
of the final version of a specification,
the Designated Experts may approve registration once they are satisfied
that the specification will be completed and published.
However, if the specification is not completed and published
in a timely manner, as determined by the Designated Experts,
the Designated Experts may request that IANA withdraw the registration.

Registration requests sent to the mailing list for review should use
an appropriate subject
(e.g., "Request to register URN urn:glue:example").

Within the review period, the Designated Experts will either approve or deny
the registration request, communicating this decision to the review list and IANA.
The Designated Experts verify that a specification exists. Experts are
encouraged to be biased towards approving registrations unless they are abusive,
frivolous, or actively harmful (not merely aesthetically displeasing or
architecturally dubious).

Denials should include an explanation and, if applicable,
suggestions as to how to make the request successful.
If the designated experts are not responsive,
the registration requesters should contact IANA to escalate the process.

Criteria that should be applied by the Designated Experts includes
determining whether the proposed registration duplicates existing functionality,
determining whether it is likely to be of general applicability
or whether it is useful only for a single application,
and whether the registration references an existing organizational registry
operated by an External Authority identified by the proposed Authority Identifier.

IANA must only accept registry updates from the Designated Experts and should direct
all requests for registration to the review mailing list.

It is suggested that multiple Designated Experts be appointed who are able to
represent the perspectives of different applications using this specification,
in order to enable broadly-informed review of registration decisions.
In cases where a registration decision could be perceived as
creating a conflict of interest for a particular Expert,
that Expert should defer to the judgment of the other Experts.

The reason for the use of the mailing list is to enable
public review of registration requests, enabling both Designated Experts
and other interested parties to provide feedback on proposed registrations.
The reason to allow the Designated Experts to
allocate values prior to publication as a final specification is to enable
giving authors of specifications proposing registrations
the benefit of review by the Designated Experts
before the specification is completely done,
so that if problems are identified, the authors can iterate and fix them
before publication of the final specification.

### Registration Template

Authority Identifier:
: identifier for the External Authority responsible for assigning the External Identifier used in GLUE URNs.
This identifier
is not case sensitive and any letters MUST be expressed in lowercase characters.
It MUST consist of a sequence of characters with a mazimum length of 50,
beginning with a letter and followed by any combination of
letters, digits, plus ("+"), period ("."), or hyphen ("-").

URN:
: The URN within the "urn:glue:" namespace
consisting of "urn:glue:" followed by
the Authority Identifier.

Organization:
: The organization responsible for the Authority Identifier.

Transformation Rules:
: Syntactic transformations applied to the original External Identifiers when creating the GLUE URN, or "N/A" if none.

Change Controller:
: For IETF stream RFCs, use "IETF".
For others, give the name of the responsible party.
Other details (e.g., postal address, e-mail address, or home page URL) may also be included.

Specification Document(s):
: Reference to the document or documents that specify the Authority Identifier to be registered,
preferably including URLs that can be used to retrieve the documents.
An indication of the relevant sections may also be included, but is not required.

### Initial Registry Contents

#### gln

{:compact}
Authority Identifier:
: gln

URN:
: urn:glue:gln

Organization:
: GS1

Transformation Rules:
: N/A

Change Controller:
: IETF

Specification Document(s):
: {{authority-identifiers}} of this specification, [GLN]


#### lei

{:compact}
Authority Identifier:
: lei

URN:
: urn:glue:lei

Organization:
: GLEIF

Transformation Rules:
: Convert uppercase characters to lowercase.

Change Controller:
: IETF

Specification Document(s):
: {{authority-identifiers}} of this specification, [LEI], [LEI-IANA]


#### duns

{:compact}
Authority Identifier:
: duns

URN:
: urn:glue:duns

Organization:
: Dun & Bradstreet

Transformation Rules:
: Delete dash characters.

Change Controller:
: IETF

Specification Document(s):
: {{authority-identifiers}} of this specification, [DUNS]


#### pen

{:compact}
Authority Identifier:
: pen

URN:
: urn:glue:pen

Organization:
: Private Enterprise Numbers

Transformation Rules:
: N/A

Change Controller:
: IETF

Specification Document(s):
: {{authority-identifiers}} of this specification, [PEN], [RFC9371]


#### iso6523

{:compact}
Authority Identifier:
: iso6523

URN:
: urn:glue:iso6523

Organization:
: ISO/IEC 6523

Transformation Rules:
: Substitute period for any colon characters.

Change Controller:
: IETF

Specification Document(s):
: {{authority-identifiers}} of this specification, [ISO6523]


## URN Namespace Registration

This specification registers the following URN namespace in the
IANA "Formal URN Namespaces" registry {{IANA.URN}} established by {{RFC8141}}.

### urn:glue

{:compact}
Namespace Identifier:
: glue

Version:
: 1

Date:
: 2026-03-02

Registrant:
: IETF

Purpose:
: GLobal Unique Enterprise (GLUE) identifier namespace,
  established by \[\[ this specification ]]

Syntax:
: As specified in the glue-urn ABNF entry in {{glue-urns}} of \[\[ this specification ]]

Assignment:
: Specification Required ({{Section 4.6 of RFC8126}}) basis

Security and Privacy:
: GLUE URNs are public identifiers for public entities.

Interoperability:
: GLUE URNs encapsulate other existing identifier namespaces.
  Therefore, they provide a globally unique equivalent to identifiers within
  those local namespaces.
  Some of those namespaces may also utilize URNs.

Resolution:
: Resolution of GLUE URNs, which are identifiers, is not performed.

Documentation:
: \[\[ this specification ]]

Additional Information:
: N/A

Revision Information:
: N/A

--- back

# Acknowledgments
{:numbered="false"}

Amanda Baber,
Carsten Bormann,
Mohamed Boucadair,
Tim Bray,
Deb Cooley,
Patrik Fältström,
Arnt Gulbrandsen,
Sue Hares,
John Klensin,
Erik Kline,
Martin Lindström,
Rohan Mahy,
James Manger,
Peter Saint-Andre,
Orie Steele,
Alexander (A.J.) Stein,
Martin Thomson,
Éric Vyncke,
Dale Worley,
and Peter Yee
contributed to this specification.

# Document History
{: numbered="false"}

-08

* Addressed urn:glue registration feedback by Dale Worley
  about identifier uniqueness and defining Transformation Rules, when needed.
* Addressed urn:glue registration feedback by Peter Saint-Andre
  about using "URN" rather than "URI",
  applying suggested language about when
  multiple External Authorities have issued identifiers for the same organization,
  and correcting issues with the registry descriptions.

-07

* Registered urn:glue per Amanda Baber's instructions.

-06

* Addressed Deb Cooley's review comments, specifically:
  - Reworked term definitions.
  - Added security consideration about uniqueness of GLUE URIs.
  - Updated registration instructions to include a criterion about
    an existing organizational registry operated by an External Authority
    identified by the proposed Authority Identifier.
* Addressed Mohamed Boucadair's review comments, specifically:
  - Used neutral language "organization" that doesn't assume that all organizations are businesses.
  - Reworded confusing text about uniqueness.
  - Made the RFC 5234 reference normative.
* Addressed Erik Kline's review comments, specifically:
  - Expanded LEI on first use to Legal Entity Identifier (LEI).
  - Included instructions to the designated experts about ensuring that
    the registration references an existing organizational registry
    operated by an External Authority identified by the proposed Authority Identifier.

-05

* Added ISO/IEC 6523 identifiers.
* The first character of the Authority Identifier may be a digit.
* Fixed wording in IANA Considerations.
* Limited character set to US-ASCII.
* Fixed multiple nits from WGLC.

-04

* Applied review suggestions from Martin Thomson, specifically:
  - Added references for each registered Authority Identifier.
  - Added size limits for Authority Identifiers and External Identifiers.
  - Added a note about LEI URNs.

-03

* Use the urn:glue URN namespace and delete the urn:ietf:spice URN namespace.
* Addressed early IANA feedback.

-02

* Improved several descriptions in the specification.

-01

* Updated Brent's affiliation.

-00

* Initial working group draft, based on draft-zundel-spice-glue-id-02
