<h2 align="center">Port Security</h2>

Port security is a security feature of Cisco switches. It allows us to control which
source MAC address(es) are allowed to enter the switchport.
If an unauthorized source MAC address enters the port, an action will be taken. The default
one is to place the interface in an "err-disabled" state.

Let's say I bring my laptop at work. I unplug my workstation rj45 cable and plug it
into my laptop. With the first packet the switch will see a new MAC address that is not
been authorized and will turn the interface in err-disabled mode. My laptop will be
unable to connect to the network. So I take back the RJ45 to my desktop....Surprise,
my workstation cannot connect neither.

When we enable port security on an interface with the default settings, one MAC
address is allowed. We can configure the allowed MAC address manually. If we do not
the switch will allow the first source MAC address that enters the interface.

We can change the maximum number of MAC addresses allowed. For example when using
an IP phone. We have a PC connected to the IP phone and then to the switch. Two MAC
will be going through the switch, we need to allow more than one address. 

<h4 align="center">Intro to Port Security</h4>


<h4 align="center">Why use Port Security</h4>

<h4 align="center">Port Security configuration</h4>
