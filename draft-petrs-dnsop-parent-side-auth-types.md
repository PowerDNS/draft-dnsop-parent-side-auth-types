%%%
title = "Parent-side authoritative DNS records for enhanced delegation"
abbrev = "parent-side-auth-types"
docName = "draft-petrs-dnsop-parent-side-auth-types"
category = "std"

ipr = "trust200902"
area = "General"
workgroup = "dnsop"
keyword = ["Internet-Draft"]

[seriesInfo]
name = "Internet-Draft"
value = "draft-petrs-dnsop-parent-side-auth-types"
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
organization = "CZNIC"
[author.address]
 email = "petr.spacek@nic.cz"
[author.address.postal]
 city = "Prague"
 country = "Czech Republic"

%%%

.# Abstract

We propose reserving a DNS RRtype numeric range that behaves like DS.
This means: being authoritative on the parent side of a delegation; being signed by the parent; being provided along with delegations by the parent.
If this document had become an RFC five years ago, deploying new types (along the lines of NS2/NS2T or various other imagined things) would be easier today.

{mainmatter}

# Introduction


# Document work

This document lives [on GitHub](https://github.com/PowerDNS/FIXME); proposed text and editorial changes are very much welcomed there, but any functional changes should always first be discussed on the IETF DNSOP WG mailing list.

# Conventions and Definitions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in BCP 14 [@!RFC2119] [@RFC8174] when, and only when, they appear in all capitals, as shown here.


# Summary



# Implementation

The subsection titles in this section attempt to follow the terminology from [@RFC8499] in as far as it has suitable terms.
'Implementation' is understood to mean both 'code changes' and 'operational changes' here.

## Authoritative server changes

This specification defines changes to query processing in authoritative servers.

FIXME

## Validating resolver changes


## Stub resolver changes

## Zone validator changes


## Domain registry changes


# Security Considerations


# Implementation Status

[RFC Editor: please remove this section before publication]


# IANA Considerations

FIXME: this bit will be hard.

# Acknowledgements

The authors would like to thank the following individuals for their useful input:

{backmatter}

# Document history

