<h2 align="center">DHCP Snooping</h2>


<h4 align="center">What is DHCP Snooping</h4>

DHCP snooping is a security feature of switches that is used to filter DHCP messages
received on _untrusted_ ports.
DHCP snooping only filters DHCP messages, non DHCP messages aren't affected.
All ports are _untrusted_ by default. Usually <strong>uplink</strong> ports are
configured as _trusted_ ports, and <strong>downlink</strong> ports remain _untrusted_.


<h4 align="center">How does it work?</h4>

When DHCP Snooping filters messages, it differentiates between <strong>DHCP Server</strong> messages
and <strong>DHCP Client</strong> messages.
DHCP server messages received on an _untrusted_ port will always be discarded with no
further check.
DHCP client messages will be inspected and the switch will decide to forward or discard the messages.

Messages sent by <strong>DHCP Servers:</strong>

- OFFER
- ACK
- NAK opposite of ACK, used to decline a client's REQUEST

Messages sent by <strong>DHCP Clients:</strong>

- DISCOVER
- REQUEST
- RELEASE
- DECLINE

If a DHCP message is received on a <strong>trusted port</strong>, forward it as normal without inspection.

If a DHCP message is received on an <strong>untrusted port</strong>, inspect it and act as follow:

- If it is a <strong>DHCP Server</strong> message, discard it.
- If it is a <strong>DHCP Client</strong> message, perform the following checks:

      DISCOVER/REQUEST messages: Check if the frame's source MAC address and the DHCP message's
      CHADDR fields match. Match = forward, mismatch = discard.

      RELEASE/DECLINE messages: Check if the packet's source IP address and the receiving interface
      match the entry in the _DHCP Snooping Binding Table_. Match = forward, mismatch = discard.

A DHCP Snooping Binding Table is a list of successful leases of IP addresses from a server.

<h4 align="center">What attacks does it prevent?</h4>

An example of a DHCP-based attack is a <strong>DHCP starvation</strong> attack. An
attacker uses spoofed MAC addresses to flood DHCP Discover messages. If successful the target
server's DHCP pool becomes full, resulting in a denial-of-service to other devices.

Similar to ARP Poisoning, <strong>DHCP Poisoning</strong> can be used to perform a Man-in-the-Middle attack.
A _spurious DHCP server_ or illegitimate DHCP server replies to client's DHCP Discovery
messages and assigns them IP addresses, but makes the clients use the spurious server's IP
as the default gateway.
This will cause the client to send traffic to the attacker instead of the legitimate default
gateway. The attacker can then examine/modify the traffic before forwarding it to the
legitimate default getaway.

<h4 align="center">DHCP Snooping configuration</h4>

To enable DHCP Snooping we use the command:

    Switch1(config)#ip dhcp snooping         # enables snooping globally

    Switch1(config)#ip dhcp snooping vlan 1  # to enable it on vlan 1, change the value as needed

    Switch1(config)#no ip dhcp snooping information option # Optional, will explain later

    Switch1(config)#interface g0/0 # selecting the uplink interface

    Switch1(config-if)#ip dhcp snooping trust # setting the interface as trusted.

We can check the DHCP Snooping binding table with this command:

Switch1#show ip dhcp snooping binding
