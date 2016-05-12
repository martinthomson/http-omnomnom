---
title: Expiring Aggressively Those HTTP Cookies
abbrev: EAT HTTP Cookies
docname: draft-thomson-http-omnomnom-latest
date: 2016
category: std

ipr: trust200902
area: General
workgroup:
keyword: Internet-Draft

stand_alone: yes
pi: [toc, tocindent, sortrefs, symrefs, strict, compact, comments, inline, docmapping]

author:
 -
    ins: M. Thomson
    name: Martin Thomson
    organization: Mozilla
    email: martin.thomson@gmail.com
 -
    ins: C. Peterson
    name: Chris Peterson
    organization: Mozilla
    email: cpeterson@mozilla.com

normative:
  RFC2119:
  RFC7230:
  RFC6265:

informative:
  RFC2818:
  RFC7258:

--- abstract

HTTP Cookies that are sent over connections without confidentiality and
integrity protection are vulnerable to theft.  Such cookies should be expired
aggressively.

--- middle

# Introduction

HTTP cookies [RFC6265] provide a means of persisting server state between
multiple requests.  This feature is widely used on both HTTP [RFC7230] and HTTPS
[RFC2818] requests.

The authority for "http://" resources (see Section 9.1 of [RFC7230]) derives
from insecure sources: notably the network and the DNS (absent DNSSEC).  This
situation might change over time.  As persistent state, cookies create a way for
an attacker to link requests.  The information that a cookie holds might also be
valuable to that attacker in some way.

To limit the effectiveness of attacks on cleartext communications [RFC7258],
user agents are encouraged to limit the persistence of cookies that are set
over insecure connections.


## Notational Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC2119].


# Expire Cookies

Cookies that are set using insecure channels (i.e., HTTP over cleartext TCP),
MUST be given shorter expiration times than the time requested.  For instance,
these cookies might only persist until the user closes their browser.

If a user agent detects a change in network conditions it SHOULD remove any
cookies that were established using insecure channels.

Alternatives:

* In the investigation into this change, it was suggested that cookies without
  the "Secure" flag might be given the same treatment.  However, this resulted
  in a far greater number of cookies being affected and some interoperability
  problems as a result.

* This change might be limited to cookies that are set in third-party contexts.
  See [I-D.west-first-party-cookies].


# Security Considerations

This document describes an improvement that could be a security improvement.
However, this is not without risks.  For cookies that are used as a substitute
for logins, more regular clearing of a login cookie could expose the primary
authentication token (for instance, a password) to more network attackers as a
result of being entered more often.

Clearing login tokens could also cause a degree of user annoyance, as login
information is lost.  Such annoyance manifests in many subtle ways.

Limiting the change to third-party contexts as suggested above might reduce
these risks, though with lesser overall impact.


# IANA Considerations

This document makes no request of IANA.


--- back

# Acknowledgements

Chris Peterson did much of the initial investigation and work for this.  See
<eref target="https://bugzilla.mozilla.org/show_bug.cgi?id=1160368"/> for details.
