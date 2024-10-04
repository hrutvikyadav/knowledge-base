---
id: TCP
aliases:
  - TCP
tags: []
---

# TCP Overview

TCP is a widely used network protocol. It's a **reliable** protocol that runs on top of an unreliable protocol: IP, short for Internet Protocol
Lets look at how TCP works under the hood:

## IP (internet's postal service)
- First let's take a look at IP(internet protocol) which is the underlying protocol for TCP.
    When data is sent over the network using IP, it is broken down and sent as multiple packets.
    Each packet contains a *`header`* and a *`data` payload*.

    The header contains information like source and destination IP addresses
    > [!warning] IP is an unreliable protocol. It doesn't guarantee that packets will be delivered in order or at all.
    > It's like sending a letter through the postal service. You can't be sure if it will reach the destination or not.
    > Every effort is made to deliver the letter, but it's sometimes packets get lost or delivered out of order.

> [!hint] TCP was created to address these limitations of IP.

## TCP (reliable delivery)
Primarily, TCP provides 2 guarantees:
1. **Reliable delivery**
    TCP ensures that no packets are lost in transit. This is done by asking the receiver for ==*aknowledgement*== for all received packets.
    *If a packet is lost, it will be retransmitted.* [[TCP reliable delivery 2024-10-04 13.58.36.excalidraw]]
2. **In order delivery**
    Additionally, TCP ensures that packets are delivered in the order they were sent. This is done by numbering each packet. [[Ordered delivery 2024-10-04 14.06.26.excalidraw]]
    The receiver will
    - reorder packets if they arrive out of order.
    - wait for retransmission of missing packets.

### TCP connection
TCP is a connection oriented protocol. This means that before any data is sent over `TCP`, a ==*connection must be established*== between the sender and receiver program.
One program takes the role of the `client` and the other the `server`.
The *server waits for connections*, and the *client initiates a connection*. Once a connection is established, the client & server can both receive and send data (==it's a two-way channel==).

A TCP connection is identified using a unique combination of four values:
- destination IP address
- destination port number
- source IP address
- source port number

Example:
If you are trying to reach google from your PC, the TCP connection will look something like -
[[TCP connection 2024-10-04 14.18.00.excalidraw]]

> [!info] If you open multiple connections to googles server, only the source port number will change. The other 3 values will remain the same. This is how the server knows which connection a packet belongs to.
> This is also why you can have multiple tabs open in your browser, each with a different connection to the same server.

### TCP handshake
When a client initiates a connection, a 3-way handshake is performed to establish the connection.
1. **SYN** - client sends a SYN packet to the server. This indicates a connection request. The packet contains the client's initial sequence number to maintain ordering.
2. **SYN-ACK** - server responds with a SYN-ACK packet. This indicates that the server is willing to establish a connection.
3. **ACK** - client sends an ACK packet to the server. This indicates that the client has received the server's response and is ready to start sending data.
Connection is now established and data can be sent in both directions.
> The three-way handshake ensures a reliable connection is created, verifies both devices are ready for data transmission, and sets initial sequence numbers for proper ordering of data packets
