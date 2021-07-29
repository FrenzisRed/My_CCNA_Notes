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


<h4 align="center">Internet connectivity</h4>

<h4 align="center">Internet VPNs</h4>
