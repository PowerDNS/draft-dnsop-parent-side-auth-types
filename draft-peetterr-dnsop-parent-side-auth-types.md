%%%
title = "Parent-side authoritative DNS records for enhanced delegation"
abbrev = "parent-side-auth-types"
docName = "draft-peetterr-dnsop-parent-side-auth-types-01"
category = "std"
updates = [1035, 4035, 5155, 6840]

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

DNS RR types with number in range 0xFA00-0xFDFF are now included in special treatment like DS RR type specified in [@!RFC4035].
Authoritative servers, DNSSEC signers, and recursive resolvers need to extend condition for DS special cases to also include this range.
DNSSEC validators also need to implement downgrade protection described in (#downgrade).
This means: being authoritative on the parent side of a delegation; being signed by the parent; being provided along with delegations by the parent.

{mainmatter}

# Introduction

[@!RFC4035] defines the DS Resource Record, as a type with the special property that it lives at the parent side of a delegation, unlike any other record (if we can briefly ignore NSEC living on both sides of a delegation as an extra special case).
In various conversations and posted drafts over the last five years in DPRIVE, DNSOP, and DELEG, a potential desire to publish other kinds of data parent-side has been identified.
Some drafts simply proposed a new type, assuming that authoritative DNS servers and registry operations would eventually follow along; other drafts have tried to shoehorn new kinds of data into the DS record.
If, when DS was defined, or at any time since then, a range of RRtype numbers would have been specified to have the same behaviour as DS, those drafts, and the experiments that need to go with figuring out the exact definition of a protocol, would have been much more feasible.
This document requests that IANA reserve such a range and defines behavior of DNS implementations.

# Document work

This document lives [on GitHub](https://github.com/PowerDNS/draft-dnsop-parent-side-auth-types); proposed text and editorial changes are very much welcomed there, but any functional changes should always first be discussed on the IETF DNSOP WG mailing list.

# Conventions and Definitions {#term}

Term "Parent-Side Types" refers to set of RR types which contains exactly:

* Value 43 as defined for DS type by [@RFC4034, section 5],
* The whole range reserved by this document in (#iana).

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in BCP 14 [@!RFC2119] [@RFC8174] when, and only when, they appear in all capitals, as shown here.

# Summary

A RR type range reservation in [#iana] is now open for allocation of future Parent-Side Types, but none such types are allocated by this document.

Special processing for DS record type related to its inclusion in DNS messages, zone files, DNSSEC signing, etc. are extended to all Parent-Side Types. The only exception are rules which pertain to content of DS resource records, which are not applicable to other Parent-Side Types.

In short, authoritative servers serve the types from the parent side of a delegation and resolvers know to ask the parent side of a delegation.

Having these type numbers reserved with defined processing rules allows for future extension of parent-side publication of data, without having to wait for implementations to catch up.

# Implementation

'Implementation' is understood to mean both 'code changes' and 'operational changes' here.

This specification extends special handling previously defined exclusively to
DS RR type to apply to all Parent-Side Types. In short, implementations
need to modify their hardcoded condition (if RRTYPE equals DS) to (if RRTYPE
is Parent-Side Type) as defined in (#term).

## Authoritative servers, Signing software, and Recursive resolvers

Updates to existing specifications:

* [@RFC4035, section 2.4]
  * All Parent-Side Types in a zone MUST be signed, and MUST NOT appear at a zone's apex.
  * Unless specified otherwise by a future specification, only DS RR type defined in [@RFC4034, section 5] establish authentication chains between DNS zones.
  * TTL of Parent-Side Types is NOT tied to NS TTL.

* [@RFC4035, section 2.6]: All Parent-Side Types are allowed at the parent side of a zone cut, and NSEC RR type continues to be allowed as well.

* [@RFC4035, section 3.1.4]: Fully applicable. When responding to a query that has the DO bit set, all Parent-Side Types or a relevant proof-of-nonexistence MUST be returned. In practice it is extremely unlikely that all Parent-Side Types would be present and thus the proof-of-nonexistence will be always present.

* [@RFC4035, section 3.1.4.1]: Special rules applicable. When responding to a query that has the DO bit set, all Parent-Side Types or a relevant proof-of-nonexistence MUST be returned. In practice it is extremely unlikely that all Parent-Side Types would be present and thus the proof-of-nonexistence will be always present.

* [@RFC5155, section 7.2]: References to DS RR type are replaced by All Parent-Side Types.

* [@RFC5155]: Opt-out feature of NSEC3 applies only if no Parent-Side Type is present at the delegation point.


FIXME

How to deal with these places which were not updated by the RFC4033-4035? Close eyes and pretend we also forgot?

* rfc2181#section-6.2
* rfc2181#section-6.1
* rfc2181#section-5.4.1

* rfc1034#section-4.3.2 / rfc6672#section-3.2



## Validating resolver changes
* [@RFC5155, section 6]
  * Security status of the child zone is determined by the presence or absence of DS RRSet, which is not changed by this document.
  * All Parent-Side Types require proof-of-nonexistence and thus NSEC3 Opt-out feature applies only if no Parent-Side Type is present at the delegation point.

* [@RFC5155, section 8]: References to DS RR type are replaced by All Parent-Side Types.


* [@RFC6840, section 4.1]: Reference to DS RR type is replaced by all Parent-Side Types.

* [@RFC6840, section 4.4]:
  * While proving existance of any Parent-Side Type needs MUST follow rules for NSEC bitmap checks from this section to detect spoofed proofs from the child zone.
  * Only DS RR type is used for determining presence of a secure delegation.

This specification defines changes to query processing in resolvers.

FIXME DNSKEY flag for downgrade resistance

### Preventing downgrade attacks

A flag in the DNSKEY record signing the delegation is used as a backwards compatible, secure signal to indicate to a resolver that DELEG records are present or that there is an authenticated denial of a DELEG record. Legacy resolvers will ignore this flag and use the DNSKEY as is.

Without this secure signal an on-path adversary can remove DELEG records and its RRsig from a response and effectively downgrade this to a legacy DNSSEC signed response.

## Zone validator changes {#downgrade}

This specification defines changes to zone validation in zone validators.

RR types from the new parent-side range in (#iana) must conform to the same signing rules as DS RR type. See [@RFC4035]




## Stub resolver changes

This specification defines no changes to query processing in resolvers.

FIXME


## Domain registry changes

Domain registries MAY decide to allow children to publish records of any type from the range defined in this document in the parent zone.
Alternatively, they MAY decide to only allow such publication for types that actually get allocated a name and a semantic.
Ideally, domain registries would allow anything in the experimental subrange.

# Security Considerations



# Implementation Status

[RFC Editor: please remove this section before publication]


# IANA Considerations {#iana}

IANA is requested to change reservations in the DNS Parameters RR TYPEs registry, with this document as the Reference.

* Range 0xFA00-0xFDFF to Registration Procedure "Expert Review or Standards Action"
* Range 0xFE00-0xFEFF to Registration Procedure "Private Use"

IANA is further requested to assign bit TBD (suggested value: 9) to "Must sign parent types (MSPT)" in the DNSKEY RR Flags registry, with this document as the Reference.

# Acknowledgements

This idea was initially proposed by Petr Spacek.
His contribution is rewarded by listing him as an author so he can take equal parts credit and blame.

{backmatter}

# Document history

* 01
  * Specific range of type numbers (subset of former "Reserved for future use") was added to IANA considerations.
