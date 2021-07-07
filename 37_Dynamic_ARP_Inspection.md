<h2 align="center">DAI = Dynamic ARP Inspection</h2>


<h4 align="center">What is Dynamic ARP Inspection</h4>

DAI is a security feature of switches that is used to filter ARP messages received on
_untrusted_ ports. \
DAI only filters ARP messages. Non-ARP messages aren't affected. \
All ports are _untrusted_ by default. Typically, all ports connected to other networks
devices (switches, routers) should be configured as <b>trusted</b>, while interfaces connected
to end hosts should remain <b>untrusted</b>.

let's use this network, I'll name the ports An:

    ----------                ----------          ---------            -------
    | Router |--------------A2| Switch1|A3------A4|Switch2|A5----------| PCs |
    ----------                -----A7---          -----A6--            -------
                                   |                    |______________| PC2 |
                                   |                    |              -------              
                                   |                    |______________| PC3 |
                              ----------                               -------
                              | Server |
                              ----------
In this scenario the typical setup should have A2,A3,A4 as trusted and A7,A5,A6 untrusted.

The functioning is the same as DHCP Snooping.


<h4 align="center">How does it work</h4>

DAI inspect the <ins>sender MAC</ins> and <ins>sender IP</ins> fields of ARP messages
received on <b>untrusted</b> ports and checks that there is a matching in the
<ins>DHCP snooping binding table</ins>. \
If there is a matching entry, the ARP message is forwarded normally. \
If the isn't a matching entry, the ARP message is discarded. \
DAI doesn't inspect messages received on <b>trusted</b> ports. They are forwarded as normal.
<b>ARP ACLs</b> can be manually configured to map IP addresses/Mac addresses for DAI to check. \
This is useful for hosts that do not use DHCP, as if they do not use DHCP they
will NOT have en entry in the DHCP snooping table and all their ARP messages will be dropped. \
DAI can be configured to perform more in-depth checks also, but these are optional. \
Like DHCP snooping, DAI also support rate-limiting to prevent attackers from overwhelming the switch
with ARP messages. \
DHCP snooping and DAI both require work from the switch's CPU, so even id the attacker's messages
are blocked, they can still overload the switch CPU with messages. Rate-limiting is
a useful mitigation technique.

Let's put a bit more info in the optional checks. Additional checks can be issued with the inspection
validate command:

    Switch2(config)#ip arp inspection validate OPTIONS

the options are:

- <b>dst-mac</b>: Enables validation of the destination MAC address in the Ethernet header
  against the target MAC address in the ARP body for ARP responses. The device classifies packets with different MAC addresses as invalid and drops them.
- <b>ip</b>     : Enables validation of the ARP body for invalid and unexpected IP addresses. Addresses include 0.0.0.0, 255.255.255.255, and all IP multicast addresses. The device checks the sender IP addresses in all ARP requests and responses and checks the target IP addresses only in ARP responses.
- <b>src-mac</b>: Enables validation of source MAC address in the Ethernet header against the sender MAC address in the ARP body for ARP requests and responses. The devices classifies packets with different MAC addresses as invalid and drops them.

These checks are an addition to the normal checks and if enabled a message has to pass all checks or it will be dropped.

<h4 align="center">What attacks does it prevent</h4>

ARP poisoning attack is similar to DHCP poisoning. It involves an attacker manipulating
target's ARP tables so traffic is sent to the attacker. \
To do this, the attacker can send gratuitous ARP messages using another device's IP address.
Other devices in the network will receive the GARP and update their ARP tables, causing
them to send traffic to the attacker instead of the legitimate destination.



<h4 align="center">DAI configuration</h4>

Let's have a look on how to enable it and configure it:

    Switch2(config)#ip arp inspection vlan VLANNUMBER

As from our network above I'll set the trusted ports:

    Switch2(config)#interface range A2-3

    Switch2(config-if-range)#ip arp inspection trust

Same configuration for Switch1:

    Switch1(config)#interface A4

    Switch1(config-if)#ip arp inspection trust

Worth noting that DHCP snooping requires two commands to enable it, DAI only one.

    Switch2(config)#ip dhcp snooping           
                                              VS    Switch2(config)#ip arp inspection vlan VLANNUMBER
    Switch2(config)#ip dhcp snooping vlan 1

Once setup, we can check with our favorite show command:

    Switch2#show ip arp inspection interfaces

There we can see the list of interfaces, the trust state, the Rate (pps) and Burst Interval.

Worth noting that DAI rate limiting is enabled on untrusted ports by default with a rate of 15
packets per second. \
It is disables on trusted ports by default. DHCP snooping rate limiting is disabled on all interfaces
by default. \
DHCP snooping rate limiting is configured like this: x packets <ins>per second</ins>. \
The DAI <b>Burst interval</b> allows us to configure rate limiting like this: x packet <ins> per y seconds</ins>.

Here is how to configure DAI rate limiting:

    Switch2(config)#interface range A2-3

    Switch2(config-if-range)#ip arp inspection limit rate 25 burst interval 2 # 25 packets on 2 seconds

Burst interval is optional, if not specified it gives the value of 1 second.

If ARP messagesare received faster than the specified rate, the interface will be
err-disabled. It can be re-enabled in the two now well known ways:

    Manually with a shutdown / no shutdown command

or

    errdisable recovery cause arp-inspection

To add the additional checks in the DAI here is the command:

    Switch2(config)#ip arp inspection validate OPTION

Be aware that if we enable one after the other one, the newly chosen option will overwrite the previous.
If we want to enable all we should do the command like this:

    Switch2(config)#ip arp inspection validate ip src-mac dst-mac

or just select in one line the options you want to enable.

<h4 align="center">Note on ARP ACLs</h4>

From our network diagram let's say that the server do not use DHCP as it has a static IP.
If it tries to send an ARP message it will be dropped as its IP address in not present in the DHCP binding table. \
To fix this we need to configure a ARP ACL to permit server. Here is the commands:

    Switch2(config)#arp access-list ARP-ACL-1

    Switch2(config-arp-nacl)#permit ip host SERVERIP mac host MACADDRESS

Now we have to apply it to make it effective:

    Switch2(config)ip arp inspection filter ARP-ACL-1 vlan 1

Now the ARP requests will be forwarded even if SERVER does not have an entry in the
DHCP snooping table.

One last useful command to check statuses:

    Switch2#show ip arp inspection

It gives a summery of the DAI configuration as well statistics about how many ARP messages
have been forwarded or dropped.
