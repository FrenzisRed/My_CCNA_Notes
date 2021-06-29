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
<ins>DHCP snooping binding table</ins>.


<h4 align="center">What attacks does it prevent</h4>

ARP poisoning attack is similar to DHCP poisoning. It involves an attacker manipulating
target's ARP tables so traffic is sent to the attacker. \
To do this, the attacker can send gratuitous ARP messages using another device's IP address.
Other devices in the network will receive the GARP and update their ARP tables, causing
them to send traffic to the attacker instead of the legitimate destination.



<h4 align="center">DAI configuration</h4>
