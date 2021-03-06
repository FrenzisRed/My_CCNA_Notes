<h2 align="center">LAN Architecture</h2>

There are standard 'best practices' for networking design, however there are few universal 'correct answers'. \
The answer to most general questions about network design is: "it depends". This is because it indeed
depends on the client needs and requirements.

Let's go over some common terminology to prepare for the chapter, not only for LAN design, but for network in general:

- <b>STAR</b> topology: When several devices all connect to one central device we can draw them in a 'star' shape like below, si this is often called a 'star topology'.

![Star](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/Star.png?raw=true "Star")

- <b>Full mesh</b>: When each device is connected to each other device.

![Full Mesh](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/Full_mesh.png?raw=true "full mesh")

- <b>Partial Mesh mesh</b>: When some devices are connected to each other, but not all.

![Partial Mesh](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/Partial.png?raw=true "partial mesh")

<h4 align="center">2-Tier and 3-Tier LAN Architecture</h4>

<h4>2-Tier LAN Architecture:</h4>

The Two-tier LAN design consist of two hierarchical layers:
- <b>Access Layer</b>: \
The layer that end hosts connect to (PCs, printers, cameras, etc..) \
Typically Access Layers Switches have lots of ports for end hosts to connect to. \
QoS marking is typically done here. \
Security services like port security, DAI, etc.. are typically performed here. \
switchports might be PoE-enabled for wireless APs, IP phones, etc..

- <b>Distribution Layer</b>: *Sometime called Aggregation Layer \
Also called  a 'Collapsed Core' design because it omits a layer that is found in the Three Tier design: the <b>Core Layer</b>. \
aggregates connections from the Access Layer Switches \
typically is the border between Layer 2 and Layer 3

Example of Tier-2 LAN architecture:

![Tier-2](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/tier-2.png?raw=true "Tier-2")

A1, A2, A3 and A4 form 2 <b>access layers</b>, think of it of different buildings inside a campus.

D1, D2, D3 and D4 are the <b>Distribution layers</b>, notice that D3 & D4 do not connect to the first <b>Access Layer</b>

In the picture we can see that the topology between the Distribution Layers is a <b>Full Mesh</b>, but the topology between the Distribution layer and its respective Access layer are <b>Partial Mesh</b>.

Now, if the network gets larger we might have many distributions layers connecting to different parts of the LAN as shown here:

![LAN](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/Distribution_no_core.png?raw=true "Distribution no core")

In large LAN networks with many Distribution Layers switches (for example in separated buildings), the number of connections required between Distribution Layers grows rapidly. \
This makes it much more difficult and complicated to scale networks, to grow it and make it bigger. \

So, to help scale large LAN networks we can add a Core layer.

<h4>3-Tier LAN Architecture:</h4>

The Three-tier LAN design consist of three hierarchical layers:
- <b>Access Layer</b>
- <b>Distribution layer</b>
- <b>Core layer</b>: \
 Connects Distribution layers together in large LAN networks \
 The focus is speed ('fast transport') \
 CPU-intensive such as security, QoS marking/classification, etc.. should be avoided at this layer. \
 Connections are all Layer 3, no spanning-tree! \
 Should maintain connectivity throughout the LAN even if devices fail.

This is the same network as before, but with a core layer in it:

![Tier-3](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/Distribution_with_core.png?raw=true "Tier-3")

BTW, Cisco recommend adding a Core Layer if there are more than three distribution layers in a single location.

Each distribution layer connects to the core layer, no need for a full mesh of direct connections between the distribution layer switches.

These core layer switches are a pair of very powerful and fast switches.

Because it's the backbone of the LAN, redundancy of devices and connections is very important.

Following our case study, here is a full diagram on how it should look:

![Tier-3](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/full_3_tier.png?raw=true "Tier-3")

<h4 align="center">Spine-Leaf Architecture (Data Center)</h4>

 Spine leaf design was created for data-centers, they used to work with three-tier architecture as shown before and worked well when most of the traffic was North-South. (meaning the traffic was from hosts up to the internet and from the internet down to the hosts passing the different layers in order).

 But in dataventer we have a lot of East-West traffic, this is traffic between these servers in the same part of the network. In this way the traffic does not go out ti the other parts of the LAN or out to the internet.

 With the precedence of virtual servers, applications are often deployed in a distributed manner, across multiple physical servers, which increase the amount of East-West traffic in the data center.

 With this kind of traffic, the traditional three-tier architecture led to bottlenecks in bandwidth as well as variability in server-to-server latency depending on the path the traffic takes.

 To solve this, Spile-leaf architecture (also called Clos architecture) has become prominent in data centers.

 Here is a basic diagram of the spine-leaf architecture, rules are under it:

 ![Spine-leaf](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/Spine-leaf.png?raw=true "Spine-Leaf")

Here are some rules about Spine-Leaf architecture:
- Every Leaf switch is connected to every Spine switch.
- Every Spine switch is connected to every Leaf switch.
- Leaf switches do not connect to other Leaf switches.
- Spine switches do not connect to other Spine switches.
- End hosts only connect to Leaf switches.
The path taken by traffic is randomly chosen to balance the traffic load among the Spine switches. \
Each server is separated by the same number of 'hops' (except those connected to the same leaf), providing consistent latency for East-West traffic.


<h4 align="center">SOHO (Small Office / Home Office)</h4>

SOHO refers to the office of a small company, or a small home office with few devices. It doesn't have to be an office, if our home has a network connected to the internet it is considered a SOHO network.

SOHO networks don't have complex needs, so all networking functions are typically provided by a single device, often a 'home router' or 'wireless router'.

This one device can server as a:
- Router
- Switch
- Firewall
- Wireless Access Point
- Modem

![SOHO](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/SOHO.png?raw=true "SOHO")
