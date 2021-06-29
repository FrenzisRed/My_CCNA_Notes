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

DHCP snooping can limit the rate at which DHCP messages are allowed to enter an interface.
If the rate of DHCP messages crosses the configured limit, the interface is err-disabled.
Like with Port Security, the interface can be manually re-enabled, or automatically
re-enabled with errdisable recovery.

<h4 align="center">DHCP Snooping configuration</h4>

In the network below, the uplink interfaces are marked as G0/0:

----------                ----------          ---------            -------
| Router |G0/1--------G0/0| Switch1|.1----G0/0|Switch2|------------| PCs |
----------                ----------          ---------            -------

To enable DHCP Snooping we use the following commands on Switch1 and 2:

    Switch2(config)#ip dhcp snooping         # enables snooping globally

    Switch2(config)#ip dhcp snooping vlan 1  # to enable it on vlan 1, change the value as needed

    Switch2(config)#no ip dhcp snooping information option # Optional, will explain later

    Switch2(config)#interface g0/0 # selecting the uplink interface

    Switch2(config-if)#ip dhcp snooping trust # setting the interface as trusted.

We can check the DHCP Snooping binding table with this command:

    Switch1#show ip dhcp snooping binding

The result should look similar to this:

MacAddress           IpAddress         Lease(sec)    Type           VLAN      Interface
-----------------    ---------------   ---------     ---------      -----     -----------------
0C:29:2F:18:79:00    192.168.100.10    86294         dhcp-snooping    1       GigabitEthernet0/3
0C:29:2F:90:91:00    192.168.100.11    86392         dhcp-snooping    1       GigabitEthernet0/2
0C:29:2F:27:E1:00    192.168.100.12    85224         dhcp-snooping    1       GigabitEthernet0/1
Total number of bindings: 3

This table logs all the information about the leased IPs and it's the table that
is checked to make sure the IP address/interface ID match as described above for the
RELEASE/DECLINE messages. This prevent attackers from sending messages in behalf of
other clients on the network causing the DHCP server that they do not need their IP  
anymore.

Here is how to configure DHCP rate limiting:

    Switch2(config)#interface range g0/1-3

    Switch2(config-if)#ip dhcp snooping limit rate VALUEFORSECOND

As for Port Security, we can manually re enable the interface with shutdown and no shutdown, but
let's see how to configure the errdisable:

    Switch2(config)#errdisable recovery cause dhcp-rate-limit 
