<h2 align="center">CDP & LLDP</h2>

<h4 align="center">Intro to Layer 2 discovery protocols</h4>

Layer 2 discovery protocols such as CDP and LLDP share information with and discover information about neighboring (connected) devices. \
The shared information includes host name, IP addresses, device type, etc.. \
CDP is a Cisco proprietary protocol and should be used only if all devices are Cisco devices.\
LLDP is an industry standard protocol (IEEE 802.1AB). \
Because they share information about the devices in the network, they can be considered a security risk and are often not used. It is up to the network engineer/admin to decide if they want to use them in the network or not.

<b>CDP:</b> \
It is enables by default on Cisco's devices (routers, switches, firewalls, IP phones, etc..). \
CDP messages are periodically sent to multicast MAC address 0100.0ccc.cccc \
When a device receive a CDP message, it processes and discards the message. It does <b>NOT</b> forward it to other devices. \
By default, CDP messages are sent once every <b>60 seconds</b>.


<h4 align="center">Cisco Discovery Protocol (CDP)</h4>

<h4 align="center">Link Layer Discovery Protocol (LLDP)</h4>
