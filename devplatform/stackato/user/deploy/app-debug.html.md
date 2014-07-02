---
layout: default-devplatform
permalink: /als/v1/user/deploy/app-debug/
---

Remote Debugging[](#remote-debugging "Permalink to this headline")
===================================================================

Different languages have different tools an protocols for remote
debugging, but most require a connection between the running application
code and the debugging tool or IDE on a port and protocol other than
standard HTTP(S).

Application Lifecycle Service makes this possible through the [*Harbor port
service*](/als/v1/user/services/port-service/#port-service), and the
[*helion push*](/als/v1/user/reference/client-ref/#command-push) command has
a `-d` option to set up port forwarding for a remote
debugging session automatically.

See the [*JPDA Debugging*](/als/v1/user/deploy/languages/java/#java-web-debug) section
of the Java deployment documentation for an example.

STACKATO\_DEBUG\_COMMAND[](#helion-debug-command "Permalink to this headline")
---------------------------------------------------------------------------------

The helion client can automatically start a local debugger client or
IDE instance with connection information for a newly pushed application.

When the `-d` debugging option is given to
[*helion push*](/als/v1/user/reference/client-ref/#command-push) command,
the client looks for a STACKATO\_DEBUG\_COMMAND environment variable. If
present, the command specified in that variable is run after the push
completes as child process in the foreground (i.e. blocking the parent
`helion` process) in the local application source
directory.

Special %HOST% and %PORT% variables can be used in this command, which
are replaced with the hostname or IP address and port number of the new
Harbor debugging service.

### [Table Of Contents](/als/v1/index-2/)

-   [Remote Debugging](#)
    -   [STACKATO\_DEBUG\_COMMAND](#helion-debug-command)
