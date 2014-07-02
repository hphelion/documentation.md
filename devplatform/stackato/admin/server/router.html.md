---
layout: default-devplatform
permalink: /als/v1/admin/server/router/
---

Router[](#index-1 "Permalink to this headline")
================================================

The Application Lifecycle Service Router role manages HTTP and HTTPS traffic between web
clients and application instances. In conjunction with the Cloud
Controller, it maps application URLs to the corresponding application
instances running in Linux containers on DEA nodes, distributing load
between multiple instances (containers) as required.

Application Lifecycle Service's default router ('router2g') supports
[WebSocket](http://www.websocket.org/aboutwebsocket) (including
"wss://" secure web sockets) and [SPDY](http://www.chromium.org/spdy).

Settings[](#settings "Permalink to this headline")
---------------------------------------------------

The Router is configured using [*kato
config*](/als/v1/admin/reference/kato-ref/#kato-command-ref-config). The
following settings are configurable:

-   **client\_inactivity\_timeout**: time (in seconds) the router waits
    for idle clients (default 1200 seconds). To change this:

        $ kato config set router2g client_inactivity_timeout 2400

-   **backend\_inactivity\_timeout**: time (in seconds) the router waits
    for applications to respond (default 1200 seconds). To change this:

        $ kato config set router2g client_inactivity_timeout 2400

-   **prevent\_x\_spoofing** (true|false): Enable HTTP "X-" header
    spoofing prevention (default 'false'). When enabled, the router
    discards all X- headers sent by the client (e.g. X-Forwarded-For,
    X-Forwarded-proto, X-Real-IP, etc.) and replaces them with values
    determined by the router itself. Anti-spoofing features should only
    be set at the network gateway, so this option should not be enabled
    when routers are configured behind an external load balancer. To
    enable:

        $ kato config set router2g prevent_x_spoofing true --json

-   **session\_affinity** (true|false - disabled/unset by default):
    Enable sticky session support on the router. Overrides normal
    round-robin load balancing for clients with JSESSIONID, SESSIONID,
    or PHPSESSID cookies set (configurable in the router's
    *config/local.json* file), routing those clients to specific
    application instances. If the backend assigned on the first request
    goes down, a new one is automatically assigned. Clients can delete
    their sticky session assignment by removing the
    STACKATO\_SESSION\_AFFINITY cookie.

-   **x\_frame\_options**: Prevent clickjacking on requests with
    [X-Frame response
    header](https://developer.mozilla.org/en-US/docs/HTTP/X-Frame-Options)
    configuration. Disabled if empty (default). Valid values are:

    -   DENY
    -   SAMEORIGIN
    -   ALLOW\_FROM \<uri\>

    For example:

        $ kato config set router2g x_frame_options SAMEORIGIN

**Note**

Alternatively, end user applications can employ
[framekiller](http://en.wikipedia.org/wiki/Framekiller) JavaScript
snippets to help prevent frame based clickjacking.

WebSockets[](#websockets "Permalink to this headline")
-------------------------------------------------------

Applications using web sockets must use the VCAP\_APP\_PORT or PORT
[*environment
variables*](/als/v1/user/reference/environment/#environment-variables)
to set the default listener port of the WebSocket server.

SPDY[](#router-spdy "Permalink to this headline")
--------------------------------------------------

[SPDY](http://dev.chromium.org/spdy/) is a protocol developed by Google
for reducing web page load time. The router supports SPDY versions 2 and
3. Applications can use SPDY over any HTTPS connection, so long as the
connection consumers (the application server and browser) support it.

### [Table Of Contents](/als/v1/index-2/)

-   [Router](#)
    -   [Settings](#settings)
    -   [WebSockets](#websockets)
    -   [SPDY](#router-spdy)
