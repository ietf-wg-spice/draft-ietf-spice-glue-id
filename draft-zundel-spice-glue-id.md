---
title: "SPICE GLUE: GLobal Unique Enterprise (GLUE) Identifiers"
abbrev: "SPICE GLUE"
lang: en
category: std

docname: draft-zundel-spice-glue-id-latest
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
  github: "mesur-io/draft-zundel-spice-glue-id"
  latest: "https://mesur-io.github.io/draft-zundel-spice-glue-id/draft-zundel-spice-glue-id.html"

author:
 -
    fullname: Brent Zundel
    organization: mesur.io
    email: brent.zundel@gmail.com
    country: United States

 -
    fullname: Pamela Dingle
    organization: Microsoft Corporation
    email: pamela.dingle@microsoft.com
    country: United States

- fullname: Michael B. Jones
  ins: M.B. Jones
  organization: Self-Issued Consulting
  email: michael_b_jones@hotmail.com
  uri: https://self-issued.info/
  country: United States

normative:
  RFC3986:

informative:
  RFC2141:
  RFC8126:


--- abstract

This specification establishes an IETF URN namespace for
GLobal Unique Enterprise (GLUE) Identifiers.
It also establishes an IETF URN namespace for identifiers defined by
the IETF Secure Patterns for Internet CrEdentials (SPICE) working group.
The GLUE URN namespace is within the SPICE URN namespace.


--- middle

# Introduction

Enterprise entity identifiers are myriad. With the increasing use of digitial
credentials, there is a need for a common methodology for expressing these
identifiers such that claims about and by such entities can be made in a
consistent and interoperable manner.

This specification defines an IETF URN namespace that standardizes the expression of
existing organizational entity identifers by providing a common representation format.
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
: an organization that allocates External Numbers for GLUE URIs using the Authority Identifier(s) over which they have jurisdiction.

Authority Identifier:
: identifier for the External Authority responsible for assigning the External Number used in GLUE URIs.

External Number:
: number assigned by an External Authority to identify a particular organization within GLUE URNs over which it has jurisdiction.

# Core Concepts

Every GLUE URI, whether expressed as a string or encoded in binary MUST
contain the following components:

- The Authority Identifier

- The External Number

## Uniqueness and Namespacing

Each GLUE URI MUST be globally unique.
It is assumed that most registered organizational entity identification schemes
already handle any necessary namespacing as part of the
External Number. However, in the event that collisions are possible within the
set of possible external identifiers for an Authority Identifier scheme, then
further namespacing might be necessary at the GLUE URI level. Such namespacing
SHOULD be done on the Authority Identifier as part of the registration process.

That is, the different namespaces would be considered either different schemes
operated by the same authority, or the same scheme operated by different
authorities. In either case a unique Authority Identifier would be necessary for
each.

For example, assume there is an External Authority FEA that provides
identifiers for organizational entities in USA and Canada. The identifiers in the USA
are unique, and the identifiers in Canada are unique, but there is no guarantee
that a organizational entity in Canada won't be assigned the same number as a organizational
entity in the USA. Upon registration of FEA as an Authority Identifier, it would
be necessary to seperately register FEA-USA and FEA-Can to provide
differentiation between the two sets of External Numbers.

# Text Encoding of GLUE URIs {#text-encoding}

All GLUE URIs comply with [RFC3986].
They begin with `urn:ietf:spice:glue:` and are followed by an Authority Identifier,
a colon character (":"), and the External Number allocated by the authority.

Authority Identifiers consist of a sequence of characters beginning with a letter and
followed by any combination of letters, digits, plus ("+"), period ("."), or hyphen ("-").
Although Authority Identifiers are case-insensitive, the canonical form is lowercase
and documents that specify Authority Identifiers must do so with lowercase letters.
An implementation should accept uppercase letters as equivalent to lowercase
in Authority Identifier names (e.g., allow "EXAMPLE" as well as "example")
for the sake of robustness but should only produce
lowercase Authority Identifier names for consistency.
The ABNF [RFC5234] for Authority Identifiers is:

```
authority-identifier = ALPHA *( ALPHA / DIGIT / "+" / "-" / "." )
```

The external identifier is the decimal representation of the
integer External Number value with no extra leading zeros.
The ABNF [RFC5234] for external identifiers is:

```
external-identifier = ("-") DIGIT *DIGIT
```

Combining these, the ABNF [RFC5234] for a GLUE URI is:

```
glue-uri = "urn:ietf:spice:glue:" authority-identifier ":" external-number
```

For example, the following is a GLUE URI using the Authority Identifier "example"
and the External Number "42":

```
urn:ietf:spice:glue:example:42
```

The Authority Identifier MUST be registered in the GLUE URI Authority Identifier registry
defined in {{GLUE-URN}}.
The External Number MUST be the identifier assigned to the organization
by the External Authority.

# GLUE Authority Identifiers Defined

This specification defines the following GLUE Authority Identifiers.

| Organization     | Authority Identifier | Specification Document      |
|:-----------------|-:------|:------------------------------------------|
| GS1              |  gln   | https://www.gs1.org/standards/id-keys/gln |
| GLEIF            |  lei   | https://www.iso.org/standard/78829.html   |
| Dun & Bradstreet |  duns  | https://www.dnb.com/duns.html             |
| Private Enterprise Numbers | pen | https://www.iana.org/assignments/enterprise-numbers/ |

They are registered in the GLUE Authority Identifier URN Registry in {{GLUE-URN}}.

# Security Considerations

There are no additional security considerations beyond those already inherent to using URNs.
Security considerations for URNs can be found in [RFC2141].

# Privacy Considerations

## Private identifiers as corporate identifiers

There are some corporate identifers which make use of personal identifiers. This
is the case for registered sole-proprietor businesses in much of the United
States, where the business identifier may be the same as the
social-security-number of the business owner.

It is possible for such identifers to be represented as GLUE URIs. An
identifier's expression as a GLUE URI does not change the privacy
characteristics of that identifier. The same cautions and concerns need to be
taken with the GLUE URI representation as with the original identifier.

Implementers storing or evaluating GLUE URIs are encouraged to evaluate the
privacy characteristics of each identification scheme represented by an
Authority Identifier and to appropriately handle any GLUE URI which violates
privacy policies.

# IANA Considerations

This section establishes two registries and populates them with their initial contents.
The following registration procedure is used for the
registries established by this specification.

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
(e.g., "Request to register URN urn:ietf:spice:example" or
"Request to register URN urn:ietf:spice:glue:example").

Within the review period, the Designated Experts will either approve or deny
the registration request, communicating this decision to the review list and IANA.
Denials should include an explanation and, if applicable,
suggestions as to how to make the request successful.
The IANA escalation process is followed when the Designated Experts
are not responsive within 14 days.

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

## SPICE URN Registry {#SPICE-URN}

This specification establishes the
IANA "SPICE URN" registry
creating a URN namespace for identifiers needed by
the IETF Secure Patterns for Internet CrEdentials (SPICE) working group.
The registry records the URN
and a reference to the specification that defines it.

### Registration Template

URN:
: The URN requested within the "urn:ietf:spice:" namespace.
The identifier following "urn:ietf:spice:"
and before any following colon (":") character
is not case sensitive and any letters MUST be expressed in lowercase characters.
This identifier MUST consist of a sequence of characters
beginning with a letter and followed by any combination of
letters, digits, plus ("+"), period ("."), or hyphen ("-").

Description:
: Brief description of the purpose of the SPICE URN.

Change Controller:
: For IETF stream RFCs, use "IETF".
For others, give the name of the responsible party.
Other details (e.g., postal address, e-mail address, home page URI) may also be included.

Specification Document(s):
: Reference to the document or documents that specify the URN to be registered,
preferably including URLs that can be used to retrieve the documents.
An indication of the relevant sections may also be included, but is not required.

### Initial Registry Contents

* URN: urn:ietf:spice:glue
* Description: GLUE URN namespace
* Change Controller: IETF
* Specification Document(s): [[ this specification ]]

## GLUE Authority Identifier URN Registry {#GLUE-URN}

This specification establishes the
IANA "GLUE Authority Identifier URN" registry
creating a URN namespace for Authority Identifiers for
GLobal Unique Enterprise (GLUE) Identifiers.

Each entry registers the URN for an Authority Identifier within the
"urn:ietf:spice:glue:" namespace.
The organization responsible for the Authority Identifier is recorded.

### Registration Template

Authority Identifier:
: identifier for the External Authority responsible for assigning the External Number used in GLUE URIs.
This identifier
is not case sensitive and any letters MUST be expressed in lowercase characters.
It MUST consist of a sequence of characters
beginning with a letter and followed by any combination of
letters, digits, plus ("+"), period ("."), or hyphen ("-").

URN:
: The URN within the "urn:ietf:spice:glue:" namespace
consisting of "urn:ietf:spice:glue:" followed by
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
* URN: urn:ietf:spice:glue:gln
* Organization: GS1
* Change Controller: IETF
* Specification Document(s): https://www.gs1.org/standards/id-keys/gln

#### lei

* Authority Identifier: lei
* URN: urn:ietf:spice:glue:lei
* Organization: GLEIF
* Change Controller: IETF
* Specification Document(s): https://www.iso.org/standard/78829.html

#### duns

* Authority Identifier: duns
* URN: urn:ietf:spice:glue:duns
* Organization: Dun & Bradstreet
* Change Controller: IETF
* Specification Document(s): https://www.dnb.com/duns.html

#### pen

* Authority Identifier: pen
* URN: urn:ietf:spice:glue:pen
* Organization: Private Enterprise Numbers
* Change Controller: IETF
* Specification Document(s): https://www.iana.org/assignments/enterprise-numbers

--- back

# Acknowledgments
{:numbered="false"}

Martin Thomson and
Alexander (A.J.) Stein
contributed to this specification.

# Document History
{: numbered="false"}

-02

* Per working group feedback, use an IETF URN namespace for GLUE URIs.
* Also establish an IETF URN namespace for identifiers defined by the SPICE working group, which the GLUE namespace is within.
* Refer to "organizational identification schemes" rather than "company identification schemes".
* Added Security Considerations.
* Added Michael B. Jones as an author.

-01

* Added Uniqueness and Naming Section.
* Added Privacy Considerations.
* Added Pamela Dingle as an author.

-00

* Initial individual draft
