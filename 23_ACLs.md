<h2 align="center">Access Control List. Standard & Extended</h2>


<h4 align="center">What are ACLs?</h4>

ACLs (Access Control List) have multiple uses. \
Here we will focus on ACLs from a security perspective, later you will see other type of uses. \
ACLs function as a packet filter, instructing the router to permit or discard specific traffic. \
ACLs can filter traffic based on source/destination IP addresses, source/destination Layer 4 ports, etc..

Let's use this network for our examples:

![ACLs](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/ACLs1.png?raw=true "ACLs")

CASE REQUIREMENTS:
- Hosts in 192.168.1.0/24 ca access the 10.0.1.0/24 network
- Hosts in 192.168.2.0/24 cannot access the 10.0.1.0/24 network

How ca we use ACLs to achieve this? Well, first of all ACLs are configured globally on the ROUTER. (Global config mode) \
Reason why we simplified the switches in the network diagram. \
ACLs are an ordered sequence of ACEs (Access Control Entries). \
When the router checks a packet against the ACL, it process the ACEs in order, from top to bottom. \
If the packet matches one of the ACEs in the ACL, the router takes the action and stops processing the ACL. All entries below the matching entry will be ignored.

In this case we could create an ACL like this:
- if source IP = 192.168.1.0/24 then permit
- if source IP = 192.168.2.0/24 then deny
- if source IP = any, then permit

The order of the entries is very important. \
Configuring the ACL in global config mode will not make the ACL take effect. The ACL must be applied to an interface.
ACLs are applied either inbound or outbound.

In our example, the best location to apply the ACL is on R2 G0/1 outbound. This will block all connections from 192.168.2.0/24 to SRV1, but will not block the rest of the traffic, for example going to SRV2 or to network 192.168.1.0/24.

Note that a maximum of one ACL can be applied to a single interface _per direction_, so one inbound and one outbound. \
If we apply a second ACL in the same direction of the previous one, it will replace it.

<b>Implicit deny:</b>
What happens if a packet doesn't match any of the entries in an ACL? Well, there is an invisible rule at the end of a ACL ACE. The invisible rule, or implicit rules tell the router to deny the packet. It's like if at the end of the list there was a: if source IP = any, then deny.

<h4 align="center">ACLs logic</h4>

<h4 align="center">ACLs types</h4>

<h4 align="center">Standard numbered ACLs</h4>

<h4 align="center">Standard named ACLs</h4>
