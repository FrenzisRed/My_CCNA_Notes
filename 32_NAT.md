<h2 align="center">NAT = Network Address Translation</h2>



<h4 align="center">Private IPv4 Addresses</h4>

As seen before IPv4 doesn't provide enough addresses for all devices that need
an IP address in the modern world.
The long-term solution is to switch to IPv6.

There are three main short-term solutions:

    1) CIDR
    2)Private IPv4 Addresses
    3)NAT

RFC 1918 dpecifies the following IPv4 addresses ranges as private:

    10.0.0.0/8 (10.0.0.0 to 10.255.255.255)

    172.16.0.0/12 (172.16.0.0 to 172.31.255.255)

    192.168.0.0/16 (192.168.0.0 to 192.168.255.255)

We have one range per class (in order of listing class A class B and class C), but
remember, we all use CIDR, classless IP addresses, Classes are a thing of the past.

We are free to use these addresses in our networks, They don't have to be
globally unique as private IP addresses cannot be used over the internet and
that's where NAT comes in.
In a LAN like our house or office, the networks works with private IP address
that cannot be used over the internet. The router that has the interface going to
the internet is the one having the Public IP that all systems on the LAN will
use to connect to the outside.


<h4 align="center">Intro to NAT</h4>

Network Address translation is used to modify the source and/or destination IP
address of packets.
There are various reasons to use NAT, but the most common reason is to allow hosts
with private IP addresses to communicate with other hosts over the internet.

For the CCNA we have to understand <strong>source NAT</strong> and how to configure
it on Cisco routers.


    -------              ----------        ----------            --------
    | PC1 |.167--------.1| Router1|.1----.2|internet|------------| SRV1 |
    -------              ----------        ----------            --------

PC1 = 192.168.0.167
SRV1 = 8.8.8.8
Router1 external int = 203.0.113.1

Let's talk about source NAT with the network topology shown above:
PC1's IP address is 192.168.0.167 and wants to communicate with server1 at 8.8.8.8
So it creates a packet with source IP 192.168.0.167 and destination 8.8.8.8
It sends the packet to its default getaway, Router1. This is where the NAT happens.
Router1 translate the source IP address from 192.168.0.167 to 203.0.113.1, the IP
of its external interface. That's why it's called 'source' NAT, because it translate
the source IP address. This is just one type of NAT, more to follow, but let's
finish this example first.
Router1 then sends the packet out to the internet and it arrives at its destination,
8.8.8.8. Now the server will send a reply.
the source is 8.8.8.8 and the destination is 203.0.113.1. It sends the packet to
the Router1, which then reverse the translation back to 192.168.0.167.

Let's see now <strong>Static NAT</strong>:

Static NAT involves statically configuring one-to-one mappings of private IP
addresses to public IP addresses. It doesn' have to be public or private, we can
NAT any address to any other address, but to keep things simple let's think of
it as mapping one private IP to one public IP. An _inside local_ IP address is
mapped to an _inside global_ IP address.
<strong>Inside Local</strong> = The IP address of the _inside_ host, from the perspective
of the local network.
<strong>Inside Global</strong> = The IP address of the _inside_ host, from the perspective
of _outside_ hosts. (the IP address of the inside host <strong>after NAT</strong>), usually a
public address.

To visualize it a bit better let's do a second network:

    -------              ----------        ----------            --------
    | PC1 |.167----o---.1| Router1|.1----.2|internet|------------| SRV1 |
    -------        |     ----------        ----------            --------
    | PC2 |.168----|
    -------
If PC1 want's to communicate to server1 it will send a packet to Router1 with
destination 8.8.8.8 and source 192.168.0.167 (this is the _Inside Local_ address)

Router1 will the translate 192.168.0.167 to 100.0.0.1 (_inside Global address_),
a manually configured public IP address mapped to PC1 and forward this to Server1.
<strong>Note that this IP is not Router1 IP interface.</strong>

When PC2 will try to communicate to server 8.8.8.8, it will do the same as PC1 and
send the packet to Router1 that will translate PC2 IP 192.168.0.168 to 100.0.0.2,
its _inside Global address_ mapped on Router1.  

<strong>Staict NAT</strong> allows devices with private IP addresses to communicate
over the internet. However, because it requires a one-to-one IP address mapping,
it doesn't help preserve IP addresses.

<h4 align="center">Static NAT Configuration</h4>

Let's have a look on how to configure static NAT. First define the _inside_ interface/s:

    Router1(config)#int g0/1

    Router1(config-if)#ip nat inside

 And do the same for the outside interface:

    Router1(config)#int g0/0

    Router1(config-if)#ip nat outside

Then configure the one-to-one IP address mapping, the command is:

    Router1(config)#ip nat inside source static 192.168.0.167 100.0.0.1

    Router1(config)#ip nat inside source static 192.168.0.168 100.0.0.2

Naturally 100.0.0.1 and 100.0.0.2 are public IP address and must be owned by you.

To check the configuration we can do:

    Router1#show ip nat translations

It will result in something like this while following our setup:

    Router1#show ip nat translations
    Pro  Inside global      Inside Local          Outside local       Outside Global
    udp  100.0.0.1:56310    192.168.0.167:56310   8.8.8.8:53          8.8.8.8:53
    ---  100.0.0.1          192.168.0.167         ---                 ---
    udp  100.0.0.2:62321    192.168.0.168:62321   8.8.8.8:53          8.8.8.8:53
    ---  100.0.0.2          192.168.0.168         ---                 ---

<strong>Pro</strong> is the used protocol
Then we find what we talked about just earlier: <strong>Inside Global</strong>,<strong>Inside Local</strong>, <strong>Outside Local</strong>, <strong>Outside Global</strong>.

unless <strong>destination NAT</strong> is used, the Outside IP addresses will always be the same.


Let's see few useful extra commands:

    Router1#clear ip nat translation *

This command clears the table for the dynamic NAT entries. Static one cannot be
deleted this way unless we remove the _ip nat inside source static_ commands from
the router.

Then:

    Router1#show ip nat statistics

This commands will show you several details on total active translations, peak translations,
inside and outside interfaces.
