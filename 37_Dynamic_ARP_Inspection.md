<h2 align="center">DAI = Dynamic ARP Inspection</h2>


<h4 align="center">What is Dynamic ARP Inspection</h4>

DAI is a security feature of switches that is used to filter ARP messages received on
_untrusted_ ports. \
DAI only filters ARP messages. Non-ARP messages aren't affected. \
All ports are _untrusted_ by default. Typically, all ports connected to other networks
devices (switches, routers) should be configured as <b>trusted</b>, while interfaces connected
to end hosts should remain <b>untrusted</b>.

let's use this network:

    ----------                ----------          ---------            -------
    | Router |G0/1--------G0/0| Switch1|.1----G0/0|Switch2|o-----------| PCs |
    ----------                ----------          ------o--            -------
                                                        |______________| PC2 |
                                                                       -------




<h4 align="center">How does it work</h4>




<h4 align="center">What attacks does it prevent</h4>




<h4 align="center">DAI configuration</h4>
