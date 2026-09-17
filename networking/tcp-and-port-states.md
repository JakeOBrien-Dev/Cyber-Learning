# TCP and Port States

These notes cover some of the TCP concepts that I come across while building my Network Asset Inventory project.

## What is a TCP port?

A port is a numbered endpoint that applications can use for network communication.

A service is the application or function listening on a port.

For example:

- TCP port 22 is commonly associated with SSH
- TCP port 80 is commonly associated with HTTP
- TCP port 443 is commonly associated with HTTPS

These are conventions rather than guarantees. A service can be configured to use a different port.

For example, an SSH server could be configured to listen on port 2222 instead of port 22.

This means that seeing an open port does not automatically prove which service is running on it.

## Open ports

An open TCP port means that a service is currently listening on that port and accepting TCP connection attempts.

An open port does not automatically mean that the service can be accessed from the public internet.

Things such as firewalls, routers and network configuration can limit who can reach the service.

In my Network Asset Inventory project, an OPEN result means that the target accepted the TCP connection attempt.

## Closed ports

A closed TCP port means that the host is reachable, but there is currently no service listening on that particular port.

The host may actively reject the connection.

This is still useful information because the response shows that the host itself is reachable.

A closed port does not mean that nothing can ever use that port. A service could start listening on it later.

## Filtered or unreachable

A filtered or unreachable result means that the scanner did not receive a clear response showing that the port was open or closed.

Possible causes include:

- a firewall silently dropping traffic
- the target host being offline
- routing problems
- an intermediate network device blocking traffic
- the connection attempt timing out

Because there was no clear response, the scanner cannot confidently determine whether a service is listening on the port.

"FILTERED/UNREACHABLE" is a classification used by my scanner rather than a TCP protocol state itself.

## Host responsiveness

Both OPEN and CLOSED results can provide evidence that a host is responsive.

An OPEN result means that something on the host accepted the TCP connection.

A CLOSED result means that the host responded but rejected the connection because nothing was listening on that port.

In both cases, the host responded to the TCP connection attempt.

This is different from ping.

Ping normally uses ICMP, so a host may block ping requests while still responding to TCP traffic.

Therefore:

> A failed ping does not necessarily mean that a host is offline.

## TCP three-way handshake

TCP normally establishes a connection using a three-way handshake.

The sequence is:

1. `SYN` - the client asks to start a TCP connection.
2. `SYN-ACK` - the server acknowledges the request and indicates that it is ready.
3. `ACK` - the client acknowledges the server's response.

The connection is then established and application data can be exchanged.

A simplified example would be:


Client                         Server

SYN  ------------------------>
     <---------------- SYN-ACK
ACK  ------------------------>

       Connection established
