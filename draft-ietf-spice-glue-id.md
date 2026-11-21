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
 - Scheme
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

 -
    fullname: Orie Steele
    email: orie@or13.io

normative:
  RFC5234:
  RFC7595:
  DUNS:
    title: D-U-N-S Numbers
    target: https://www.dnb.com/en-us/smb/duns/what-is-a-duns-number.html
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
  RFC1034:
  RFC3986:
  RFC8126:
  RFC8264:
  RFC9562:
  RFC9839:
  RFC9371:
  IANA.URISchemes:
    title: Uniform Resource Identifier (URI) Schemes
    target: https://www.iana.org/assignments/uri-schemes

---

--- abstract

This specification defines GLobal Unique Enterprise (GLUE) Identifiers,
which are strings or URIs that identify businesses and organizations.
Every GLUE Identifier carries a prefix that makes it globally unique.
The specification establishes an IANA registry of prefixes, including prefixes
that are compatible with existing organizational identifier systems,
and prefixes that are simple strings rather than URIs.

--- middle

# Introduction

There are myriad entity identifier types for businesses and organizations.
With the increasing use of digital credentials, there is a need for a common
methodology for expressing these identifiers such that claims about and by such
entities can be made in a consistent and interoperable manner.

This specification defines GLUE Identifiers,
which are either URIs or strings of a simple prefixed form.
Both forms carry a prefix that makes the identifier globally unique.
URIs already have this property through their scheme.
Strings obtain it through a Prefix registered in an IANA registry
established by this specification.
That registry allows existing organizational entity identification mechanisms
to be represented as GLUE Identifiers,
and allows new Prefixes to be registered by anyone, without requiring
a new URI scheme or URN namespace.

## Requirements Notation and Conventions

{::boilerplate bcp14-tagged}

## Terminology

This specification uses the following terms:

GLUE Identifier:
: an identifier for an organization that is either a GLUE String or a URI.

GLUE String:
: a GLUE Identifier that is not a URI, of the form "glue_" Prefix "_" External Identifier, defined in {{glue-strings}}.

GLUE URI:
: a GLUE Identifier that is a URI, in which the URI scheme provides the global uniqueness; see {{glue-uris}}.

Prefix:
: the part of a GLUE String that makes it globally unique.
Each Prefix is registered in the registry established in {{GLUE-Prefix-Reg}}.
A Prefix typically corresponds to an External Authority.

External Identifier:
: the part of a GLUE String that follows the Prefix.
It is allocated by the External Authority to identify a particular organization
within the Prefix over which that authority has jurisdiction.

External Authority:
: an organization that allocates External Identifiers over which it has jurisdiction that are used in GLUE Strings with its Prefix(es).

# Core Concepts

Every GLUE Identifier is globally unique and is either:

- a GLUE String, which MUST contain a registered Prefix
  and an External Identifier; or

- a URI, whose scheme (and, where applicable, namespace) makes it globally unique.
  Examples are "urn:lei:7ltwfzyicnsx8d621k86" and "https://example.com/entities/1".

## Uniqueness and Namespacing

Each GLUE Identifier is a globally unique identifier.
An organization can be identified by multiple GLUE Identifiers,
but each distinct GLUE Identifier can only refer to a single organization.
If multiple External Authorities have issued identifiers for the same organization
(e.g., the organization has both a Dun & Bradstreet number and a Private Enterprise Number),
then the same organization can be identified by GLUE Strings differentiated by the Prefix.

It is assumed that most registered organizational entity identification schemes
already handle any namespacing necessary to achieve uniqueness as part of the External Identifier.
However, if collisions are possible within the set of possible external
identifiers for a Prefix scheme, then further namespacing is
necessary at the Prefix level.
Such namespacing could be done by allocating multiple Prefixes.
The combination of the Prefix and the External Identifier
MUST result in a unique GLUE String.

For example, assume there is an External Authority FEA that provides identifiers
for organizations in Singapore and South Korea. The identifiers issued in
Singapore are unique within Singapore, and the identifiers issued in South Korea
are unique within South Korea, but there is no guarantee that an organization in
Singapore will not be assigned the same identifier as an organization in South
Korea. Upon registration of FEA as a Prefix, it would be
necessary to separately register two different Prefixes (e.g.,
fea-sg and fea-kr) to provide differentiation between the two sets of External
Identifiers.

# GLUE Strings {#glue-strings}

A GLUE String begins with the literal "glue_",
followed by a Prefix, an underscore character ("_"),
and the External Identifier allocated by the authority.

Prefixes consist of a sequence of characters beginning with a
letter or digit and followed by any combination of letters, digits,
hyphen ("-"), or period (".").
This repertoire is compatible with DNS names {{RFC1034}}.
A Prefix MUST NOT contain an underscore, so the Prefix ends at
the first underscore that follows "glue_".
Although Prefixes are case-insensitive, the canonical form is
lowercase and documents that specify Prefixes must do so with
lowercase letters. An implementation should accept uppercase letters as
equivalent to lowercase in Prefix names (e.g., allow "EXAMPLE" as
well as "example") for the sake of robustness but should only produce lowercase
Prefix names for consistency. There is a limit of 253 characters
for the length of a Prefix.
The ABNF [RFC5234] for Prefixes is:

```
prefix = (ALPHA / DIGIT) *252( ALPHA / DIGIT / "-" / "." )
```

External Identifiers consist of a sequence of unreserved characters
{{Section 2.3 of RFC3986}}: letters, digits,
hyphen ("-"), period ("."), underscore ("_"), and tilde ("~").
It is specific to the
Prefix whether the External Identifiers are case-insensitive or
case-sensitive. When they are case-insensitive, the canonical form is lowercase
and documents that specify External Identifiers MUST do so with lowercase letters.
Always using the canonical form of these identifiers means that code performing comparisons
need not be aware of whether External Identifiers are case-sensitive or not;
case-sensitive comparisons can always be performed
on the whole GLUE String.

While the original representation of some External Identifiers may contain
characters that are ignored, these MUST be omitted when used in GLUE Strings.
For instance, Dun & Bradstreet {{DUNS}} identifiers sometimes contain
hyphen characters that are ignored.
The DUNS numbers 12-345-6789 and 123456789 are considered to be equivalent.
The latter form without the ignored characters MUST be used
as the External Identifier in GLUE Strings.

Documents that define a new Prefix
MUST NOT allow the representation of an External Identifier
to contain characters outside the set above, such as the colon character.
Specifications MUST define a substitute character,
such as period, that is used in place of any such character in External Identifiers.
Any substitution can be specified in the Transformation Rules
in the IANA registration for a Prefix; see {{GLUE-Prefix-Reg}}.

There is a limit of 1000 characters for an External Identifier.
The ABNF [RFC5234] for External Identifiers is:

```
external-identifier = 1*1000( ALPHA / DIGIT / "-" / "." / "_" / "~" )
```

Combining these, the ABNF [RFC5234] for a GLUE String is:

```
glue-string = "glue_" prefix "_" external-identifier
```

For example, the following is a GLUE String using the Prefix "pen"
and the External Identifier "32473". This example uses the Enterprise Number "32473" reserved for documentation in {{?RFC5612}}.

```
glue_pen_32473
```

The Prefix MUST be registered in the GLUE Prefix registry
established in {{GLUE-Prefix-Reg}}.
The External Identifier MUST be the identifier assigned to the organization
by the External Authority.

## Recommendations for New Prefixes {#recommendations}

The following recommendations apply to documents that register new Prefixes.

- Consider using a DNS name {{RFC1034}} under the control of the External Authority
  as the Prefix (e.g., "example.com"), because DNS names are already globally
  unique and have an established allocation and ownership model.
  A DNS-name Prefix also helps limit conflicts between registrations.

- Consider using a UUID {{RFC9562}} in its lowercase hexadecimal string form
  as the External Identifier,
  because a UUID can be generated without coordination and is URL-safe.
  For example, "glue_example.com_3f2504e0-4f89-41d3-9a0c-0305e82c3301".

- Restrict both the Prefix and the External Identifier to URL-safe characters,
  as defined by this section.

- If a Prefix registration needs External Identifiers that contain
  non-ASCII characters, the registration specifies the permitted repertoire.
  That repertoire SHOULD be the Unicode Assignables subset defined in {{RFC9839}},
  and the registration SHOULD consider applying the PRECIS framework {{RFC8264}}
  for preparation, enforcement and comparison of the strings.
  Such identifiers are not URL-safe and need percent-encoding when embedded in a URI.

# GLUE URIs {#glue-uris}

A URI {{RFC3986}} can be used as a GLUE Identifier.
Because the URI scheme is already globally unique,
no Prefix registration is needed.
This specification does not define a new URI scheme.

Existing identifier systems that already have a URI form,
for example LEIs in the "urn:lei" namespace {{LEI-IANA}},
can be used directly as GLUE URIs.
See {{equivalence}} for the relationship between such URIs and GLUE Strings.

# Initial GLUE Prefixes {#prefixes}

This section defines the GLUE Prefixes listed in {{glue-def}}.
Each is compatible with an existing, globally unique identifier system.

| Organization     | Prefix | Example GLUE String | External Authority Specification |
|:-----------------|:-------|:--------------------|:---------------------|
| GLEIF            |  lei   | glue_lei_7ltwfzyicnsx8d621k86 | https://www.iso.org/standard/78829.html |
| Dun & Bradstreet |  duns  | glue_duns_123456789 | https://www.dnb.com/en-us/smb/duns/what-is-a-duns-number.html |
| Private Enterprise Numbers | pen | glue_pen_32473 | https://www.iana.org/assignments/enterprise-numbers |
| ISO/IEC 6523 | iso6523 | glue_iso6523_0060.123456789 | https://www.iso.org/standard/82246.html |
{: #glue-def title='Defined GLUE Prefixes'}

These are registered in the GLUE Prefix Registry
established in {{GLUE-Prefix-Reg}}.
Other Prefixes can be registered as described there,
including Prefixes that are not tied to an existing identifier system.

## Equivalence to Similar Identifiers {#equivalence}

A GLUE String is an identifier in its own namespace. By default, a GLUE String
is not equivalent to any other identifier, including a URI defined by the referenced
authority's own namespace. Equivalence between a GLUE String and a URI
exists only when explicitly specified for a given Prefix.
Implementations and relying parties MUST NOT assume equivalence between GLUE
Strings and URIs unless such equivalence is explicitly defined by the
authority and the party understands that particular Prefix
and the specified equivalence.

### LEI URNs

[LEI-IANA] registers a URN namespace for Legal Entity Identifiers (LEIs).
This means that LEIs can be
represented in at least two ways. Therefore there is an equivalence
between a GLUE String with an "lei" Prefix and an LEI URN, provided
the 20-character LEI Code of the LEI URN is identical to the External Identifier
of the GLUE String, after converting uppercase letters to lowercase.
For example, "urn:lei:7ltwfzyicnsx8d621k86" is equivalent to
"glue_lei_7ltwfzyicnsx8d621k86".
The LEI Code used in this example is the one given as an example
in the LEI URN namespace registration {{LEI-IANA}};
no LEI Code is reserved for use in examples.

# Security Considerations

The security considerations inherent to using URIs apply to GLUE URIs.
Security considerations for URIs can be found in {{RFC3986}}.

The global uniqueness of GLUE Identifiers prevents situations in which
the same identifier is allocated in different local namespaces
and cannot be disambiguated when used.
For instance both Canadian and Singaporean organization registries might use
the local identifier "42", but with these referring to different organizations.
Embedding these local identifiers in GLUE Strings with distinct Prefixes enables disambiguation.

A Prefix identifies the namespace, not the party presenting an identifier.
A GLUE Identifier on its own does not authenticate that the presenter is the organization it names.
Relying parties need to establish that by other means.

A DNS name used as a Prefix can change hands when the domain registration lapses or is transferred.
Registrations of such Prefixes are subject to the review described in {{GLUE-Prefix-Reg}}.
Registered Prefixes remain bound to the registrant of record in the registry,
whatever later happens to the domain.

# Privacy Considerations

## Private Identifiers as Corporate Identifiers

There are some corporate identifiers that make use of personal identifiers. For
example, this is the case for some registered sole-proprietor businesses in the
United States, where the Tax ID may be the same as the Social Security Number
(SSN) of the business owner. Where the Tax ID uniquely identifies the business,
the SSN uniquely identifies an individual.

It is possible for such business identifiers to be represented as GLUE Identifiers. An
identifier's expression as a GLUE Identifier does not change the privacy
characteristics of that identifier. The same cautions and concerns need to be
taken with the GLUE Identifier representation as with the original identifier.

Implementers storing or evaluating GLUE Identifiers are encouraged to be aware the
privacy characteristics of each identification scheme represented by a
Prefix and to appropriately handle any GLUE Identifier that violates
privacy policies.

# IANA Considerations

## GLUE Prefix Registry {#GLUE-Prefix-Reg}

This specification establishes the
IANA "GLUE Prefix" registry
creating a URI namespace for Prefixes for
GLobal Unique Enterprise (GLUE) Identifiers.

Each entry registers a Prefix within the
"glue:" namespace.
The organization responsible for the Prefix is recorded.

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
(e.g., "Request to register URI glue:example").

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
operated by an External Authority identified by the proposed Prefix.

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

Prefix:
: Identifier for the External Authority responsible for assigning the External Identifier used in GLUE URIs.
This identifier
is not case sensitive and any letters MUST be expressed in lowercase characters.
It MUST consist of a sequence of characters with a mazimum length of 50,
beginning with a letter or digit and followed by any combination of
letters, digits, plus ("+"), period ("."), or hyphen ("-").

URI:
: The URI within the "glue:" namespace
consisting of "glue:" followed by
the Prefix.

Organization:
: The organization responsible for the Prefix.

Transformation Rules:
: Syntactic transformations applied to the original External Identifiers when creating the GLUE URI, or "N/A" if none.

Change Controller:
: For IETF stream RFCs, use "IETF".
For others, give the name of the responsible party.
Other details (e.g., postal address, e-mail address, or home page URL) may also be included.

Specification Document(s):
: Reference to the document or documents that specify the Prefix to be registered,
preferably including URLs that can be used to retrieve the documents.
An indication of the relevant sections may also be included, but is not required.

### Initial Registry Contents

#### lei

{:compact}
Prefix:
: lei

URI:
: glue:lei

Organization:
: GLEIF

Transformation Rules:
: Convert uppercase characters to lowercase.

Change Controller:
: IETF

Specification Document(s):
: {{prefixes}} of \[\[ this specification ]], [LEI], [LEI-IANA]


#### duns

{:compact}
Prefix:
: duns

URI:
: glue:duns

Organization:
: Dun & Bradstreet

Transformation Rules:
: Delete hyphen characters.

Change Controller:
: IETF

Specification Document(s):
: {{prefixes}} of \[\[ this specification ]], [DUNS]


#### pen

{:compact}
Prefix:
: pen

URI:
: glue:pen

Organization:
: Private Enterprise Numbers

Transformation Rules:
: N/A

Change Controller:
: IETF

Specification Document(s):
: {{prefixes}} of \[\[ this specification ]], [PEN], [RFC9371]


#### iso6523

{:compact}
Prefix:
: iso6523

URI:
: glue:iso6523

Organization:
: ISO/IEC 6523

Transformation Rules:
: Substitute period for any colon characters.

Change Controller:
: IETF

Specification Document(s):
: {{prefixes}} of \[\[ this specification ]], [ISO6523]


## URI Scheme Registration

This specification registers the following URI scheme in the
IANA "Uniform Resource Identifier (URI) Schemes" registry {{IANA.URISchemes}}
established by {{RFC7595}}.

### glue

{:compact}
Scheme Name:
: glue

Status:
: Permanent

Applications/protocols that use this scheme name:
: Systems that need to refer to organizational entities utilizing interoperable identifiers derived from existing organizational identifier systems

Contact:
: Michael B. Jones - michael_b_jones@hotmail.com

Change Controller:
: IETF

References:
: \[\[ this specification ]]


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

-10

* Removed GS1 usage.

-09

* Changed from using a URN namespace to a URI scheme,
  based on feedback from the URN designated experts.

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
