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

Suppose the server supports Extended CONNECT and has a wss::// URL, but does not
support bootstrapping WebSockets over this HTTP connection.
In this case, a client attempting to initiate a WebSocket handshake using
Extended CONNECT will fail, and the client would need to create a WebSocket
connection using the HTTP/1.1 Upgrade mechanism.

This is why a SETTINGS_ENABLE_WEBSOCKETS settings parameter is needed.


# The SETTINGS_ENABLE_WEBSOCKETS Setting
This document defines the SETTINGS_ENABLE_WEBSOCKETS parameter for HTTP/2 and HTTP/3.
A server can send this setting to inform a client that it supports bootstrapping WebSockets over the HTTP connection.

The value of the parameter MUST be 0 or 1, with 0 being the default.

If the server supports bootstrapping WebSockets over the HTTP connection,
it SHOULD include the SETTINGS_ENABLE_WEBSOCKETS parameter in the SETTINGS frame with a value of 1.
If the server does not support bootstrapping WebSockets over the HTTP connection it SHOULD send the parameter with a value of 0.

A server MUST NOT send a SETTINGS_ENABLE_WEBSOCKETS parameter
with the value of 0 after previously sending a value of 1.

A client MUST NOT send this setting parameter.
Receipt of this parameter by a server does not have any impact.


The SETTINGS_ENABLE_WEBSOCKETS parameter is an explicit signal about the server
support for bootstrapping WebSockets on the connection. Where a server declares
it does not support WebSockets, clients can avoid sending WebSocket handshake
requests that would fail. This saves unnecessary work for both client and
server, and potentially reduces delays. For instance, a client that learns an
HTTP/2 or HTTP/3 connection does not support WebSockets via the setting, could
instead attempt to create a WebSocket using the HTTP/1.1 Upgrade mechanism at
the immediate moment it is required.

Other protocols also rely on the extended CONNECT extension for bootstrapping.
This mechanism provides clients with a stronger signal about whether the
WebSocket protocol is supported on a connection. This can help improve
compatibility with other extended CONNECT-based protocols by avoiding the client
making assumption about the supported protocols.

Clients that do not implement this extension will not be able to use its signal.
In order to support legacy deployments, clients MAY initiate a WebSocket request
when they receive SETTINGS_ENABLE_WEBSOCKETS with a value of 0, or if the
parameter is omitted from received settings. Such requests could fail,
introducing additional latency, which this extension is intended to help avoid.

A server that sends SETTINGS_ENABLE_WEBSOCKETS with a value of 0 or omits the
parameter MUST NOT treat reception of the a WebSocket request as a stream or
connection error. Instead, the server can reject the request with a suitable
status code.

# The SETTINGS_ENABLE_CONNECT_PROTOCOL Setting
A server which sends SETTINGS_ENABLE_WEBSOCKETS parameter MUST also send the
SETTINGS_ENABLE_CONNECT_PROTOCOL = 1.

# Security Considerations

This document introduces no new security considerations beyond those discussed in {{!RFC8441}}.

# IANA Considerations

## HTTP/2 Setting
IANA is requested to register the following entry in the "HTTP/2 Settings"
registry maintained at <[](https://www.iana.org/assignments/http2-parameters)>:

Code: TBD

Name: SETTINGS_ENABLE_WEBSOCKETS

Initial Value: 0

Specification: This document


## HTTP/3 Setting
IANA is requested to register the following entry in the "HTTP/3 Settings"
registry maintained at <[](https://www.iana.org/assignments/http3-parameters)>:

Value: TBD

Setting Name: SETTINGS_ENABLE_WEBSOCKETS

Default: 0

Status: provisional

Reference: This document

Change Controller: Momoka Yamamoto (IETF if this document is approved)

Contact: Momoka Yamamoto (HTTP_WG; HTTP working group; ietf-http-wg@w3.org if this document is approved)

--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge people.

Thank you for reading this draft. :)
