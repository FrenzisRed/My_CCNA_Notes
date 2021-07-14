<h2 align="center">CDP & LLDP: Discovery Protocols</h2>

<h4 align="center">Intro to Layer 2 discovery protocols</h4>

Layer 2 discovery protocols such as CDP and LLDP share information with and discover information about neighboring (connected) devices. \
The shared information includes host name, IP addresses, device type, etc.. \
CDP is a Cisco proprietary protocol and should be used only if all devices are Cisco devices.\
LLDP is an industry standard protocol (IEEE 802.1AB). \
Because they share information about the devices in the network, they can be considered a security risk and are often not used. It is up to the network engineer/admin to decide if they want to use them in the network or not.

<h4 align="center">Cisco Discovery Protocol (CDP)</h4>

It is enables by default on Cisco's devices (routers, switches, firewalls, IP phones, etc..). \
CDP messages are periodically sent to multicast MAC address 0100.0ccc.cccc \
When a device receive a CDP message, it processes and discards the message. It does <b>NOT</b> forward it to other devices. \
By default, CDP messages are sent once every <b>60 seconds</b>. \
By default, the CDP holdtime  is <b>180 seconds</b>. If a message isn't received from a neighbor for 180 seconds, the neighbor is removed from the CDP neighbor table. \
CDPv2 messages are sent by default, CDPv1 is very old and not used anymore.

On a router we can use this command to see global CDP information:

    Router1#show cdp

Use this one to check information on traffic:

    Router1#show cdp traffic

And this one to see the interfaces information:

    Router1#show cdp interface

To check the CDP neighbor table we use:

    Router1#show cdp neighbor

Here we can learn a lot, let's check the columns we have:
- Device ID: we can see the neighbors the device has received messages from.
- Local Intrfce: We can see the interface used
- Holdtme: This will be reset to 180 every time it receive a CDP message. If it gets to 0 the neighbor will be removed.
- Capability: This helps identify what kind of device you are connected to.
- Platform: This display the model of the neighboring device.
- PortID: This tells you the port ID on the neighboring device.

To learn more about CDP we can use the following command that gives us a lot of information:

    Router1#show cdp neighbor detail

It will gives us detailed information about all the neighbors. If we want full details on a specific neighbor we can use:

    Router1#show cdp entry DEVICENAME


<h4 align="center">Link Layer Discovery Protocol (LLDP)</h4>
