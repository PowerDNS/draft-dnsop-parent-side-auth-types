%%%
title = "Parent-side authoritative DNS records for enhanced delegation"
abbrev = "parent-side-auth-types"
docName = "draft-peetterr-dnsop-parent-side-auth-types-01"
category = "std"

ipr = "trust200902"
area = "General"
workgroup = "dnsop"
keyword = ["Internet-Draft"]

[seriesInfo]
name = "Internet-Draft"
value = "draft-peetterr-dnsop-parent-side-auth-types-01"
stream = "IETF"
status = "standard"

[pi]
toc = "yes"

[[author]]
initials = "P."
surname = "van Dijk"
fullname = "Peter van Dijk"
organization = "PowerDNS"
[author.address]
 email = "peter.van.dijk@powerdns.com"
[author.address.postal]
 city = "Den Haag"
 country = "Netherlands"

[[author]]
initials = "P."
surname = "Spacek"
fullname = "Petr Spacek"
organization = "ISC"
[author.address]
 email = "pspacek@isc.org"
[author.address.postal]
 city = "Prague"
 country = "Czech Republic"

%%%

.# Abstract

A DNS RRtype numeric range that behaves like DS is reserved.
This means: being authoritative on the parent side of a delegation; being signed by the parent; being provided along with delegations by the parent.
If this document had become an RFC five years ago, deploying new types (such as DELEG) would be easier to deploy and experiment with today.

{mainmatter}

# Introduction

[@!RFC4035] defines the DS Resource Record, as a type with the special property that it lives at the parent side of a delegation, unlike any other record (if we can briefly ignore NSEC living on both sides of a delegation as an extra special case).
In various conversations and posted drafts over the last five years in DPRIVE, DNSOP, and DELEG, a potential desire to publish other kinds of data parent-side has been identified.
Some drafts simply proposed a new type, assuming that authoritative DNS servers and registry operations would eventually follow along; other drafts have tried to shoehorn new kinds of data into the DS record.
If, when DS was defined, or at any time since then, a range of RRtype numbers would have been specified to have the same behaviour as DS, those drafts, and the experiments that need to go with figuring out the exact definition of a protocol, would have been much more feasible.
This document requests that IANA allocate such a range.

# Document work

This document lives [on GitHub](https://github.com/PowerDNS/draft-dnsop-parent-side-auth-types); proposed text and editorial changes are very much welcomed there, but any functional changes should always first be discussed on the IETF DNSOP WG mailing list.

# Conventions and Definitions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in BCP 14 [@!RFC2119] [@RFC8174] when, and only when, they appear in all capitals, as shown here.

# Summary

A range of new types is allocated, but not assigned (FIXME: wording?).
This range of types is defined to be handled by DNS software like the DS record is handled.
Authoritative servers serve the types from the parent side of a delegation.
Resolvers know to ask the parent side of a delegation.

No semantics are assigned to the numbers at this time.
Having these numbers reserved with these processing rules allows for future extension of parent-side publication of data on behalf of a child, without having to wait for implementations to catch up.

# Implementation

The subsection titles in this section attempt to follow the terminology from [@RFC8499] in as far as it has suitable terms.
'Implementation' is understood to mean both 'code changes' and 'operational changes' here.

## Authoritative server changes

This specification defines changes to query processing in authoritative servers.

FIXME

## Validating resolver changes

This specification defines changes to query processing in resolvers.

FIXME

## Stub resolver changes

This specification defines no changes to query processing in resolvers.

FIXME

## Zone validator changes

This specification defines changes to zone validation in zone validators.

FIXME

## Domain registry changes

Domain registries MAY decide to allow children to publish records of any type from the range defined in this document in the parent zone.
Alternatively, they MAY decide to only allow such publication for types that actually get allocated a name and a semantic.
Ideally, domain registries would allow anything in the experimental subrange.

# Security Considerations



# Implementation Status

[RFC Editor: please remove this section before publication]


# IANA Considerations

IANA is requested to reserve a range of numbers in the Domain Name System (DNS) Parameters Resource Record (RR) TYPEs, with this document as the Reference.
The numbers shall get no meaningful names (but perhaps they would get some useful mnemonic, a weak proposal is `PA00` through `PAXX` for 'parent authoritive').
IANA should expect to eventually receive requests to assign meaningful names to some of these numbers.

IANA is also requested to mark a subset of that range as 'experimental'.
The experimental numbers are expected to never be hardcoded in published, released software, and no further allocation or naming of the experimental numbers by an RFC or otherwise is expected.

# Acknowledgements

This idea was initially proposed by Petr Spacek.
His contribution is rewarded by listing him as an author so he can take equal parts credit and blame.

{backmatter}

# Document history

