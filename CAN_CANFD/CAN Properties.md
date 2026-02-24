---
id: CAN Properties
aliases:
  - CAN Properties
tags: []
---

# CAN Properties
Varius communication protocols have various properties. Here are CAN communication properties

1. ==Wired== v/s Wireless
    CAN is wired with Bus topology
1. ==Serial== v/s Parallel
    CAN is a serial communication protocol with the MSB transmitted first
1. Synchronous v/s ==Asynchronous==
    CAN is an asynchronous communication protocol. The frame is sent over the bus whenever it is ready, and it is self contained with regards to who is the intended receiver of that frame. As opposed to synchronous communication where all nodes are synchronized to a clock signal and if a node want to transmit it need to wait for the next time slot based on the clock pulse.
1. Peer2Peer v/s ==Broadcast==
    CAN is a Multicast communication protocol - similar to broadcast but for one or more nodes instead of all nodes.
    > [!hint]
    > nodes may choose to ignore the frame on the bus if they have no use for it i.e. **filtering**
1. Message based protocol
    CAN is message based as opposed to node based (think message id v/s src + destination addresses).
    > [!hint]
    > the message id decides the priority which will come up in **bus arbitration** later i.e. which message get prioritized on the bus if multiple nodes want to transmit
1. Message filtering
    It depends on message ID. In case of 4 nodes - `A`, `B`, `C`, `D` - Lets say `D` is transmitting message with certain ID; `A` and `B` can filter IN, `C` can filter OUT this ID. This configuration is done with the CAN controller hardware with 2 parameters - `mask` and `filter`
1. Simplex v/s ==Half Duplex== v/s Full Duplex
    CAN is a half duplex communication model where 2 or more nodes can act as receiver as well as transmitter but each node can either rx or tx not both at a given time instance.
1. Acknowledgement method
    The CAN frame transmitted will have a field set aside for acknowledgement purpose. The details will be discussed later.
    This unique method helps acknowledge messages without bus overload at the same time being better than some systems which skip acknowledgement entirely because of overload
1. CSMA-CA protocol
1. ==Temporary== and ==Permanent== Node failures
    Temp failure is when a txed frame was malformed and the tx node will destroy that frame and send the correct one again. Permanent failure is when such situation will happen again and again in which case that node withdraws itself from the network.
    CAN has ways to differentiate between these 2 kunds of failures and then employ respective recovery strategy which is discussed later in detail.


