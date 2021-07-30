<h2 align="center">WAN Architecture</h2>

<h4 align="center">Intro to WAN</h4>

WAN stands for Wide Area network. \
A WAN is a network that extends over large geographic area. \
WANs are used to connect geographically separate LANs \
Although the internet itself can be considered a WAN, the term WAN is typically used to refer to an enterprise's private connection that connect their offices, data centers, and other sites together. \
Over public/shared networks like the internet, VPNs (Virtual Private Networks) can be used to create private WAN connections.
There have been many different WAN technologies over the years. Depending on the location, some will be available and some will not be. Technologies which are considered 'legacy' (old) in one country might still be used in other countries.

<h4 align="center">Leased Lines</h4>

A <b>leased line</b> is a dedicated physical link, typically connecting two sites.
Leased lines use serial connections ( PPP, HDLC encapsulation).
There are various standards that provide different speeds, and different standards are available in different countries.

Here is a little chart of standards to give an idea:

![Standards](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/standards.png?raw=true "Standards")

Due to the higher cost; higher installation lead time and slower speeds of leased lines, Ethernet WAN technologies are becoming more popular.

<h4 align="center">MPLS VPNs</h4>

MPLS stands for 'Multi Protocol Label Switching'.
Similar to the internet, service providers MPLS networks are shared infrastructure because many customer enterprises connect and share the same infrastructure to make WAN connections.
However, the _label switching_ in the name of MPLS allows VPNs to be created over the MPLS infrastructure through the use of <b>labels</b>.

Some important terms:
- CE router = Customer Edge router
- PE router = Provider Edge router
- P router  = Provider core router

As an example:

![Routers](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/routers.png?raw=true "Routers")

When the PE routers receive frames from the CE routers, they add a label to the frame. This label is actually placed in between the layer 2 Ethernet header and the Layer 3 IP header, so sometime MPLS is called a Layer 2.5 protocol. \
These labels are the used to make forwarding decisions within the service provider network, not the destination IP.

The CE routers do not use MPLS, it is only used by the PE/P routers. \
When using a _layer 3 MPLS VPN_, the CE and PE routers peer using OSPF (or any other routing protocol), to share routing information. Or the customer could just write static routes using the PE routers as the next hop.

For example, in the diagram above, Office A's CE will peer with one PE and Office B's CE will peer with the other PE. Then, office A's CE will learn about office B's routes via *OSPF peering, and office B's CE will learn office A's routes too. This is a Layer 3 MPLS VPN.

When using _layer 2 MPLS VPN_, the CE and PE routers do not form a peering. So the entire service provider network is transparent to the CE routers. Although the CE routers will physically connect to a PE router, it is in effect like two CE routers are directly connected. Their WA? interfaces will be in the same subnet. \
If a routing protocol is used, the two CE routers will peer directly with each other. In this case the service provider network is still running MPLS just like before, but it's doing so in a way that it's like the entire service provider network is just a big switch connecting the two CE routers together.

MPLS is a technology that runs in the service provider network, but many different technologies, many different kinds of connections, can be used to actually connect to the service provider's MPLS network for WAN service.

Here is a little example:

![Connetions](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/connections_to_sp.png?raw=true "Connections")

They will all connect to each other through the service provider.

For the CCNA exam we should know that MPLS uses labels to forward traffic, not IP addresses. We should know the terms CE routers, PE routers and P routers. We should know that Layer 3 MPLS VPNs have CE routers and PE routers forming peerings using a routing protocol such as OSPF, whereas in Layer 2 MPLS VPNs it as if the CE routers are all directly connected to each other. The service provider routers are completely transparent and acting like a switch connecting the CE routers together.

<h4 align="center">Internet connectivity</h4>

There are countless way for an enterprise to connect to the internet, for example, private WAN technologies such as leased and MPLS VPNs can be used to connect ti a service provider's internet infrastructure. \
In addition, technologies such as CATV and DSL commonly used by consumers (home internet access) can also be used by an Enterprise. \
These days, for both enterprise and consumer internet access, fiber optic Ethernet connections are growing in popularity due to the high speeds they provide over long distances.

Let's briefly look at two internet access technologies: CATV and DSL.

<b>DSL</b>: Digital Subscriber Line provides internet connectivity to customers over phone lines, and can share the same phone line that is already installed in most homes. There is an extra device here that we did not talk about yet, that is the modem. \

![DSL](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/CATV_DSL.png?raw=true "Diagram")

A modem, which stands for modulator-demodulator, is required to convert data into a format suitable to be sent over the phone lines. \
The modem might be a separate device, as in the diagram, or it might be incorporated into a home router.

This connect the network to the service provider over the phone lines. But there is another common kind of communication line installed in most homes that can also be used for internet access: Cable Internet.

![CATV](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/CATV.png?raw=true "Diagram")

Cable Internet provides internet access via the same CATV (Cable Television) lines used for TV service.

Like DSL, a cable modem is required to convert data into a format suitable to be sent over the CATV cables. And as for DSL, this can be a separate device or built in the home router.


Now, for a home user, having one connection to the internet isn't a problem. it's a bit annoying if we lose Internet access, but it's not a disaster. However, for many companies Internet access is essential to their operations. So, it's best to have redundant Internet connections, and there are a few terms we should know:

- If we have 1 connection to 1 ISP, it's called <b>Single Homed</b>.
- If we have 2 connections to that same ISP, it's called <b>Dual Homed</b>.
- If we have 1 connection to each of 2 ISPs it's called <b>Multihomed</b> #better redundancy
- If we have 2 connections to each of 2 ISPs it's called <b>Dual Multihomed</b> #most redundancy

<h4 align="center">Internet VPNs</h4>

Private WAN services such as leased lines and MPLS provide security because each customer's traffic is separated by using dedicated physical connections (leased lines) or by MPLS tags. \
When using the internet as a WAN to connect sites together, there is no built-in security by default. \
To provide secure communications over the internet, VPNs (Virtual Private Networks) are used. Here we will see two kinds of VPNs:

- Site-to-Site VPNs using IPsec
- Remote-access VPNs using TLS

<h3 align="center">Site-to-Site VPNs using IPsec</h3>:

A site-to-Site VPN is a VPN between two devices and is used to connect two sites together over the internet. \
A VPN 'tunnel' is created between the two devices by encapsulating the original IP packet with a VPN header and a new IP header. When using IPsec, the original packet is encrypted before being encapsulated with the new header. This is what makes IPsec secure.

Here is a very basic small diagram to show how it works:

![IPsec](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/IPsec_VPN.png?raw=true "IPsec")

Explanations:
- The sending device combines the original packet and session key (encryption key) and runs them through an encryption formula.
- The sending device encapsulates the encrypted packet with a VPN header and a new IP header.
- The sending device sends the new packet to the device on the other side of the tunnel.
- The receiving device decrypts the data to get the original packet, and then forwards the original packet to its destination.

In a 'site-to-site' VPN, a tunnel is formed only between two tunnels endpoints (for example, the two routers connected to the internet).

All other devices in each site don't need to create a VPN for themselves. They can send unencrypted data to their site's router, which will encrypt it and forward it in the tunnel as described above.

There are some limitations to standard IPsec:

- IPsec doesn't support broadcast and multicast traffic, only unicast. This means that routing protocols such as OSPF can't be used over the tunnels, because they rely on multicast traffic. (This can be solved with 'GRE over IPsec' - see below).
- Configuring a full mesh of tunnels between many sites is a labor-intensive task. ( this can also be solved with Cisco's DMVPN
)

Just a brief description of <b>GRE over IPsec</b>:

GRE ( Generic Routing Encapsulation) creates tunnels like IPsec, however it does not encrypt the original packet, so it's not secure.
However it has the advantage of being able to encaplsulate a wide variety of Layer 3 protocols as well as broadcast and multicast messages. So to get the flexibility of GRE with the security of IPsec, GRE over IPsec can be used. The original packet will be encapsulated by GRE header and then a new IP header, and then the GRE packet will be encrypted and encapsulated within an IPsec VPN header and a new IP header.

<b>DMVPN</b>:

DMVPN (Dynamic Multipoint VPN) is a Cisco-developed solution that allows routers to dynamically create a full mesh of IPsec tunnels without having to manually configure every single tunnel.

To simplify, we configure IPsec tunnels to a hub site:

![hub site](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/hub_site.png?raw=true "IPsec hub site")

Then the hub router gives each router information about how to forman IPsec tunnel with the other routers.

![IPsec Mesh](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/IPsec_mesh.png?raw=true "Mesh")

To summarize, DMVPN provides the configuration simplicity of hub-and-spoke (each spoke router only needs one tunnel configured) and efficency of direct spoke-to-spoke communication (spoke routers can communicate directly without traffic passing through the hub).

Be aware that some company may want all traffic to go through the hub router so a central firewall can control the traffic, but many do not want it as the mesh spoke-to-spoke is more efficient.

<h3 align="center">Remote-access VPNs:</h3>

Whereas site-to-site VPNs are used to make a point-to-point connection between two sites over the internet,remote-access VPNs are used to allow en devices (PCs, mobile phones) to access the company's internal resources securely over the internet.

Remote-access VPNs typically use TLS ( transport Layer Security), TLS is also what provides security for HTTPS. TLS formerly known as SSL (secure socket layer) and developed by Netscape, but it was renamed to TLS when it was standardized by the IETF.

VPN client software (for example Cisco AnyConnect) is installed on end devices (company laptops used from home).

These end devices then form secure tunnels to one of the company's routers/firewalls acting as a TLS server. This allows the end users to securely access resources on the company's internal network without being directly connected to the company network.

Let's visualize it:

![Remote-Access](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/remote_access.png?raw=true "Remote-access")

<h3 align="center">Site-to-Site vs Remote-access VPN:</h3>

- <b>Site-to-Site</b> VPNs typically use IPsec  
- <b>Remote-access</b> VPNs typically use TLS.
- ----------
- <b>Site-to-Site</b> VPNs provide service to many devices within the sites they are connecting.
- <b>Remote-access</b> VPNs provide service to the one end device the VPN client software is installed on.
- ----------
- <b>Site-to-Site</b> VPNs are typically used to permanently connect two sites over the internet.
- <b>Remote-access</b> VPNs are typically used to provide on-demand access for end devices that want to securely access company resources while connected to a network which is not secure.
