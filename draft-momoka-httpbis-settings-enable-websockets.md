---
title: "Define SETTINGS_ENABLE_WEBSOCKETS for HTTP/2 & HTTP/3"
abbrev: SETTINGS_ENABLE_WEBSOCKETS
docname: draft-momoka-httpbis-settings-enable-websockets-latest
category: std
updates: 8441, 9220

ipr: trust200902
area: Applications and Real-Time
workgroup: HTTP
keyword:
  - WebSockets
submissionType:
  IETF
stand_alone: yes
smart_quotes: no
pi: [toc, sortrefs, symrefs]

author:
 -
    fullname: 山本 桃歌
    ascii: Momoka Yamamoto
    asciiFullname: Momoka Yamamoto
    organization: The University of Tokyo/WIDE Project
    email: momoka.my6@gmail.com
    country: Japan



normative:




informative:




--- abstract

It would be nice if the server sends a SETTINGS parameter indicating support for websockets over h2/h3.


--- middle

# Introduction


Why we need it


# SETTINGS_ENABLE_WEBSOCKETS for HTTP/2
clients will love the information


# SETTINGS_ENABLE_WEBSOCKETS for HTTP/3


# Deployment Notes
TODO


# Security Considerations

This document introduces no new security considerations beyond those
discussed in {{!RFC8441}}, and {{!RFC9220}}.

# IANA Considerations

Ask IANA for this after discussion.

This document registers a new setting in the "HTTP/3 Settings"
registry (Section 11.2.2 of {{?HTTP3=RFC9114}}).

Value:  something
Setting Name:  SETTINGS_ENABLE_WEBSOCKETS
Default:  0
Status:  permanent
Specification:  This document
Change Controller:  IETF
Contact:  HTTP Working Group (ietf-http-wg@w3.org)



This document registers an entry in the "HTTP/2 Settings" registry
that was established by Section 11.3 of {{?HTTP2=rfc9113}}.

Code: something
Name: SETTINGS_ENABLE_ENABLE_WEBSOCKETS
Default:  0
Status:  permanent
Specification: This document
Change Controller:  IETF
Contact:  HTTP Working Group (ietf-http-wg@w3.org)
# Implementation Status

TODO:

--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge people.

Thank you for reading this draft.
