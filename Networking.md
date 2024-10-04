---
id: 1709208720-IZOO
aliases:
  - Networking
tags: []
---

# Networking

## Network protocols

Network protocols are rules(that are standardized) used by devices to communicate with each other over a network.
Some examples:
- HTTP for web browsing
- SMTP for email
- TCP/IP for general internet communication
- FTP for file transfers

> [!info] Network protocols can be broadly categorized into **layers**
> Each layer:
> - relies on the services provided by the layer below it.
> - provides services to the layer above it.

Various models categorize these layers differently. The most common one is the **OSI model**. but it is only theoretical and not used in practice. The **TCP/IP model** is more practical and is used in practice. It has 4 layers:
```mermaid
---
title: TCP/IP model
---
%%{init: {"flowchart": {"htmlLabels": false}} }%%

flowchart TD
    Application["`**Application**
    It is responsible for providing high-level application services like file transfers, email, etc to the user.
    It relies on the services provided by the TCP/UDP network protocols in the *Transport* layer.
    Some protocols: *HTTP*, *SMTP*, *FTP*`"]

    Transport["`**Transport**
    It is responsible for providing reliable data transfer between two devices.
    It relies on the services provided by the *Internet* layer.
    Some protocols:
    *TCP*, for reliable delivery of data
    *UDP*, for fast but less reliable delivery
    `"]

    Internet["`**Internet**
    It is responsible for routing packets from one network to another to get them to right destination.
    It relies on the services provided by the *Link* layer.
    Some protocols: *IP*, *ICMP*, *ARP*`"]

    Link["`**Link**
    It is responsible for sending data over a physical medium.
    Ex. of a medium: cable, wireless, etc.

    Some protocols: *Ethernet*, *WiFi*, *Bluetooth*.`"]

    Application --> Transport --> Internet --> Link
```

## Further reading
[IP suite wiki](https://en.wikipedia.org/wiki/Internet_protocol_suite)
[Layers of TCP/IP model](https://subscription.packtpub.com/book/networking-and-servers/9781783989522/1/ch01lvl1sec10/the-layers-in-the-tcp-ip-model)

