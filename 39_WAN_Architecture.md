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

<h4 align="center">Internet connectivity</h4>

<h4 align="center">Internet VPNs</h4>
