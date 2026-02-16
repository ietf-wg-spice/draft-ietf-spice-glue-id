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
  RFC2141:
  RFC8126:
  RFC5234:
  RFC9371:

---

--- abstract

This specification establishes a URN namespace for
GLobal Unique Enterprise (GLUE) Identifiers.
This enables URN identifiers to be used for businesses and organizations.
It enables organizational identities from existing authorities to be represented within this URN namespace.

--- middle

# Introduction

There are a myriad of entity identifier types for businesses and organizations.
With the increasing use of digital credentials, there is a need for a common
methodology for expressing these identifiers such that claims about and by such
entities can be made in a consistent and interoperable manner.

This specification establishes a URN namespace that standardizes the expression of
existing organizational entity identifiers by providing a common representation format.
It also establishes a registry for managing how existing organizational entity
identification mechanisms relate to this namespace.

Any organizational entity identifier whose identification mechanism has been registered
as an Authority Identifier in the registry may be represented as a GLUE URI.

## Requirements Notation and Conventions

{::boilerplate bcp14-tagged}

## Terminology

This specification uses the following terms:

GLUE URI:
: a URI that uses the GLUE URN namespace established in this specification.

External Authority:
: an organization that allocates External Identifiers for GLUE URIs using the Authority Identifier(s) over which they have jurisdiction.

Authority Identifier:
: identifier for the External Authority responsible for assigning the External Identifier used in GLUE URIs.

External Identifier:
: identifier assigned by an External Authority to identify a particular organization within GLUE URNs over which it has jurisdiction.

# Core Concepts

Every GLUE URI MUST
contain the following components:

- The Authority Identifier

- The External Identifier

## Uniqueness and Namespacing

Each GLUE URI MUST be globally unique.

A business entity can be identified by multiple GLUE URIs,
but each GLUE URI can only refer to a single business entity.

It is assumed that most registered organizational entity identification schemes
already handle any necessary namespacing as part of the External Identifier.
However, if collisions are possible within the set of possible external
identifiers for an Authority Identifier scheme, then further namespacing is
necessary at the GLUE URI level. Such namespacing MUST be done on the Authority
Identifier. The combination of the namespacing and the authority MUST result in
a unique Authority Identifier.

For example, assume there is an External Authority FEA that provides identifiers
for organizations in Singapore and South Korea. The identifiers issued in
Singapore are unique within Singapore, and the identifiers issued in South Korea
are unique within South Korea, but there is no guarantee that an organization in
Singapore will not be assigned the same identifier as an organization in South
Korea. Upon registration of FEA as an Authority Identifier, it would be
necessary to separately register two different Authority Identifiers (e.g.,
FEA-SG and FEA-KR) to provide differentiation between the two sets of External
Identifiers.

# GLUE URIs {#glue-uris}

GLUE URIs comply with [RFC3986].
They begin with `urn:glue:` and are followed by an Authority Identifier,
a colon character (":"), and the External Identifier allocated by the authority.

Authority Identifiers consist of a sequence of characters beginning with a
letter or digit and followed by any combination of letters, digits, plus ("+"),
period ("."), or hyphen ("-").
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
plus ("+"), period ("."), or hyphen ("-").
A digit or hyphen is allowed as the first character to permit the case where the
External Identifier is the representation of a number. It is specific to the
Authority Identifier whether the External Identifiers are case-insensitive or
case-sensitive. When they are case-insensitive, the canonical form is lowercase
and documents that specify External Identifiers must do so with lowercase
letters. There is a limit of 1000 characters for an External Identifier.
The ABNF [RFC5234] for External Identifiers is:

```
external-identifier = ( ALPHA / DIGIT / "-" ) *999( ALPHA / DIGIT / "+" / "-" / "." )
```

Combining these, the ABNF [RFC5234] for a GLUE URI is:

```
glue-uri = "urn:glue:" authority-identifier ":" external-identifier
```

For example, the following is a GLUE URI using the Authority Identifier "pen"
and the External Identifier "32473":

```
urn:glue:pen:32473
```

A GLUE URI is defined over the restricted US-ASCII syntax specified in this
section. Percent-encoding is not permitted. Consequently, GLUE URIs do not
support representation of External Identifiers whose canonical form includes
non-ASCII characters. This specification is therefore limited to identifier
systems whose canonical representations are fully within the permitted character
set.

The Authority Identifier MUST be registered in the GLUE URI Authority Identifier registry
defined in {{GLUE-URN}}.
The External Identifier MUST be the identifier assigned to the organization
by the External Authority.

# GLUE Authority Identifiers {#authority-identifiers}

This section defines the following GLUE Authority Identifiers.

| Organization     | Authority Identifier | External Authority Specification |
|:-----------------|:-------|:------------------------------------------|
| GS1              |  gln   | https://www.gs1.org/standards/id-keys/gln |
| GLEIF            |  lei   | https://www.iso.org/standard/78829.html   |
| Dun & Bradstreet |  duns  | https://www.dnb.com/duns.html             |
| Private Enterprise Numbers | pen | https://www.iana.org/assignments/enterprise-numbers |
| ISO/IEC 6523 | iso6523 | https://www.iso.org/standard/82246.html |

They are registered in the GLUE Authority Identifier URN Registry in {{GLUE-URN}}.

## Equivalence to Similar URIs

A GLUE URI is an identifier in a distinct URN namespace. By default, a GLUE URI
is not equivalent to any other URI, including a URI defined by the referenced
authority's own namespace. Equivalence between a GLUE URI and a non-GLUE URI
exists only when explicitly specified for a given Authority Identifier.
Implementations and relying parties MUST NOT assume equivalence between GLUE
URIs and non-GLUE URIs unless such equivalence is explicitly defined by the
authority or documented in the relevant registry entry.

### LEI URNs

[LEI-IANA] registers a URN namespace for LEIs. This means that LEIs can be
represented as URNs in at least two ways. Therefore there is an equivalence
between a GLUE URI with an "lei" Authority Identifier and an LEI URN, provided
the 20-digit LEI Code of the LEI URN is identifical to the External Identifier
of the GLUE URI. For example, "urn:lei:INR2EJN1ERAN0W5ZP974" is equivalent to
"urn:glue:lei:INR2EJN1ERAN0W5ZP974".

# Security Considerations

There are no additional security considerations beyond those already inherent to
using URNs.
Security considerations for URNs can be found in [RFC2141].

# Privacy Considerations

## Private Identifiers as Corporate Identifiers

There are some corporate identifiers that make use of personal identifiers. For
example, this is the case for some registered sole-proprietor businesses in the
United States, where the Tax ID may be the same as the Social Security Number
(SSN) of the business owner. Where the Tax ID uniquely identifies the business,
the SSN uniquely identifies an individual.

It is possible for such business identifiers to be represented as GLUE URIs. An
identifier's expression as a GLUE URI does not change the privacy
characteristics of that identifier. The same cautions and concerns need to be
taken with the GLUE URI representation as with the original identifier.

Implementers storing or evaluating GLUE URIs are encouraged to be aware the
privacy characteristics of each identification scheme represented by an
Authority Identifier and to appropriately handle any GLUE URI which violates
privacy policies.

# IANA Considerations

This section establishes a registry and populates it with its initial contents.
The following registration procedure is used for the
registry established by this specification.

Values are registered on a Specification Required [RFC8126]
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
and whether the registration makes sense.

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

## GLUE Authority Identifier URN Registry {#GLUE-URN}

This specification establishes the
IANA "GLUE Authority Identifier URN" registry
creating a URN namespace for Authority Identifiers for
GLobal Unique Enterprise (GLUE) Identifiers.

Each entry registers the URN for an Authority Identifier within the
"urn:glue:" namespace.
The organization responsible for the Authority Identifier is recorded.

IANA is requested to create the
"GLobal Unique Enterprise (GLUE) Identifiers"
registry group located at
https://www.iana.org/assignments/glue-identifiers/
and place this registry there.

### Registration Template

Authority Identifier:
: identifier for the External Authority responsible for assigning the External Identifier used in GLUE URIs.
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

Change Controller:
: For IETF stream RFCs, use "IETF".
For others, give the name of the responsible party.
Other details (e.g., postal address, e-mail address, home page URI) may also be included.

Specification Document(s):
: Reference to the document or documents that specify the Authority Identifier to be registered,
preferably including URLs that can be used to retrieve the documents.
An indication of the relevant sections may also be included, but is not required.

### Initial Registry Contents

#### gln

* Authority Identifier: gln
* URN: urn:glue:gln
* Organization: GS1
* Change Controller: IETF
* Specification Document(s): {{authority-identifiers}} of this specification, [GLN]

#### lei

* Authority Identifier: lei
* URN: urn:glue:lei
* Organization: GLEIF
* Change Controller: IETF
* Specification Document(s): {{authority-identifiers}} of this specification, [LEI], [LEI-IANA]

#### duns

* Authority Identifier: duns
* URN: urn:glue:duns
* Organization: Dun & Bradstreet
* Change Controller: IETF
* Specification Document(s): {{authority-identifiers}} of this specification, [DUNS]

#### pen

* Authority Identifier: pen
* URN: urn:glue:pen
* Organization: Private Enterprise Numbers
* Change Controller: IETF
* Specification Document(s): {{authority-identifiers}} of this specification, [PEN], [RFC9371]

#### iso6523

* Authority Identifier: iso6523
* URN: urn:glue:iso6523
* Organization: ISO/IEC 6523
* Change Controller: IETF
* Specification Document(s): {{authority-identifiers}} of this specification, [ISO6523]

--- back

# Acknowledgments
{:numbered="false"}

Martin Lindström,
Alexander (A.J.) Stein,
and
Martin Thomson
contributed to this specification.

# Document History
{: numbered="false"}

-05

* Added ISO/IEC 6523 identifiers.
* The first digit of the Authority Identifier may be a digit

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
