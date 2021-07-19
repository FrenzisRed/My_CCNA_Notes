<h2 align="center">Access Control List. Standard & Extended</h2>


<h4 align="center">What are ACLs?</h4>

ACLs (Access Control List) have multiple uses. \
Here we will focus on ACLs from a security perspective, later you will see other type of uses. \
ACLs function as a packet filter, instructing the router to permit or discard specific traffic. \
ACLs can filter traffic based on source/destination IP addresses, source/destination Layer 4 ports, etc..


<h4 align="center">ACLs logic</h4>

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
What happens if a packet doesn't match any of the entries in an ACL? Well, there is an invisible rule at the end of a ACL ACE. The invisible rule, or implicit rules tell the router to deny the packet. It's like if at the end of the list there was a: <b>if source IP = any, then deny.</b>
So understand that there is an implicit deny at the end of ALL ACLs. The implicit deny tell the router to deny all traffic that doesn't match any of the configured entries in the ACL. Be aware of it not to deny traffic that was supposed to be permitted.


<h4 align="center">ACLs types</h4>

Standard ACLs: Match based on <b>Source IP address</b> _only_.
- Standard Numbered ACLs, identified with a number like 1, 2, etc..
- Standard Named ACLs, identified with a name.

Extended ACLs: Match based on <b>Source/Destination IP, Source/Destination port,</b> etc..
- Extended Numbered ACLs
- Extended Named ACLs

<h4 align="center">Standard numbered ACLs</h4>

Standard ACLs match traffic based only on the source IP address of the packet, the router does not check the destination IP, the source Layer 4 port, the destination port, etc.. \
As explained above, numbered ACLs are identified with a number, for example ACL 1, ACL 2, etc.. \
Different type of ACLs have a different range of numbers that can be used, Standard ACLs can use range between 1-99 and 1300-1999. \
Originally, standard ACLs could use only 1 to 99, but later range 1300-1999 was added. \
Here are the official ranges, other than the IP ranges not to be memorized for the CCNA, but it's good to have an idea:

- Standard IP&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;1-99 and 1300-1999
- Extended IP&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;100-199 and 2000-2699
- Ethernet type code&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;200-299
- Ethernet address&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;700-799
- Transparent Bridging&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;200-299&emsp;&emsp;# Protocol type
- Transparent Bridging&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;700-799&emsp;&emsp;# Vendor Code
- Extended Transparent Bridging&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;1100-1199
- DECnet and extended DECnet&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;300-399
- Xerox Network Systems (XNS)&emsp;&emsp;&emsp;&emsp;&emsp;400-499
- Extended XNS&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; 500-599
- Apple Talk&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; 600-699
- Source-route Bridging&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; 200-299&emsp;&emsp;# Protocol type
- Source-route Bridging&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; 700-799&emsp;&emsp;# Vendor Code
- Internetwork Packet Exchanger (IPX)&emsp;&emsp;&emsp;800-899
- Extended IPX &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;900-999
- IPX Service Advertising Protocol (SAP)&emsp;&emsp;1000-1099

The basic command to configure a standard numbered ACL is:

    Router1(config)# access-list _number_ {<b>deny|permit</b>} _ip wildcard-mask_
<h4 align="center">Standard named ACLs</h4>
