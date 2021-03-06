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
Originally, standard ACLs could use only 1 to 99, but later range 1300-1999 was added.

Here are the official ranges, other than the IP ranges not to be memorized for the CCNA, but it's good to have an idea:

- Standard IP&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&ensp;1-99 and 1300-1999
- Extended IP&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&ensp;100-199 and 2000-2699
- Ethernet type code&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&ensp;200-299
- Ethernet address&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&ensp;700-799
- Transparent Bridging&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;200-299&emsp;&emsp;# Protocol type
- Transparent Bridging&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp;&nbsp;&nbsp;700-799&emsp;&emsp;# Vendor Code
- Extended Transparent Bridging&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp;1100-1199
- DECnet and extended DECnet&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp;&nbsp;300-399
- Xerox Network Systems (XNS)&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp;&nbsp;400-499
- Extended XNS&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp;&nbsp;500-599
- Apple Talk&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp;&nbsp;600-699
- Source-route Bridging&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp;&nbsp;200-299&emsp;&emsp;# Protocol type
- Source-route Bridging&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp;&nbsp;700-799&emsp;&emsp;# Vendor Code
- Internetwork Packet Exchanger (IPX)&emsp;&emsp;&emsp;800-899
- Extended IPX &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;900-999
- IPX Service Advertising Protocol (SAP)&emsp;&emsp;1000-1099

The basic command to configure a standard numbered ACL is:

    Router1(config)# access-list  NUMBER { DENY | PERMIT } IP WILDCARD-MASK

As an example:

    Router1(config)# access-list 1 deny 1.1.1.1 0.0.0.0

This will deny a single host. When you specify a /32 mask, you actually do not need to specify the wildcard-mask. To do the same we can use the _host_ keyword to do the same:

    Router1(config)# access-list 1 deny host 1.1.1.1

Ok, this command is denying access the host 1.1.1.1, but if we leave the ACL like this, all traffic will be denied due to the implicit deny, so we should add an extra rule like this as an example:

    Router1(config)# access-list 1 permit any

or

    Router1(config)# access-list 1 permit 0.0.0.0 255.255.255.255

This access list will deny host 1.1.1.1, but will permit all other. Naturally this is just an example.

ACLs are pretty flexible, one entry can be configured in many ways. There is as well the possibility to enter remarks:

    Router1(config)# access-list 1 remark ##TEXT FOR THE REMARK

This has no impact to the ACL, but helps reminding the purpose of the ACL or just to put a note about the configuration.

We can have a look to the ACL with this command:

    Router1(config)#do show access-lists

Here we can see all entry in their order, but not the remark. This will show all kind of ACLs, but as for now we are focusing on IP access lists, so here is another command:

    Router1(config)# do show IP access-lists

And we can check in the running config for access-lists by doing a pipe on the running-config:

    Router1(config)# do show running-config |include access-list

This time we will have the remark present as well.

Once all this is ready, we have to apply it to the interface of our choice, for this we select the interface and we run the following command:

    Router1(config)# ip access-group NUMBER { in | out }

So let's do an example with our network described above and here below are the requirements:
- PC1 can access 192.168.2.0/24
- Other PCs in 192.168.1.0/24 can't access 192.168.2.0/24

The configuration would be:

    Router1(config)#access-list 1 permit 192.168.1.1

    Router1(config)#access-list 1 deny 192.168.1.0 0.0.0.255

    Router1(config)#acess-list 1 permit any

    Router1(config)#interface g0/2

    Router1(config-if)#ip access-group 1 out

This bring us to a rule on ACLs: <ins>Standard ACLs should be applied as close to the destination as possible.</ins> \
If we do not do this way, we might block more traffic than intended.


<h4 align="center">Standard named ACLs</h4>

Standard Named ACLs work just like Numbered ACLs, but instead of a number we can use names to identify ACLs. \
Standard named ACLs are configured by entering 'standard named ACL config mode', and then configuring each entry within that config mode.

This is how we enter the config mode:

    Router1(config)# ip access-list standard ACL-NAME

<b>Note that for named ACLs we use IP in front of the command.</b>

Once in the config mode we can enter the ACEs :

    Router1(config-std-nacl)# [ENTRY-NUMBER] { deny | permit } IP WILDCARD-MASK

Note that in the named ACL command we can specify the _entry number_ of the rule we are configuring. If we do not specify it will automatically increase by a value of 10.

Let's now use our network do configure R2, here are the requirements:
- PCs in 192.168.1.0/24 can't access 10.0.2.0/24
- PC3 can't access 10.0.1.0/24
- Other PCs in 192.168.2.0/24 can access 10.0.1.0/24
- PC1 can access 10.0.1.0/24
- Other PCs in 192.168.1.0/24 can't access 10.0.1.0/24

So, we will configure one ACL to control access to 10.0.2.0/24 and apply it outbound on R2's G0/2. \
Then we will configure an ACL to control access to 10.0.1.0/24 and apply it outbound on R2's G0/1

Here is my config:

    R2(config)#ip access-list standard NAME1

    R2(config-std-nacl)#deny 192.168.1.0 0.0.0.255

    R2(config-std-nacl)#permit any

    R2(config-std-nacl)#interface G0/2

    R2(config-if)#ip access-group NAME1 out

and now for G0/1:

    R2(config-if)#ip access-list standard NAME2

    R2(config-std-nacl)#deny 192.168.2.1

    R2(config-std-nacl)#permit 192.168.2.0 0.0.0.255

    R2(config-std-nacl)#permit 192.168.1.1

    R2(config-std-nacl)#deny 192.168.1.0 0.0.0.255

    R2(config-std-nacl)#permit any

    R2(config-std-nacl)#interface G0/1

    R2(config-if)#ip access-group NAME2 out

ACLs configurations are flexible, this is just one of the ways to do it.

If we now check out entry with _show ip access-lists_ we will have this output:

    Standard IP access list NAME2
      30 permit 192.168.1.1
      10 deny   192.168.2.1
      20 permit 192.168.2.0, wildcard bits 0.0.0.255
      40 deny   192.168.1.0, wildcard bits 0.0.0.255
      50 permit any

    Standard IP access list NAME1
      10 deny 192.168.1.0, wildcard bits 0.0.0.255
      20 permit any

As we can see the sequence number matches the entries I did, but the order is not as expected. This is something out of the CCNA scope and has to do with Cisco IOS way of working. Good to noting:
- The router may re-order the /32 entries.
- This improves the efficiency of processing the ACL.
- it DOES NOT change the effect of the ACL.
- This applies to standard names and standard numbered ACLs.

<h4 align="center">Another way to configure numbered ACLs</h4>

We learned that numbered ACLs are configured in global config mode and that named ACLs are configured with subcommands in a separate config mode. \
However, in modern IOS you can also configure numbered ACLs in the exact same way as named ACLs. This is just a different way of configuring numbered ACLs. However, in the running-config the ACL will display as if it was configured using the traditional method.

<h4 align="center">Editing ACLs</h4>

The reason behind this is that there are few advantages on using the named ACL config mode, here are some:

- We can easily delete individual entries in the ACL with <b>no</b> _entry-number_.
- We can insert new entries in between other entries by specifying the sequence number.
- We can resequence the ACL.

When configuring/editing numbered ACLs from global config mode, we can't delete individual entries, we can only delete the entire ACL!

To resequence an ACL we sue this command:

    ip access-list resequence ACL-ID  STARTING-SEQ-NUM  INCREMENT

What's the reason to resequence an ACL? Well here is an example:

![Bad ACL](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/bad_acl.png?raw=true "Bad ACL")

This ACL works, but will make it impossible to insert a new rule between existing ones. \
By resequencing with our command (shown in the picture) we will be able to give space to the ACEs and bee more flexible:

![Good ACL](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/good_acl.png?raw=true "Good ACL")

Let's comment the resequence command:

    R1(config)#ip access-list resequence 1 10 10

- first we have the main command: <b>ip access-list resequence</b>
- then we specify the acl number of <b>1</b>
- then we tell the ACL to start from 10
- and we tell the ACL to increment by 10 for each ACE.

Now it's easier to add or insert new ACEs.

<h3 align="center">Extended ACLs</h3>

Extended ACLs function mostly the same as standard ACLs. \
They can be numbered or named, just like standard ACLs, as seen before, Extended Numbered ACLs use the following ranges: 100-199, 2000-2699.
They are processed from top to bottom as Standard ACLs. \
However, they can match traffic based on more parameters, so they are more precise (and complex) than standard ACLs. For example we can do matching using <b> Layer 4 protocol/port, source address/destination address.</b>

To configure an extended Numbered ACL we use the following command:

    R1(config)# access-list NUMBER [ PERMIT | DENY ] PROTOCOL SRC-IP DEST-IP

And for an extended named ACL:

    R1(config)# access-list extended [ NAME | NUMBER ]

    R1(config-ext-nacl)#[SEQ-NUM] [ PERMIT | DENY ] PROTOCOL SRC-IP DEST-IP

Just like Standard ACLs, Extended number ACLs can be configured in Named ACLs.

<b><ins>Matching Protocols:</ins></b>

Here are the protocols we can use in the ACE matching, I entered the config mode and then typed deny and question mark to bring up the options:

![protocols](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/protocols.png?raw=true "protocols")

Note the long list and note the first one, we can actually select the protocol by the protocol number (if you remember them all :D). We can as well just do it by name, easier.

<b><ins>Matching Source/Destination IP address:</ins></b>

Now let's see how we can match the source/destination IP address, from the prevuous example I selected TCP and added the question mark:

![Source/Destination](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/src_dst.png?raw=true "Source/Destination")

Note that in extended ACLs, to specify a /32 source or destination we have to use the <b>host</b> option or specify the wildcard mask. You can't just write the address without either of those like in standard ACLs.

Under we have the type of destinations, as you can see it's very complete. I selected the first one, destination address and then I entered the wildcard bits.
So the entered ACE of _deny tcp any 10.0.0.0 0.0.0.255_ will deny all packets that encapsulate a TCP segment, from any source, to destination 10.0.0.0/24.

Here are few examples:

- Allow all traffic:

    R1(config-ext-nacl)# permit ip any any

- Prevent 10.0.0.0/16 from sending UDP traffic to 192.168.1.1/32

    R1(config-ext-nacl)# deny udp 10.0.0.0 0.0.255.255 host 192.168.1.1

- Prevent 172.16.1.1/32 from pinging hosts in 192.168.0.0/24

    R1(config-ext-nacl)# deny icmp host 172.16.1.1 192.168.0.0 0.0.0.255

<b><ins>Matching the TCP/UDP port numbers:</ins></b>

When we specify matching TCP/UDP, we can optionally specify the source or/and destination port number to match. If we don't all ports will match.

For example, if we want to match port 80 we would do:

    R1(config-ext-nacl)# deny | permit tcp SRC-IP eq SRC-PORT-NUM DEST-IP eq DST-PORT-NUM

In this case we use <b>eq</b> as it means equal, but as seen in the picture above, we have many options. Here is a reminder:

- eq 80         = equal to port 80
- gt 80         = greater than 80 ( 81 and greater)
- lt 80         = less than 80 (79 and less)
- neq 80        = NOT 80
- range 80 100  = from port 80 to port 100

As for Protocols, we can bring up a list inside the CLI where the first option is to manually specify the port number.

![ports](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/ports.png?raw=true "Ports")

After the destination IP address and/or destination port numbers, there are many more options you can use to match (not necessary for the CCNA). \
Some examples:

- <b>ack</b>: match the TCP ACK flag
- <b>fin</b>: match the TCP FIN flag
- <b>syn</b>: match the TCP SYN flag
- <b>ttl</b>: match packets with a specific TTL value
- <b>dscp</b>: match packets with a specific DSCP value

If you specify the protocol, source IP, source port, destination IP, destination port, etc, a packet MUST match ALL of those values to match the ACL entry. \
Even if it matches all except one of the parameters, the packet won't match that entry of the ACL.

As before, here are some examples:

- Allow traffic from 10.0.0.0/16 to access the server at 2.2.2.2/32 using https

    R1(config-ext-nacl)#permit tcp 10.0.0.0 0.0.255.255 host 2.2.2.2 eq 443

- Prevent all hosts using UDP port numbers from 20000 to 30000 from accessing the server at 3.3.3.3/32

    R1(config-ext-nacl)#deny udp any range 20000 30000 host 3.3.3.3

- Allow hosts in 172.16.1.0/24 using a TCP source port greater than 9999 to access all TCP ports on server 4.4.4.4/32 except port 32

    R1(config-ext-nacl)#permit tcp 172.16.1.0 0.0.0.255 gt 9999 host 4.4.4.4 neq 23

Now let's have some examples using our network from the beginning of the chapter:

Requirements:

- Hosts in 192.168.1.0/24 can't use HTTPS to access SRV1
- Hosts in 192.168.2.0/24 can't access 10.0.2.0/24
- None of the hosts in 192.168.1.0/24 or 192.168.2.0/24 can ping 10.0.1.0/24 or 10.0.2.0/24

Here how it's done, there is a little difference from the example on Standard ACLs:

First requirement:

    R1(config)#ip access-list extended HTTP_SRV1

    R1(config-ext-nacl)#deny tcp 192.168.1.0 0.0.0.255 host 10.0.1.100 eq 443

    R1(config-ext-nacl)#permit ip any any

    R1(config-ext-nacl)#interface g0/1

    R1(config-if)#ip access-group HTTP_SRV1 in

As you noticed, we applied the ACL close to the source and not to the destination as explained before, the reason is that the rule for the Extended ACLs is inverted. \
Extended ACLs should be applies <ins>as close as possible to the source</ins>, to limit how far the packets travel in the network before being denied. \
Standard ACLs are less specific, so if they are applied close to the source there is a risk of blocking more traffic than intended.

Second requirement:

    R1(config)#ip access-list extended BLOCK_SRV2

    R1(config-ext-nacl)#deny ip 192.168.2.0 0.0.0.255 10.0.2.0 0.0.0.255

    R1(config-ext-nacl)#permit ip any any

    R1(config-ext-nacl)#interface g0/2

    R1(config-if)#ip access-group BLOCK_SRV2 in

Third requirement:

    R1(config)#ip access-list extended NO_PING

    R1(config-ext-nacl)#deny ICMP 192.168.1.0 0.0.0.255 10.0.1.0 0.0.0.255

    R1(config-ext-nacl)#deny ICMP 192.168.1.0 0.0.0.255 10.0.2.0 0.0.0.255

    R1(config-ext-nacl)#deny ICMP 192.168.2.0 0.0.0.255 10.0.1.0 0.0.0.255

    R1(config-ext-nacl)#deny ICMP 192.168.2.0 0.0.0.255 10.0.2.0 0.0.0.255

    R1(config-ext-nacl)#permit ip any any

    R1(config-ext-nacl)#interface g0/0

    R1(config-if)#ip access-group NO_PING out

It's worth noting that if this was a full scenario, we could omit the _deny ICMP 192.168.2.0 0.0.0.255 10.0.2.0 0.0.0.255_ as we already denied all access to the server from that IP on the second requirement.

This in not the most effective solution, but it shows how the extended ACLs are configured.

lastly, to check inbound and outbound ACLs we can run this command:

    R1#show ip INTRERFACE

This will summarize the information and we will be seen the applied access lists and their directions.
