---
title: "SETTINGS_ENABLE_WEBSOCKETS settings parameter for HTTP/2 and HTTP/3"
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
  HTTP:
    title: "HTTP Semantics"
    date: 2022-04
    seriesinfo:
      RFC: 9110
      DOI: 10.17487/RFC9110
    author:
      -
          ins: R. Fielding
          name: Roy T. Fielding
          org: Adobe
          role: editor
      -
          ins: M. Nottingham
          name: Mark Nottingham
          org: Fastly
          role: editor
      -
          ins: J. Reschke
          name: Julian Reschke
          org: greenbytes
          role: editor

  HTTP2:
    display: HTTP/2
    title: "HTTP/2"
    date: 2022-04
    seriesinfo:
      RFC: 9113
      DOI: 10.17487/RFC9113
    author:
      -
          fullname: Martin Thomson
          org: Mozilla
          role: editor
      -
          fullname: Cory Benfield
          org: Apple Inc.
          role: editor

  HTTP3:
    display: HTTP/3
    title: "Hypertext Transfer Protocol Version 3 (HTTP/3)"
    date: 2022-04
    seriesinfo:
      RFC: 9114
      DOI: 10.17487/RFC9114
    author:
      -
          ins: M. Bishop
          name: Mike Bishop
          org: Akamai
          role: editor



informative:




--- abstract

This document proposes a new HTTP settings parameter, SETTINGS_ENABLE_WEBSOCKETS.
This parameter indicates whether the server supports bootstrapping WebSockets over the established connection.


--- middle

# Introduction

The mechanisms for running the WebSocket protocol {{!RFC6455}} over a single stream of an HTTP/2 and HTTP/3 connection is defined in {{!RFC8441}} and {{!RFC9220}}.
The extended CONNECT mechanism is used for bootstrapping WebSockets from HTTP/2 and HTTP/3.
Support for the extended CONNECT mechanism is advertised using HTTP/2 and HTTP/3 settings parameter SETTINGS_ENABLE_CONNECT_PROTOCOL.

However, the support of extended CONNECT does not necessarily indicate support for WebSockets over that HTTP connection.
Other protocols such as {{?WEBTRANSPORT=I-D.draft-ietf-webtrans-overview}} also use extended CONNECT and send SETTINGS_ENABLE_CONNECT_PROTOCOL settings parameters as well.

Suppose the server supports Extended CONNECT and has a wss::// URL, but does not support
bootstrapping WebSockets over this HTTP connection.
In this case, a client attempting to initiate a WebSocket handshake using Extended CONNECT will fail,
and the client would need to create a WebSocket connection using the HTTP/1.1 Upgrade mechanism.

This is why a SETTINGS_ENABLE_WEBSOCKETS settings parameter is needed.


# SETTINGS_ENABLE_WEBSOCKETS settigs parameter for H2 and H3
This document adds a new SETTINGS parameter to those defined by
{{HTTP3}} Section 11.2.2 and {{HTTP2}} Section 11.3.

The new parameter name is SETTINGS_ENABLE_WEBSOCKETS.
The value of the parameter MUST be 0 or 1, with 0 being the default.

A sender MUST NOT send a SETTINGS_ENABLE_WEBSOCKETS parameter
with the value of 0 after previously sending a value of 1.

If the server supports bootstrapping WebSockets over the HTTP connection,
it SHOULD include the SETTINGS_ENABLE_WEBSOCKETS parameter in the SETTINGS frame with a value of 1.
If the server does not support bootstrapping WebSockets over the HTTP connection it SHOULD send the parameter with a value of 0.

A client MUST not send this setting parameter.
Receipt of this parameter by a server does not have any impact.


The SETTINGS_ENABLE_WEBSOCKETS parameter would allow the client to determine in advance whether the server supports WebSockets over the connection for HTTP/2 or HTTP/3.
This allows the client to avoid sending unnecessary WebSocket handshake requests on HTTP connections that do not support WebSockets.

This mechanism will improve compatibility with other extended CONNECT-based protocols.

For compatibility with past implementations which do not use this parameter,
 clients MAY initiate a WebSocket request without the receipt of this parameter.


# Security Considerations

This document introduces no new security considerations beyond those discussed in {{!RFC8441}}.

# IANA Considerations

## HTTP3
This document registers a new entry in the "HTTP/3 Settings" registry (Section 11.2.2 of {{HTTP3}}).

Value: TBD

Setting Name: SETTINGS_ENABLE_WEBSOCKETS

Default: 0

Status: permanent

Specification: This document

Change Controller: IETF

Contact: HTTP Working Group (ietf-http-wg@w3.org)

## HTTP2
This document registers a new entry in the "HTTP/2 Settings" registry (Section 11.1 of {{HTTP2}}).

Code: TBD

Name: SETTINGS_ENABLE_ENABLE_WEBSOCKETS

Default: 0

Status: permanent

Specification: This document

Change Controller: IETF

Contact: HTTP Working Group (ietf-http-wg@w3.org)

--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge people.

Thank you for reading this draft. :)
