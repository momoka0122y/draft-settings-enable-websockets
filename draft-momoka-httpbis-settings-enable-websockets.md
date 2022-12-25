---
title: "Define SETTINGS_ENABLE_WEBSOCKETS settings parameter for HTTP/2 and HTTP/3"
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

This document proposes the addition of a new HTTP settings parameter, SETTINGS_ENABLE_WEBSOCKETS, which would be sent by the server to the client in the HTTP/2 or HTTP/3 handshake response. This parameter would indicate whether the server supports WebSockets over HTTP/2 or HTTP/3 on the established connection. The SETTINGS_ENABLE_WEBSOCKETS parameter would allow the client to determine in advance whether the server supports WebSockets over the connection, allowing it to avoid sending unnecessary WebSocket handshake requests on HTTP/2 or HTTP/3 connections that do not support bootstrapping WebSockets.


--- middle

# Introduction

The mechanisms for running the WebSocket protocol over a single stream of an HTTP/2 and HTTP/3 connection are defined in {{?RFC8441}} and {{?RFC9220}}. For bootstrapping WebSockets from HTTP/2 and HTTP/3, the extended CONNECT mechanism is used. Support for the extended CONNECT is advertised using HTTP/2 and HTTP/3 settings SETTINGS_ENABLE_CONNECT_PROTOCOL. However, the support of extended CONNECT does not necessarily indicate support for WebSockets over that HTTP connection. Other protocols such as {{?I-D.draft-ietf-webtrans-overview}} also use extended CONNECT and will be sending SETTINGS_ENABLE_CONNECT_PROTOCOL settings parameters as well.

If the server sends SETTINGS_ENABLE_CONNECT_PROTOCOL because it supports extended CONNECT but not bootstrapping WebSockets over that HTTP connection, the client sending a WebSocket handshake request will result in a 501 response and falling back to HTTP/1 will be needed.


# SETTINGS_ENABLE_WEBSOCKETS settigs parameter for h2 nad h3
This document proposes the addition of a new HTTP settings parameter, SETTINGS_ENABLE_WEBSOCKETS, which would be included in the SETTINGS frame of the HTTP/2 or HTTP/3 handshake response. If the server supports WebSockets over the HTTP connection, it would include the SETTINGS_ENABLE_WEBSOCKETS parameter in the SETTINGS frame with a value of 1. If the server does not support WebSockets over HTTP/2 or HTTP/3, it would not include the parameter in the SETTINGS frame or send it with a value of 0.

The SETTINGS_ENABLE_WEBSOCKETS parameter would allow the client to determine in advance whether the server supports WebSockets over the connection for HTTP/2 or HTTP/3, allowing it to avoid sending unnecessary WebSocket handshake requests on HTTP/2 or HTTP/3 connections that do not support WebSockets. This would improve the performance and efficiency of WebSocket connections over HTTP/2 and HTTP/3, as well as providing better compatibility with servers that support WebSockets over HTTP/1 or HTTP/2 but not HTTP/3.


# Security Considerations

This document introduces no new security considerations beyond those discussed in {{?RFC8441}} and {{?RFC9220}}.

# IANA Considerations

## HTTP3
This document registers a new setting in the "HTTP/3 Settings" registry (Section 11.2.2 of {{?RFC9114}}).

Value: TBD
Setting Name: SETTINGS_ENABLE_WEBSOCKETS
Default: 0
Status: permanent
Specification: This document
Change Controller: IETF
Contact: HTTP Working Group (ietf-http-wg@w3.org)

## HTTP2
This document registers an entry in the "HTTP/2 Settings" registry (Section 11.3 of {{?RFC9113}}).

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
