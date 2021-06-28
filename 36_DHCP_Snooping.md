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
