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
 - Prefix
 - Registry
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
  RFC3986:
  RFC5234:

informative:
  RFC1034:
  RFC3061:
  RFC8126:
  RFC9371:
  RFC9562:
  RFC9839:
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
  WIKIDATA:
    title: "Wikidata:Identifiers"
    author:
      org: Wikimedia Foundation
    target: https://www.wikidata.org/w/index.php?title=Wikidata:Identifiers&oldid=2327606992

---

--- abstract

This specification defines GLobal Unique Enterprise (GLUE) Identifiers,
which are strings, some of which are also URIs, that identify businesses and organizations.
Every GLUE Identifier begins with a registered prefix that makes it globally unique.
The specification establishes an IANA registry of prefixes.
Registered prefixes can be the leading portion of identifiers from existing organizational identifier systems,
such as "urn:lei:",
or can be new simple string prefixes of the form "glue_name_".

--- middle

# Introduction

There are myriad entity identifier types for businesses and organizations.
With the increasing use of digital credentials, there is a need for a common
methodology for expressing these identifiers such that claims about and by such
entities can be made in a consistent and interoperable manner.

This specification defines GLUE Identifiers.
A GLUE Identifier is a string that begins with a Prefix registered in an IANA registry
established by this specification.
The Prefix makes the identifier globally unique.
Many existing organizational identifiers already have a globally unique form,
for example as a URN or as an HTTPS URL.
Such identifiers are used as they are, and the registry records
the leading portion of the identifier as a Prefix, together with a reference to
the specification that defines the identifiers.
This specification does not create new identifiers for these systems.
For identifier systems that have no such form,
and for new systems, the registry also allows Prefixes of the simple form "glue_name_".

## Requirements Notation and Conventions

{::boilerplate bcp14-tagged}

## Terminology

This specification uses the following terms:

GLUE Identifier:
: an identifier for an organization that is a string beginning with a registered Prefix.
It might also be a URI, URN, or IRI.

Prefix:
: the leading portion of a GLUE Identifier, registered in the registry established in {{GLUE-Prefix-Reg}}.
Each GLUE Identifier begins with exactly one registered Prefix.
A Prefix typically corresponds to an External Authority.

External Identifier:
: the remainder of a GLUE Identifier after its Prefix.
It is allocated by the External Authority to identify a particular organization
within the Prefix over which that authority has jurisdiction.

External Authority:
: an organization that allocates External Identifiers over which it has jurisdiction that are used in GLUE Identifiers with its Prefix(es).

# Core Concepts

Every GLUE Identifier is a string consisting of the following components, in order:

- A registered Prefix

- The External Identifier

This specification requires only that registered Prefixes are unique,
and it relies on the IANA registry to enforce this property.
This specification does not address the equivalence or uniqueness of the
full identifiers defined by the specification that is referenced
from the registration of a Prefix.

## Uniqueness and Namespacing

Each GLUE Identifier is a globally unique identifier.
An organization can be identified by multiple GLUE Identifiers,
but each distinct GLUE Identifier can only refer to a single organization.
If multiple External Authorities have issued identifiers for the same organization
(e.g., the organization has both a Dun & Bradstreet number and a Private Enterprise Number),
then the same organization can be identified by GLUE Identifiers differentiated by the Prefix.
Establishing whether multiple GLUE Identifiers
refer to the same organization is out of scope for this specification.

It is assumed that most registered organizational entity identification schemes
already handle any namespacing necessary to achieve uniqueness as part of the External Identifier.
However, if collisions are possible within the set of possible external
identifiers for an External Authority, then further namespacing is
necessary at the Prefix level.
Such namespacing could be done by registering multiple Prefixes.
The combination of the Prefix and the External Identifier
MUST result in a unique GLUE Identifier.

For example, assume there is an External Authority FEA that provides identifiers
for organizations in Singapore and South Korea. The identifiers issued in
Singapore are unique within Singapore, and the identifiers issued in South Korea
are unique within South Korea, but there is no guarantee that an organization in
Singapore will not be assigned the same identifier as an organization in South
Korea. Upon registration of FEA, it would be
necessary to separately register two different Prefixes (e.g.,
glue_fea-sg_ and glue_fea-kr_) to provide differentiation between the two sets of External
Identifiers.

## Lifecycle of Identifiers {#lifecycle}

Organizations merge, split, are acquired, are renamed, and are dissolved or go bankrupt,
and some identifier systems record relationships between organizations, such as
parent and subsidiary.
This specification does not define how identifiers behave through such events,
and it does not define relationships or aliases between organizations.
Each External Authority handles these in its own way,
and the resulting identifier behavior differs between systems.

Specifications that register Prefixes SHOULD make the following clear to their readers,
for the identifiers they reference:

- whether an External Identifier is permanent, or can change,
  for example when organizations merge or split;
- whether an External Identifier can ever be reused for a different organization,
  after its organization is dissolved or goes bankrupt, or for any other reason;
- how the status of an identifier is conveyed,
  such as an organization that is no longer active,
  and the longest time from the event (dissolution, merger, strike-off)
  to the change of that status; and
- whether and how the External Authority expresses relationships between organizations,
  or alternative identifiers for one organization, if it does.

Relying parties need this information to know whether a GLUE Identifier
can be assumed to refer to the same organization over time.
An identifier that can be reassigned to a different organization
weakens the global uniqueness that GLUE Identifiers are intended to provide.

# GLUE Identifiers {#glue-identifiers}

A GLUE Identifier is a string that begins with a Prefix registered
in the registry established in {{GLUE-Prefix-Reg}},
followed by the External Identifier allocated by the External Authority.
A registered Prefix is either a Prefix for an existing identifier system,
described in {{existing-prefixes}}, or a new "glue_" Prefix,
described in {{new-prefixes}}.

No registered Prefix may be a leading substring of another registered Prefix,
so that every GLUE Identifier begins with exactly one registered Prefix.
A Prefix is compared exactly as registered,
except where its registration states that comparison is case-insensitive,
as it is for the scheme and namespace of a URN.

## Prefixes for Existing Identifier Systems {#existing-prefixes}

Many organizational identifiers already have a globally unique form,
such as a URN or a URL.
The Prefix for such a system is the leading portion of the identifier,
up to the point where the External Identifier allocated by the External Authority begins.
Examples are "urn:lei:" and
"https://business-registry.example/companies/us_va/".
The Prefix MUST contain enough of the identifier to be globally unique
and to scope the External Identifiers that the External Authority allocates.
A URI scheme on its own, such as "https:", is not sufficient.

For these systems the GLUE Identifier is the existing identifier,
used as defined by the External Authority.
This specification does not define new identifiers for them,
and it does not restrict the characters of their External Identifiers.
The specification referenced by the registration defines the syntax of the External Identifiers.

## New Prefixes {#new-prefixes}

Where an identifier system has no existing globally unique identifier form,
a new Prefix of the form "glue_" name "_" can be registered.
The name consists of a sequence of characters beginning with a
letter or digit and followed by any combination of letters, digits,
hyphen ("-"), or period (".").
This repertoire is compatible with DNS names {{RFC1034}}.
The name MUST NOT contain an underscore,
so the Prefix ends at the first underscore that follows "glue_".
Although names are case-insensitive, the canonical form is
lowercase and documents that specify names must do so with
lowercase letters. An implementation should accept uppercase letters as
equivalent to lowercase in names (e.g., allow "EXAMPLE" as
well as "example") for the sake of robustness but should only produce lowercase
names for consistency. There is a limit of 253 characters
for the length of a name.
In the form "glue_" name "_", the strings "glue_" and "_" are literal,
and name is a placeholder for the registered name.
The ABNF [RFC5234] for new Prefixes is:

```
new-prefix = "glue_" name "_"
name = (ALPHA / DIGIT) *252( ALPHA / DIGIT / "-" / "." )
```

External Identifiers following a new Prefix consist of a sequence of unreserved characters
{{Section 2.3 of RFC3986}}: letters, digits,
hyphen ("-"), period ("."), underscore ("_"), and tilde ("~").
It is specific to the
Prefix whether the External Identifiers are case-insensitive or
case-sensitive. When they are case-insensitive, the canonical form is lowercase
and documents that specify External Identifiers MUST do so with lowercase letters.
Always using the canonical form of these identifiers means that code performing comparisons
need not be aware of whether External Identifiers are case-sensitive or not;
case-sensitive comparisons can always be performed
on the whole GLUE Identifier.

While the original representation of some External Identifiers may contain
characters that are ignored, these MUST be omitted when used in GLUE Identifiers
with a new Prefix.
For instance, Dun & Bradstreet {{DUNS}} identifiers sometimes contain
hyphen characters that are ignored.
The DUNS numbers 12-345-6789 and 123456789 are considered to be equivalent.
The latter form without the ignored characters MUST be used
as the External Identifier in GLUE Identifiers.

Documents that define a new Prefix
MUST NOT allow the representation of an External Identifier
to contain characters outside the set above, such as the colon character.
Specifications MUST define a substitute character,
such as period, that is used in place of any such character in External Identifiers.
Any substitution can be specified in the Transformation Rules
in the IANA registration for the Prefix; see {{GLUE-Prefix-Reg}}.

GLUE Identifiers with a new Prefix are defined over the restricted US-ASCII syntax specified in this
section. Percent-encoding is not permitted. Consequently, such GLUE Identifiers do not
support representation of External Identifiers that use
non-ASCII characters. New Prefixes are therefore limited to identifier
systems whose representations can be expressed fully within the permitted character set.
This character set is a subset of the Unicode Assignables subset defined in {{RFC9839}}.

There is a limit of 1000 characters for an External Identifier following a new Prefix.
The ABNF [RFC5234] for such GLUE Identifiers is:

```
glue-identifier = new-prefix external-identifier
external-identifier = 1*1000( ALPHA / DIGIT / "-" / "." / "_" / "~" )
```

For example, the following is a GLUE Identifier using the Prefix "glue_duns_"
and the External Identifier "123456789".

```
glue_duns_123456789
```

### Recommendations for New Prefixes {#recommendations}

The following recommendations apply to documents that register new Prefixes.

- Consider using a DNS name {{RFC1034}} under the control of the External Authority
  as the name (e.g., "business-registry.example"), because DNS names are already globally
  unique and have an established allocation and ownership model.
  A DNS-name Prefix also helps limit conflicts between registrations.

- Consider using a UUID {{RFC9562}} in its lowercase hexadecimal string form
  as the External Identifier,
  because a UUID can be generated without coordination and is URL-safe.
  For example, "glue_business-registry.example_3f2504e0-4f89-41d3-9a0c-0305e82c3301".

- Restrict both the name and the External Identifier to URL-safe characters,
  as defined by this section.

# Initial GLUE Prefixes {#prefixes}

This section defines the initial GLUE Prefixes listed in {{glue-def}}.
These registrations are normative.
Prefixes for existing identifier systems that already have a globally unique form
are registered as the leading portion of that form.

| Organization     | Prefix                           |
|:-----------------|:---------------------------------|
| GLEIF            | urn:lei:                         |
| IANA (PEN)       | urn:oid:1.3.6.1.4.1.             |
| Wikidata         | https://www.wikidata.org/entity/ |
| Dun & Bradstreet | glue_duns_                       |
| ISO/IEC 6523     | glue_iso6523_                    |
{: #glue-def title='Defined GLUE Prefixes'}

These are registered in the GLUE Prefix Registry
established in {{GLUE-Prefix-Reg}}.
Each registration includes an illustrative example identifier,
which is not normative.
The LEI Code in the first example is the one given as an example
in the LEI URN namespace registration {{LEI-IANA}};
no LEI Code is reserved for use in examples.
The second example uses the Enterprise Number "32473" reserved for documentation in {{?RFC5612}}.

The Prefix "urn:oid:1.3.6.1.4.1." is the OID arc
iso.org.dod.internet.private.enterprise (1.3.6.1.4.1),
under which IANA allocates Private Enterprise Numbers (PEN) [PEN]
to organizations {{RFC9371}}.
Its URN form is defined in {{RFC3061}}.

## Related Identifiers {#related}

The specification that registers a Prefix can mention related identifiers,
including URIs, URNs, IRIs and other strings.
Establishing equivalence between those identifiers,
or whether multiple identifiers refer to the same organization,
is out of scope for this specification.
Implementations and relying parties MUST NOT assume equivalence between
GLUE Identifiers and other identifiers
unless such equivalence is explicitly defined by the specification
referenced from the registration of the Prefix.

# Security Considerations

The security considerations inherent to using URIs apply to GLUE Identifiers that are URIs.
Security considerations for URIs can be found in {{RFC3986}}.

The global uniqueness of GLUE Identifiers prevents situations in which
the same identifier is allocated in different local namespaces
and cannot be disambiguated when used.
For instance both Canadian and Singaporean organization registries might use
the local identifier "42", but with these referring to different organizations.
Embedding these local identifiers in GLUE Identifiers with distinct Prefixes enables disambiguation.

A Prefix identifies the namespace, not the party presenting an identifier.
A GLUE Identifier on its own does not authenticate that the presenter is the organization it names.
If needed, relying parties need to establish that by other means.

A DNS name used in a Prefix can change hands when the domain registration lapses or is transferred.
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
of Prefixes for
GLobal Unique Enterprise (GLUE) Identifiers.

Each entry registers a Prefix for use in GLUE Identifiers.
The organization responsible for the Prefix is recorded.
A registered Prefix is either the leading portion of an identifier
in an existing organizational identifier system (such as "urn:lei:"),
or a new Prefix of the form "glue_name_" as defined in {{new-prefixes}}.

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

As is done for media type registrations,
registration requests are sent to the review mailing list,
and the review takes place on that list.
Requests should use an appropriate subject
(e.g., "Request to register GLUE Prefix example").

During the review period, the Designated Experts provide their feedback on the review list,
and the applicant responds there.
This keeps the review a public discussion
among the applicant, the Designated Experts, and other interested parties,
rather than a discussion only between the applicant, the Designated Experts, and IANA.
Within the review period, the Designated Experts will either approve or deny
the registration request, communicating this decision to the review list and IANA.
The Designated Experts verify that a specification exists
and that the proposed Prefix is unique,
is not a leading substring of a registered Prefix,
and does not have a registered Prefix as a leading substring.
A new Prefix also has to conform to the syntax in {{new-prefixes}}. Experts are
encouraged to be biased towards approving registrations unless they are abusive,
frivolous, or actively harmful (not merely aesthetically displeasing or
architecturally dubious).

Denials should include an explanation and, if applicable,
suggestions as to how to make the request successful.
If the Designated Experts are not responsive,
the registration requesters can raise this on the review list
and with the IESG, which appoints the Designated Experts.

Criteria that should be applied by the Designated Experts includes
determining whether the proposed registration duplicates existing functionality,
determining whether it is likely to be of general applicability
or whether it is useful only for a single application,
and whether the Prefix is distinct enough to avoid confusion
with an existing registration.
For a Prefix for an existing identifier system,
the Designated Experts check that the registration references
the specification of those identifiers, operated by an External Authority,
and that the Prefix contains enough of the identifier to be globally unique,
as described in {{existing-prefixes}}.
Equivalent forms of an identifier system that already have a registered Prefix
should not be registered under a new Prefix;
the existing identifiers are used instead.
Where a Prefix contains a DNS name, the Designated Experts should
check that the requester controls that DNS name.
For a new Prefix that is not tied to an existing identifier system,
the specification needs to describe how External Identifiers are allocated
and how their uniqueness within the Prefix is ensured.
The Designated Experts should also check that the specification
addresses the topics in {{lifecycle}}.
The recommendations in {{recommendations}} apply.

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
: The Prefix to be registered.
For a Prefix for an existing identifier system,
it is the leading portion of the identifier, as described in {{existing-prefixes}}.
For a new Prefix, it MUST conform to the "new-prefix" syntax in {{new-prefixes}}
and any letters MUST be expressed in lowercase characters.
The registration states whether comparison of the Prefix is case-insensitive.

Example:
: An example GLUE Identifier using the Prefix.

Organization:
: The organization responsible for the Prefix.

Transformation Rules:
: Syntactic transformations applied to the original External Identifiers when creating the GLUE Identifier, or "N/A" if none.

Change Controller:
: For IETF stream RFCs, use "IETF".
For others, give the name of the responsible party.
Other details (e.g., postal address, e-mail address, or home page URL) may also be included.

Specification Document(s):
: Reference to the document or documents that specify the identifiers for the Prefix to be registered,
preferably including URLs that can be used to retrieve the documents.
An indication of the relevant sections may also be included, but is not required.

### Initial Registry Contents

#### urn:lei:

{:compact}
Prefix:
: urn:lei: (case-insensitive)

Example:
: urn:lei:7ltwfzyicnsx8d621k86

Organization:
: GLEIF

Transformation Rules:
: N/A

Change Controller:
: IETF

Specification Document(s):
: {{prefixes}} of \[\[ this specification ]], [LEI], [LEI-IANA]


#### urn:oid:1.3.6.1.4.1.

{:compact}
Prefix:
: urn:oid:1.3.6.1.4.1. (case-insensitive)

Example:
: urn:oid:1.3.6.1.4.1.32473

Organization:
: Private Enterprise Numbers

Transformation Rules:
: N/A

Change Controller:
: IETF

Specification Document(s):
: {{prefixes}} of \[\[ this specification ]], [PEN], [RFC9371], [RFC3061]


#### https://www.wikidata.org/entity/

{:compact}
Prefix:
: https://www.wikidata.org/entity/ (scheme and host are case-insensitive)

Example:
: https://www.wikidata.org/entity/Q95

Organization:
: Wikimedia Foundation

Transformation Rules:
: N/A

Change Controller:
: IETF

Specification Document(s):
: {{prefixes}} of \[\[ this specification ]], [WIKIDATA]


#### glue_duns_

{:compact}
Prefix:
: glue_duns_

Example:
: glue_duns_123456789

Organization:
: Dun & Bradstreet

Transformation Rules:
: Delete hyphen characters.

Change Controller:
: IETF

Specification Document(s):
: {{prefixes}} of \[\[ this specification ]], [DUNS]


#### glue_iso6523_

{:compact}
Prefix:
: glue_iso6523_

Example:
: glue_iso6523_0060.123456789

Organization:
: ISO/IEC 6523

Transformation Rules:
: Substitute period for any colon characters.

Change Controller:
: IETF

Specification Document(s):
: {{prefixes}} of \[\[ this specification ]], [ISO6523]


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

-11

* Defined GLUE Identifiers as strings that begin with a registered Prefix,
  which makes them globally unique.
* Prefixes for existing identifier systems are the leading portion of the existing identifier,
  such as "urn:lei:", and no new identifiers are defined for them.
* Added new Prefixes of the form "glue_name_" for identifier systems without an existing globally unique form.
* Removed the "glue:" URI scheme and its registration.
* Renamed "Authority Identifier" to "Prefix".
* Added recommendations for new Prefixes:
  DNS names, UUIDs, and URL-safe characters.
* Registered the Wikidata entity URI prefix as an example of a general-purpose identifier.
* Encouraged specifications that register Prefixes to state how identifiers behave
  through mergers, dissolution, reuse, and relationships between organizations.
* Adopted a media-type-style review process in which Designated Experts
  give feedback on the review list, and replaced the IANA escalation text.
* Stated that equivalence between identifiers, and establishing whether multiple identifiers
  refer to the same organization, are out of scope.
* Replaced the example LEI, and corrected "20-digit" to "20-character".
* Updated the D-U-N-S reference.

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
