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
    

<h4 align="center">Intro to QoS</h4>
