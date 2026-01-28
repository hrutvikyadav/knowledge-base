---
id: CAN Features
aliases:
  - CAN Features
tags: []
---

# CAN Features

# Outline
## CAN Speed
We know that in a **C**ontroller **A**rea **N**etwork, *nodes* i.e. `ECU`'s are connected together and data is transmitted between them in the form of CAN frames.
We want to consider the speed with which these frames will travel on the CAN bus.
> CAN frames can go upto 1 mbps fast on a bus with length 40 meters
> As the bus length increases, the speed will decrease

- 1000 kbps at 40 meters length meaning
    - kbps is kile bits per second
    - because this is a serial bus, bits will be sent one after the other serially
    - usually across the industry CAN will be configured for 500 kbps (although it can transmit more, this is sufficient)
    - this means in one second, 500,000 bits will be transmitted one after the other
    - ususally with a payload of 8 bytes along with the overhead bytes, one frame will take upto 125 bits
    - considering this, we can say that if the bandwidth is 500,000 and one frame can fit 125 bits then we can send 500000/125 = 4000 frames per second but due to overhead and other considerations, we will say that we can transmit ==2000 frames per second==
    - this calculation will be used later to decide further information on bus overloading, bus traffic, how many nodes(ECU's) can be connected in a network, how many frames(baud rate) will be transmitted trough the network, time period(periodicity) for periodic messages and so on.

> [!warning]
> The real-world throughput is usually lower than the theoretical max e.g., due to:
> - Bus arbitration
> - Bit stuffing
> - Idle time between frames
> - Error handling and retries

## CAN Data 
CAN frame has 2 parts
- PCI(protocol control information)
- Data

When we are sending frames over the CAN network, it is not enough to simply send the data because we need to know some metadata like who is it intended for, what it the length and so on. This metadata is encoded in the PCI part of the frame. ^PCI

As per the CAN protocol ( rules ) the maximum data we can send per frame is 8bytes max i.e. we can send less than that as well.
However, we cannot send *bits - it has to be bytes* (2 bits ❌ 1 byte ✔️)
> [!info]
> If the use case needs data > 8 bytes then we can use other protocols like CANtp (transmission layer protocol). It allows larger data to be split into frames which can be sent over the network.

Another thing to consider is that if we have an odd number of bits to send, then we send remaining as dummy bits.
Ex. to send - 10 bits; which does not fit in 1 byte but does fit in 2 bytes along with some bits which are rendered irrelevant. These are sent as dummy bits(6 in this case) and our **application must be aware of this fact**

The CAN Rx will know how many bytes were sent via the DLC field in the [[#^PCI|PCI]]

## CAN Bus
The physical CAN bus is implemented as a twisted pair cable terminated on both ends with a fixed 120 Ohm resistor. One wire is CAN low and other is CAN high
It is a serial communication bus which means bits will be transmitted one after another. The termination resistor provides impedance matching???
This one byt one bit transmission is facilitated with the **differential voltage** i.e. if we want to send logical high (1) the delta V will be 0V(Dominant), and for logical low(0) delta V will be 2V(Recessive)

> [!hint]
> Logic 0 is called dominant bit because in case of multiple node in the network, if two ECU try to transmit over the bus, assuming one is sending logic 1 and other is sending logic 0, the bus will first take the logic 0.
> This helps prioritize which ECU will transmit in case of conflict.
^dominant-recessive-concept
==This will come up lateer in error handling and bus arbitration==

<!-- TODO: what will happen if 2 or more ECU try to send logic 1??? -->

Because of the differential voltage and twisted pair CAN bus is highly immune to electrical noise.
- Twisted pair caused the same noise(if any) to be added to both the wires i.e. VH and VL.
- And due to differential voltage the noise from both VH and VL cancels out.

## CAN Logic
## CAN Controllers and transreceivers
## CAN Network Topology

# Transcript

Instructor: In this lecture,
we are going to learn the initial part
of the CAN features, right?
So the agenda of this lecture is,
in this lecture, we are going
to learn about the CAN speed, the CAN data,
and about CAN bus, the physical part, and the CAN logic
.We're going to understand the basics
about the CAN controllers, what they are,
and what are CAN transceivers and why do we need them
.And finally the network topology
.So that's our agenda for this lecture
.So let's directly jump into it. So the CAN speed, right?
What's the meaning of CAN speed, right?
The CAN frames, we know that in the CAN,
multiple ECUs are connected in the network
and it has to transfer the data
in form of the frames, right?
So the CAN frames,
they can go on the bus, they'll travel on the bus
.So what's the speed
with which the frames are transmitted on the bus, right?
That is the meaning of CAN speed
.So CAN frames can go up to 1 mbps speed
with a bus length up to 40 meters, right?
As the bus length increases,
the maximum speed gets reduced, right?
So you can see this graph, right?
This graph, you can see
.Here in this graph, you are able to see
that the Y-axis talks about the bit rate,
that is 10 kpbs 20 kbps, 50 kbps,
100 kbps, 200, and this 1 mbps
.And the X-axis talks about the length of the bus, right?
So as and how you can see,
for 1,000 kbps, that is 1 mbps,
this point we'll talk about, for a bus length of 40 meters,
we can have up to 1,000 kbps, that is 1 mbps speed
.And as and how the CAN bus length increases the length
of the bus, the length of the wires basically,
as and how it increases,
the maximum allowed speed keeps on decreasing
with this path
.For example, if you want to go for 500 kbps,
then it can go up to 100
.For 100 meters, if you want how the bus length,
then the maximum you can have is 500 kbps, right?
That's the meaning of this chart, right?
So that's about the CAN speed,
and let us try to understand this CAN speed literally
.We talk about this mbps and kbps, what does it mean?
Kbps, if you're talking about here, let's say,
Kbps, k stands for kilo and bits per second
.Since this is a serial bus, we are going to transmit one bit
after one bit after one bit after one bit
.So here, the bps means bits per second, right?
So 500 kilobits per second
.So usually in industry, we'll be having,
the CAN will be configured for 500 kbps
.Even though the maximum speed can go is up till 1 mbps,
usually in generally the CAN speed will be configured
with the 500 kbps in the industry today
.So what does it mean, 500 kbps means?
It means that in one second,
500 kilobits can be transmitted
.That is 500,000 bits can be transmitted, okay?
And let's say about a single frame can take up to 125 bits,
usually with eight bytes of data
and all maximum data length,
the normal bits will be 111 bit
.I'll explain this concept of bit
when I'm explaining the frame formats and all,
then you'll be able to see how many bits
.But let's say for an example,
that a frame will take about 125 bits per frame, right?
Then in a second,
the bus can fit around maximum of 2,000
.That is 500 K bits and 125 bits per frame
.So 2,000 frames can be fit
on the CAN bus in a single second
.In one second, 2,000 frames can be sent, right?
So this calculation will be used
to calculate the bus overloading
and the bus traffic and all,
how many nodes can be connected,
how many frames can we have for a network,
and their periodicity of their periodic messages and all,
for all these calculations,
which you can see in further coming lectures
.This is a basic, right?
So depending on this speed, CAN speed,
we can decide on the number of ECUs,
the number of nodes
which can be presented in the CAN network,
and what will be baud rate,
and what will periodicity, how many frames can be possible
.All these calculations will be depending on this
.So I hope this explanation have made you
understand the meaning of this CAN speed, right?
And next we'll go to the data limit, right?
So why basically we use CAN?
It is because, let's say there are multiple ECUs,
and in this example, let's take two ECUs,
so ECU 1 and ECU 2
.Two ECUs are there, which are communicating the data
.And it has, ECU 1 has the application
.Application 1 which is sending the data
.So this let it be the one which is transmitting
and Application 2 is receiving the data
.So this one will be Rx, right?
So the transmission is happening
.And data, it has to send the data, right?
So can the,
CAN protocol on the bus,
just data, can we send over the bus?
No, because we don't know what is the data
and what is related to the message id,
what data contains, and what is the size of the data
.All those informations are required
that are things which are imposed by CAN protocol, right?
So along with the data,
which has to be transmitted between the application,
there are other informations also
which are required for communication
.So the bigger whole thing,
which is sent over the bus, is called as a frame
.And the frame, inside that, the data is one of the part
.The other bits or other parts
which are present in the frame,
which is apart from data, are protocol related information,
which we call as protocol control information, PCI
.Okay?
So you can say that the frame has two parts
.One is PCI, which is all the various fields
which are protocol specific,
here, the protocol being CAN protocol,
and the data, the actual data which is communicated
between the multiple nodes, right?
So in a single frame, how much data can you send?
Can you send thousands of byte or can you send one bit each
or can you send some 10 bits?
Can you send three bytes, four bytes, 10 bytes, megabytes?
How much maximum data can you send in a frame
is the question, and in CAN protocol, it is decided
that the maximum data amount
which can be sent per frame is eight bytes
.So in a single frame the maximum is eight bytes,
and you can even send zero bytes
.Also you can send one byte, two byte,
but you cannot send 10 bits,
because 10 bits is two
or one byte and two bits, right?
So you cannot send 10 bits,
you have to send in multiple of bytes
.That is a rule imposed by the CAN protocol
.So you can send zero bytes, one byte, two byte, three byte,
four byte, five byte, six byte, seven byte,
eight byte, right?
So maximum is eight bytes,
you can send per the each frame, right?
So that is a data limit,
maximum limit imposed by the CAN protocol
.So if you are having more than eight bytes,
then there are other protocols which is
supporting that transport layer protocols
called as CANTp is there,
which is ISO 15765 protocol,
where you'll divide the data
and send that a huge amount
of data greater than eight bytes into multiple frames,
multiple CAN frames, right?
That one will be there
.And for now, you can understand
if this is a course about the CAN protocol,
in a single frame, maximum of eight bytes
of data can be transmitted
.And let's say, take a case that you have some 10 bits
of data to be transmitted,
how are you going to achieve that?
You don't have, that is two bytes of data
.That is,
let's say, two bytes is 16 bits,
16 bits and other, six bits and other
.So you will send two bytes
with last six bits as a dummy bits
.The application should know
that last six bits is dummy bits
.And you will send two bytes actually in that case
.The next higher byte is what we are going to send
.So that's how you will send a transmit in the CAN bus, okay?
So how many, I told that data can be one byte, two byte,
three byte, any number of bytes,
but maximum is eight bytes
.So how does the CAN frame knows
that the application knows that?
How many bytes of data is there?
How does the ECU know about that?
For that, you have to understand the CAN frame format
.So when I am explaining CAN frame format,
there's a field called the DLC data length code,
which will tell how many bytes
of data is there in this frame
.I'll be explaining it that part in detail there,
but right now you will understand
that how any number of bytes, from zero to eight,
can we present per frame, maximum is eight bytes,
and how many bytes is present, you'll be knowing
by a part of the protocol control information,
which is called as a DLC
.Okay? Good
.So that covers the CAN speed and the data limit
.And next we come to the physical layer part
of the CAN protocol, that is, the CAN bus
.So you can see,
this is how the CAN bus is represented as, okay?
So you can see there are twisted pairs
.So there is one wire and there is another wire
.You take that wire and you twist them like this
.They're intertwined
.You are seeing the snakes intertwined
with each other kind of a thing, right?
Like that only this CAN bus is there
.So CAN bus mainly contains two wires,
which are twisted with each other
called as twisted pair, okay?
The two wires, the one wire is called as a CAN high,
other wire is called as CAN low,
and they're twisted with each other,
and they're ended with a resistor
.This is one resistor,
and this is one resistor, right?
Like this, and the resistor value is always 120 ohm
of resistance, right?
You have two wires
.One wire is called as CAN high,
other wire is called as CAN low,
and these wires are terminated on the both ends
with 120 ohm resistors, okay?
And CAN bus is a dual twisted pair wires
called CAN high, CAN low,
and they're having the differential voltages, right?
First let us understand why do we go for the twisted pair
.For that, first you have to understand the logics, right?
So you know this is a serial bus communication,
that means the bits are transmitted
.So at a given point of time the wires will be,
the CAN bus will be having a single bit value, right?
So it can be either a one bit,
a logic one or logic zero, right?
So depending on the voltages
which are present on CAN high and CAN low,
the delta V is computed
.Delta V is nothing,
but the difference between the voltages
in CAN high and CAN low
.That is, voltage in CAN high,
voltage in CAN high minus voltage in CAN low
.The difference of voltage is called
as differential voltage, all right?
If you want to put logic one on the CAN bus,
then you would make the CAN high value as 2.5 volts,
CAN low value as 2.5 volts
.If both the wires are having same voltage,
which is 2.5 volts, then the delta V,
the difference between them, the differential voltage,
the difference between them will be zero
.And if there want to put the logic zero,
then CAN high will be 3.5 volts
and CAN low will be 1.5 volts, right?
And the difference between them will be 2 volts, right?
So you can see that the mapping between logic and delta V,
so if the delta V is 0, then it is logic one
.If delta V is 2 volts, then logic zero
.You can see in this diagram, right?
So logical one, both of them is 2.5 volts
.Logical zero, CAN high is 3.5 volts, CAN low is 1.5 volt
.If you want to remember, you can see it like that
.You imagine CAN high as your upper lip
CAN low is lower lip in this
.And if you want to put zero, logical zero,
if you make oh, like this,
it looks like a zero, right?
And this is one in a horizontal position, right?
So for this one which is horizontal position,
both of them are 2.5 volts, and this one is like CAN high
and CAN low are different voltages
.CAN high is 3.5, CAN low is 1.5
.So this is a zero and this is one again, right?
This is the way you can remember, right?
And since a logical zero
for logical zero, you have to have different voltage values,
for CAN high and CAN low,
this one is called as a dominant bit
.I'll explain why it's called a dominant bit
and what's a dominant bit,
but you just remember that always in CAN protocol,
logic zero is called as a dominant bit,
logic one is called as a recessive bit, right?
So the default state of a CAN bus will be recessive,
because the CAN high and CAN low voltages are equal there
.That is, the difference voltage is zero, right?
And the dominant bit is logic zero,
because the CAN high voltage
and CAN low voltage are different
.The differential voltage is non-zero, okay?
Logic zero is called as a dominant bit,
because let's say, if two nodes are trying
to change the value of the bus
and one node is trying to put logic one on the bus
and other logic node is trying to put logic zero on the bus,
then the bus will take the logic zero
and not logic one
.So when competed between these two logics,
since logic zero wins the bus,
that's why logic zero is called as dominant,
because it has dominated over logic one
and it has made the bus logic as equal to logic zero
.So logic zero is called as a dominant bit,
logic one is called recessive bit
.Remember this, because this dominant
and recessive concept will be used
for various CAN errors and for arbitration,
the way how the nodes are going to win the bus
.In those concept,
which is one of the beautiful concept of CAN,
this is the basic concept,
that logic zero is dominant
and logic one is recessive, okay?
Why it is called as dominant?
Because if the logic one and logic zero are competing
to have the bus value, then the node which is trying
to put logic zero will win,
because the bus will take logic zero on the bus, okay?
So that's the meaning of logic zero and logic one
and that is how CAN high and CAN low is there
.And since we have this as a differential voltage here,
because of the using differential voltage,
and because of twisted pair, the CAN bus is highly immune
to the electrical noise, right?
You know this CAN bus?
Where we are going to put this CAN bus in the car, where?
Between the connecting multiple ECUs,
we are putting it in the car,
where there're a lot of engine is there,
shafts are there, mechanical parts are there,
chassis is there, vibrations are there,
and it is an environment of pretty high electrical noise,
whatever voltage is present in the bus,
because of inserting of this noise,
we will be having some distortions
and it may result in some error
.So it is very, very important that whichever bus
and whichever protocol we are implementing
in this environment of high electrical noise,
the protocol and the bus should be very highly immune
to the noise, right?
Now CAN is very much suitable for that
.Let us understand how
.Let's say that we are trying to send bits on the CAN bus
and CAN high has the voltage value VH, okay?
And CAN low has a voltage value VL, okay?
Depending on whether it is logic zero or logic one,
VH and VL will be either 3.5 volts,
VH will be 3.5 volts or 2.5 volts,
VL will be 1.5 volts or 2.5 volts, all right?
And because of noise,
there is a additional voltage that's inserted in the bus
.Since it is a twisted pair, they're very close to each other
and if a noise is being inserted in one wire, it is bound
to insert same amount of voltage in other wire also, right?
So let's say the noise voltage,
which is random, which will be inserted, will be VN, okay?
N stands for noise, okay?
So since the noise has been added
to both the wires,
CAN high and CAN low
.So the CAN high value will become VH plus VN,
and the CAN low value will become VL plus VN
.So the VN, this voltage now, which is noise voltage,
has been added to CAN high and CAN low now
.Now here in this case, if there was no noise,
what we would do?
We would take a differential voltage
.The differential voltage is delta V
equal to CAN high value minus CAN low value, right?
Minus CAN low,
which is nothing, but VH minus VL
.Okay?
Let's say VH minus VL equal to VD, differential voltage
.Okay?
Now in our case,
after inserting the noise, what will this be,
CAN high minus CAN low?
CAN high is nothing, but VH plus VN,
minus, CAN low voltage is VL plus VN, right?
Which is nothing, but VH plus VN
minus VL minus VN, right?
So plus VN minus VN gets canceled
.We are left with VH minus VL,
which is nothing, but we're told VH minus VL is VD
.So you have both the cases where there is noise
and there's no noise, right?
In both cases, the differential voltage has remained same,
because the noise has canceled itself
.That's why we are taking the differential voltage
.Hence, irrespective of the value of VN,
VN may be small, VN may be big,
because of two properties of CAN protocol,
that is, one being the twisted pair
.So that same noise gets added to CAN high and CAN low both
.And because we are taking the differential voltage,
because of these two properties,
the CAN bus is highly immune to the noise
and hence is very much suitable for the automotive purposes
where the environment is very noisy, right?
So that's why the twisted pair, why we are going
for twisted pair and going for differential voltage?
Because of noise immunity
.And you can see
that the CAN bus is terminated both the sides
with 120 ohm resistor itself,
because impedance matching,
because let's say, a voltage is there going,
and if you are familiar
with the signals and systems concept,
if you're from the electronics background,
then you would know that the signals which are traveling
through this wire will be bounced back at the end,
and there is something called the reflective waves
which will interfere with the existing waves
.So this reflection of waves has to be minimized
.In order to minimize this wave,
we have to terminate the wires
with a suitable resistor, right?
Depending on different value of resistors,
the level of or the degree of reflection
of waves will be altered, right?
There's a equation called as one divided by four
by epsilon nought, mu nought,
or one by four by epsilon mu,
where epsilon is a electrical permittivity,
and mu is magnetic permeability
.So depending on this equation and all,
it has been calculated that for CAN protocol
with the wire, whichever is used,
120 ohms, if you connect it at the end of each ending
of the CAN bus, then this impedance matching will happen
.And the wave, which is reflected back, will be very minimal,
and hence for the purpose of this impedance matching,
we connect 120 ohm at the end of CAN bus, okay?
So this is the basics of the CAN bus protocol,
why we are going for the twisted pair,
why we are going for a differential voltage,
why we are going for 120 ohms,
and what are the different CAN logics, CAN high and CAN low
.But there's one problem here. What's that problem?
You know that our microcontrollers which you're working on,
this is not the logic which you are seeing, right?
We always have logic zero is represented by 0 volts
.Logic one represented by let's say,
3.3 volts or 5 volts and all
.That logic is called the TTL, transistor-transistor logic
.But since it's not immune to noise,
and all those things we are going for CAN logic here,
but how is this CAN being interpreted by microcontrollers?
The microcontrollers work with TTL and not CAN logic
.The bus is having the CAN logic,
but the microcontrollers are the ECUs,
are having TTL logic
.So how are we going to connect a bridge?
We need someone
who has to take this TTL logic from microcontrollers
and convert it into a CAN logic and put it on the CAN bus
.And vice versa, take the CAN logic, present on the CAN bus,
and convert it to the TTL logic
and give it to the microcontrollers,
which is it is able to understand, right?
We need some mediator or translator in between
which is doing that signal conversion,
technology conversion, right?
That's where we come across the concept
called as a transceiver, CAN transceiver, right?
So in this CAN transceiver you can see that,
transceiver, you can see this side
which is connected to bus,
is having the CAN logic, CAN high and CAN low
.And here we have the TTL logic, Rx and Tx,
this TTL logic, all right?
So how we have?
You have to know that there are concept like,
this is a macro controller, which is our embedded system,
and then we have a CAN controller,
which implements the complete CAN protocol
inside the hardware
.And then we have a transceiver
which will handle the logic conversion
.This CAN controller also
implements a CAN protocol inside itself,
but it is working on TTL, right?
So this TTL, transistor-transistor logic,
that is 0 volt and 5 volts
.And here the CAN logic,
that means the 1.5 volts, two 3.5 volts
and 2.5 volt for logic one
.This inter conversion is done by CAN transceiver, right?
So whatever data and all has to be converted,
that data will be given
by the microcontroller to CAN controller
.The CAN controller will form the CAN frame
.But in the TTL bits,
and the complete CAN frame is given to the transceiver
.The CAN transceiver, whatever is there,
that same total CAN frame, it converts from TTL to CAN logic
and sends it on the CAN bus
.And vice versa, once the CAN frame is coming,
the frame is taken,
converted from CAN logic to the TTL logic
.The frame with the TTL logic is given to the CAN controller
.The CAN controller will remove all the controller specific,
protocol specific fields,
and whatever is there that needed in data part,
that will be given to the microcontroller
and microcontroller will further process it
for whatever application
and all those things which is needed for it
.So that's how the CAN real implementation works
.We have microcontroller, then we have a CAN controller,
and we have a CAN transceiver
.This microcontroller will be working on the software,
hardware plus software will be there
.The logic will be implemented
by software which is flashed in this and it works,
but the CAN controller doesn't have any software
.This is completely hardware
.The whole CAN protocol are implemented
in the hardware itself
.There's no coding in this CAN
.There are some configurations which can be done,
filter configuration and all those things
.But the CAN protocol is implemented in hardware itself
and CAN transceiver is also complete hardware, right?
This is how the whole thing
of CAN communication in each node is going to happen
.If there are 10 nodes,
then there will be 10 CAN controllers at least, okay?
It can be more,
but at least 10 CAN controllers will be there, right?
So I hope you understand this CAN basics part,
what's the CAN transceiver, and what's the CAN controller,
and what's the need of for that, right?
And let's see the final part which is CAN topology
.So if there are multiple ECUs are there,
then we have something called as the CAN network
.And the topology which we are having is called
as a bus topology
.What is the meaning of bus topology you say?
Let's say the network is having N number of ECUs,
ECU 1, ECU 2, ECU 3 to ECU N
.And this is a CAN bus
.You can see CAN high, this is a CAN high,
and this is a CAN low wire
.Here I have shown is that the parallel,
but this will be twisted
.CAN high, CAN low will be twisted
as you have seen previously, right?
Like this, CAN high and CAN low will be like,
twisted like this
.Why? Because of noise immunity
.And they're ended with 120 ohms at the both ends,
at this end also, at that end also
.And here it is tapped and CAN high,
ECU 1's CAN high part
.So here in ECU 1, when I say
that means here the microcontroller, CAN controller,
and this CAN high
.These two wires are there, right?
These two pins are there, right?
Those will be the CAN high will be connected
to the CAN high of the bus
.The CAN low will be connected to the CAN low of the bus
for each of those,
and that's how the communication happens,
that's how the ECUs are connected
.And since all these ECUs are connected
to the same CAN bus, we call it
as these all ECUs are present in the same CAN network
.So this whole thing is a CAN network, okay?
Which is N number of ECUs connected to the bus
of CAN high and CAN low
.Note that the baud rate, that is the speed
that is if one ECU is connected to,
configured as a 500 kbps,
then all of this has to be 500 kbps itself
.The speed, or baud rate what we call, is configured
for the network and not for individual ECUs
.That configuration has to be done in individual ECU,
but it has to be same
.It cannot have that here, 500 kbps
.Here, 200 kbps. Here, 1 mbps
.We cannot have that
.The network is having a baud rate of 500 kbps
or let's say this baud rate of this CAN bus is 200 kbps
.Then all the ECUs which are connected to this CAN network,
all those controllers,
have to be configured at 200 kbps only, okay?
So that's the basic features of the CAN network
.So in this lecture, we understood about the CAN speed,
that is, CAN can go up to 1 mbps maximum,
up to 4 meters length
.And as in how the bus length keeps on increasing,
the wire length keeps on increasing,
the maximum speed keeps on decreasing
.And in industry as a standard,
we usually go for 500 kbps
.And hence, the bus length can go up to 100 meters, right?
And the CAN data, the CAN frame has a PCI part,
the protocol related part,
and the actual data which has to communicated
.Per frame, we can communicate up to eight bytes of data
.We can even have less than eight bytes
.And each, how many bytes are there?
It is commonly told in the frame in a field called the DLC,
which you'll understand
in the frame formats in extensive way
.We cannot have 10 bits, 12% all,
we have to have multiples of bytes only
to communicate in the CAN frame
.Then we understood about the CAN bus logic
.It's a twisted pair. We use a differential voltage
.What are the CAN logics and all,
and why we end terminate the bus with 120 ohm,
because of impedance matching
.Then the CAN logic, CAN zero is represented
with CAN high being 3.0 or 2.5
.Logic zero is dominant and CAN high will be 3.5 volts
.CAN low will be 1.5 volts
.Logic one is being the recessive bit
with both CAN high, CAN low being 2.5 volts
.And how do we remember
that's the way I also understood about that trick,
that's about the CAN logic
.And because of difference in CAN logic
and how the microcontrollers work in the TTL logic,
we need a bridge between them
.So we understood about the CAN transceivers
and the implementation of CAN protocol
in the hardware in the code,
which is called as CAN controllers
.So we understood about the CAN controller
and transceiver, the basics
.And then we understood the network topology,
how ECU are connected in the CAN protocol
.The network is in the bus topology
.So I hope this is clear
.In next lectures, we'll try
to understand the various characteristics of CAN protocol...
