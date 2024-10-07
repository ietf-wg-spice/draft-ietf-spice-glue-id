---
title: "SPICE GLUE: GLobal Unique Enterprise Identifiers"
category: info

docname: draft-zundel-spice-glue-id-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Security"
workgroup: "Secure Patterns for Internet CrEdentials"
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
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

 -
    fullname: Pamela Dingle
    organization: Microsoft Corporation
    email: pamela.dingle@microsoft.com

normative:
  RFC3986: URI-syntax
  RFC8126: IANA-guidelines

informative:


--- abstract

This specification defines the `glue` URI scheme and the rules for encoding
these URIs. It also establishes the registries necessary for management of this
scheme.


--- middle

# Introduction

Enterprise entity identifiers are myriad. With the increasing use of digitial
credentials, there is a need for a common methodology for expressing these
identifiers such that claims about and by such entities can be made in a
consistent and interoperable manner.

This specification defines a URI scheme that standardizes the expression of
existing company entity identifers and establishes a registry for managing how
these existing identifiers relate to this scheme.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

For the remainder of this document, the term "glue URI" is used to refer to a
URI that uses the glue scheme.

TODO: define external authority, company entity

# Core Concepts

Every glue URI, whether expressed as a string or encoded in binary MUST be
comprised of the following components:

- The Authority Identifier

- The External Number

The Authority Identifier indicates the external authority responsible for
assigning the External Number and the scheme used to do so.

The External Number is the identifier assigned to the company by the external
authority.

## Uniqueness and Namespacing

Each glue URI MUST be globally unique. It is assumed that most registered company entity
identification schemes already handle any necessary namespacing as part of the
external number. However, in the event that collisions are possible within the
set of possible external identifiers for an authority identifier scheme, then
further namespacing might be necessary at the glue id level. Such namespacing
SHOULD be done on the authority identifier as part of the registration process.

That is, the different namespaces would be considered either different schemes
operated by the same authority, or the same scheme operated by different
authorities. In either case a unique authority identifier would be necessary for
each.

For example, assume there is an external authority FEA that provides
identifiers for company entities in USA and Canada. The identifiers in the USA
are unique, and the identifiers in Canada are unique, but there is no guarantee
that a company entity in Canada won't be assigned the same number as a company
entity in the USA. Upon registration of FEA as an Authority Identifier, it would
be necessary to seperately register FEA-USA and FEA-Can to provide
differentiation between the two sets of external numbers.

# Text Encoding of glue URIs

All glue URIs comply with {{-URI-syntax}} and are therefore represented by a
scheme identifier and a scheme-specific part. The scheme identifier is: glue, and
the scheme-specific parts are represented as a sequence of alphanumeric
components separated by the '.' character. A formal definition is provided in
the next section, but it can informally be considered as:

glue:&lt;authority-identifier>.&lt;external-number>

The Authority Identifier MUST be an alphanumeric string from the "Scheme" field
of the glue URI Authority Identifier registry. The External Number MUST be the
identifier assigned to the company by the external authority under the
identified scheme.

## glue URI Scheme Text Syntax

TODO ABNF

# DIDs and glue

TODO DIDs and glue

# Security Considerations

TODO Security

# Privacy Considerations

## Private identifiers as corporate identifiers

There are some corporate identifers which make use of personal identifiers. This
is the case for registered sole-proprietor businesses in much of the United
States, where the business identifier may be the same as the
social-security-number of the business owner.

It is possible for such identifers to be represented as glue URIs. An
identifier's expression as a glue URI does not change the privacy
characteristics of that identifier. The same cautions and concerns need to be
taken with the glue URI representation as with the original identifier.

Implementers storing or evaluating glue ids are encouraged to evaluate the
privacy characteristics of each identification scheme represented by an
authority identifier and to appropriately handle any glue id which violates
privacy policies.

# IANA Considerations

The following sections detail requests to IANA for the creation of a new
registry and registration of the 'glue' URI scheme.

## 'glue' URI Scheme Registration

TODO

## 'glue' Scheme URI authority registry

IANA is requested to create a new registry entitled "'glue' Scheme URI Authority
Identifiers". The registration policy for this registry is Expert Review as
defined in {{-IANA-guidelines}}.

Each entry in this registry associates one or more Authority Identifiers with a
single organization. Within the registry, the organization is identified using
the "Name" field. Each identified organization will be associated with one or
more number of company identification schemes, which are listed in the "Scheme"
field. A reference for each scheme is listed in the "Reference" field of the
registry.

The initial values for the registry are:

~~~ aasvg

+==================+========+===========================================+
| Name             | Scheme | Reference                                 |
|                  |        |                                           |
+==================+========+===========================================+
| GS1              |  gln   | https://www.gs1.org/standards/id-keys/gln |
|                  |        |                                           |
+------------------+--------+-------------------------------------------+
| GLEIF            |  lei   | https://www.iso.org/standard/78829.html   |
|                  |        |                                           |
+------------------+--------+-------------------------------------------+
| Dun & Bradstreet |  duns  | https://www.dnb.com/duns.html             |
|                  |        |                                           |
+------------------+--------+-------------------------------------------+
| Private Enterprise Numbers | pen | https://www.iana.org/assignments/enterprise-numbers/ |
|                            |     |                                                      |
+----------------------------+-----+------------------------------------------------------+

~~~
### Guidance for Designated Experts

It is not required that registration of an Authority Identifier be done by a
representative of the external authority.

## URI Scheme Registration
The "glue" URI scheme is requested to be registered in the provisional "URI Schemes" registry. The information below is provided according to the guidelines from RFC 4395:
  * URI scheme name: glue
  * Status: provisional
  * URI scheme syntax: See [Section 4.1 of Text Encoding of glue URIs](#text-encoding-of-glue-uris)

--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
