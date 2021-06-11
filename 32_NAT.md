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


<h4 align="center">Static NAT Configuration</h4>
