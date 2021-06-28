<h2 align="center">DHCP Snooping</h2>


<h4 align="center">What is DHCP Snooping</h4>

DHCP snooping is a security feature of switches that is used to filter DHCP messages
received on _untrusted_ ports.
DHCP snooping only filters DHCP messages, non DHCP messages aren't affected.
All ports are _untrusted_ by default. Usually <strong>uplink</strong> ports are
configured as _trusted_ ports, and <strong>downlink</strong> ports remain _untrusted_.



<h4 align="center">How does it work?</h4>


<h4 align="center">What attacks does it prevent?</h4>

An example of a DHCP-based attack is a <strong>DHCP starvation</strong> attack. An
attacker uses spoofed MAC addresses to flood DHCP Discover messages. If successful the target
server's DHCP pool becomes full, resulting in a denial-of-service to other devices.

Similar to ARP Poisoning, DHCP Poisoning can be used to perform a Man-in-the-Middle attack.
A _spurious DHCP server_ or illegitimate DHCP server replies to client's DHCP Discovery
messages and assigns them IP addresses, but makes the clients use the spurious server's IP
as the default gateway.
This will cause the client to send traffic to the attacker instead of the legitimate default
gateway. The attacker can then examine/modify the traffic before forwarding it to the
legitimate default getaway.

<h4 align="center">DHCP Snooping configuration</h4>
