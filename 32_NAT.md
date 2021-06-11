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

Let's see now <strong>Static NAT</strong>

<h4 align="center">Static NAT Configuration</h4>
