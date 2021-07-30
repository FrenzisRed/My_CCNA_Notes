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
When using the internet as a WAN to connect sites together, there is no built-in security by default.
