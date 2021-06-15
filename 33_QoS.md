<h2 align="center">QoS = Quality of Service & PoE = Power over Ethernet & VoIP = Voice over IP</h2>

<h4 align="center">IP Phones & Voice VLANs</h4>

Traditional phones operate overthe _public switched telephone network_ (PSTN).
Sometime this is called POTS (Plain Old Telephone Service).

IP phones use VoIP (Voice over IP) technologies to enable phone calls over an
IP network, such as the internet.
IP Phones are connected to a switch just like any other end host.
IP phones have an internal 3-port switch

    1 port is the uplink to the external switch.

    1 port is the downlink to the PC.

    1 port connects internally to the phone itself.

This setup is optimal as it allow the PC and Phone to share one single switch port.
Traffic from the PC passes through the IP phone to the switch.

It is recommended to separate 'voice' traffic (from the IP phone) and 'data' traffic
(from the PC) by placing them in separate VLANs. This can be accomplished using a
_voice VLAN_. Treffic from the PC will be untagged, but traffic from the phone will
be tagged with VLAN ID.
It's quite easy to configure, we need only one additional command to configure the
access port:

    Switch(config)#interface gigabitethernet0/0

    Switch(config-if)#switchport mode access

    Switch(config-if)#switchport access vlan 10 # access, untagged.

    Switch(config-if)#switchport voice vlan 11 # New command.

This will make interface G0/0 having 2 VLANs, but as we configured as access, this
will not result in a trunk mode.

Now if we have 3 PCs and 3 Phones, we will use just 3 switch interfaces.
As any device, phones do need power. Instead of a wall plug let's introduce PoE.

<h4 align="center">PoE</h4>

PoE allows Power Sourcing Equipment (PSE) to provide power to Powered Devices (PD)
over an Ethernet cable. Typically the PSE is a switch and the PDs are IP phones,
IP cameras, wireless access points, etc..
The PSE (the switch) receives power from the outlet, converts it to DC power, and
supplies that DC to the PDs.

Be aware that too much electrical current can damage electrical devices. PoE has
a process to determinate if a connected device needs power, and how much power it
needs. To keep it simple, when a device is connected to a PoE-Enabled port, the PSE
sends low power signals, monitor the responses, and determines how much power the
PD needs.
If the device needs power, the PSE supplies the power to allow the PD to boot.
The PSE continues to monitor the PD and supply the required amount of power.

_Powering policy_ can be configured to prevent a PD from taking too much power.

<strong>power inline police</strong> configures power policing with the default
settings: Disable the port and send a Syslog message if a PD draws too much power.

This is equivalent to:

<strong> power inline police action err-disable</strong>

The interface will be put in an 'error-disabled' state and can be re-enabled with
<strong>shutdown</strong> followed by <strong>no shutdown</strong>.

<strong>power inline police action log</strong> does not shut down the interface
if the PD draws too much power. It will restart the interface and send a Syslog message.

To check power usage and settings use:

    Switch#show power inline police INTERFACE

Here are the standards and it's worth noting that PoE was first ILP, Cisco Inline Power.

      name             Standard#          Watts           Powered Wires Pairs
    ---------------------------------------------------------------------------
    | ILP        |   Cisco Proprietary |     7      |             2           |
    ---------------------------------------------------------------------------
    | PoE Type1  |     802.3af         |     15     |             2           |
    ---------------------------------------------------------------------------
    | PoE+ Type2 |     802.3at         |     30     |             2           |
    ---------------------------------------------------------------------------
    | UPoE Type3 |     802.3bt         |     60     |             4           |
    ---------------------------------------------------------------------------
    | UPoE+ Type4|     802.3bt         |    100     |             4           |
    ---------------------------------------------------------------------------


<h4 align="center">Introduction to QoS</h4>

We talked about PoE and VoIP up to now as QoS often apply to these services as
QoS makes sure to prioritize communication and having a clear audio over the internet.

Voice traffic and data traffic used to use entirely separated networks.
Voice traffic used PSTN and data traffic used the IP network (enterprise WAN, Internet, etc..)
QoS wasn't necessary as the different kinds of traffic didn't compete for bandwidth.

But modern networks are typically _converges networks_ in which IP phones, video traffic,
regular data traffic, etc.. share the same IP network.
This enables cost saving as well as more advanced features for voice and video traffic,
for example integrations with collaboration software as Cisco WebEx, Microsoft Teams, etc..
So now the different kinds of traffic have to compete for bandwidth.

QoS is a set of tools used by networks devices to apply different treatement to
different packets. VoIP and Video traffic are very sensible on delays.

QoS is used to manage the following characteristics of network traffic:

    1 Bandwidth - The overall capacity of the link, measured in bits per second
    QoS tools allow us to reserve a certain amount of a link's bandwidth for
    specific kinds of traffic. For example: 20% voice traffic, 30% for specific
    kinds of data traffic, leaving 50% for all other traffic.

    2 Delay - Two type of delays, One-way delay and two-way delay.
    One-way delay is the amount of time it takes traffic to go from source to destination.
    Two-way delay is the amount of time traffic takes to go from source to destination and return.

    3 Jitter - The variation in one-way delay between packets sent by the same application.
    IP phones have a 'jitter buffer' to provide a fixed delay to audio packets.

    4 Loss - The % of packets sent that do not reach their destination. Can be caused
    by faulty cables or when a device's packet _queues_ get full and the device starts
    discarding packets.

The following standards ar recommended for acceptable interactive audio quality:

<strong>One-way delay:</strong> 150ms or less

<strong>Jitter:</strong> 30ms or less

<strong>Loss:</strong> 1% or less

If these standards are not met, there could be a noticeable reduction in the quality
of the phone call.

<h4 align="center">QoS - Queuing</h4>

If a network device receives messages faster than it can forward them out of the
appropriate interface, the messages are placed in a queue.
By default, queued messages will be forwarded in a First In First Out (FiFo) manner.
if the queue is full new packets will be dropped, this is called _tail drop_.

<strong>Tail drop</strong> is harmful because it can lead to <strong>TCP global synchronization</strong>.

Review of the <strong>TCP sliding window</strong>:

    Hosts using TCP use the 'sliding window' increase/decrease the rate at which they
    send traffic as needed. When a packet is dropped it will be re-transmitted.
    When a drop occurs, the sender will reduce the rate it sends traffic.
    It will then gradually increase the rate again.

When the queue fills up and <strong>tail drop</strong> occurs, all TCP hosts sending
traffic will slow down the rate at which they send traffic.
They will all then increase the rate at which they send traffic, which rapidly leads
to more congestion, dropped packets, and the process repeats again. This will cause
a wave effect of congestion and underutilized network that will cycle non stop.

To visualize it:

    network_________\ tail drop_______\ Global TCP Window_______\ network_________\ Global TCP window____
    congestion      /                 / size decrease           / underutilized   / size increase        \
          |                                                                                               |
          \______________________________________________________________________________________________/


A solution to prevent tail drop and TCP global synchronization is <strong>Random Early Detection</strong>
or RED.

When the amount of traffic in the queue reaches a certain threshold, the
device will start randomly dropping TCP synchronization, in which ALL TCP flows
reduce and the increase the rate of transmission at the same time in waves.

In Standard RED, all kinds of traffic are treated the same.

An improved version, <strong> Weighted Random Early Detection</strong> (WRED), allows
to control which packets are dropped depending on the traffic class.

<h4 align="center">Classification/Marking</h4>

As we understood QoS purpose is to give certain kind of network traffic priority over others
during congestion.

<strong>Classification</strong> organize network traffic (packets) into
traffic classes (categories). This is fundamentals to QoS. To give priority to
a certain type of traffic, we have to identify which types of traffic to give
priority to. But how do we classify traffic? Here are some examples:

    An ACL. Traffic, which is permitted by the ACL will be given certain treatment.
    NBAR (Network Based Application Recognition) performs a _deep packet inspection_,
    looking beyond the Layer 3 and Layer 4 information up to layer 7 to identify
    the specific kind of traffic.

    In the layer 2 and Layer 3 headers there are specific fields used for this purpose.

    The PCP (Priority Code Point) field of the 802.1Q tag (in the Ethernet header)
    can be used to identify high/low priority traffic. (naturally when a dot one Q tag is used)

    The DSCP (Differentiated Services Code Point) field in the IP header can also
    be used to identify high/low priority traffic.

Let's have a look to the Ethernet Header again:

    -----------------------------------------------------------------
    |           |     |             |        |        |             |
    | Preamble  | SFD | Destination | Source | 802.1Q | Type/Length |
    |           |     |             |        |        |             |
    -----------------------------------------------------------------

The PCP field is right in the "Dot 1 Q tag", a closer look to the format:

    --------------------------------------
    | 16 bits | 3 bits | 1 bit | 12 bits |
    --------------------------------------
    |         |         TCI              |  
    |  TPID   |---------------------------
    |         | PCP    | DEI  |   VID    |
    --------------------------------------



<h4 align="center">Queuing/Congestion Management</h4>




<h4 align="center">Shaping/Policing</h4>
