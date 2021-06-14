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

<h4 align="center">PoE</h4>


<h4 align="center">Intro to QoS</h4>
