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
When a device receive a CDP message, it processes and discards the message. It does <b>NOT</b> forward it to other devices. So only directly connected devices becomes neighbors. \
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


<h4 align="center">CDP Configuration</h4>

As stated above CDP is enabled globally by default on each interface.

To enable/disable CDP globally:

    Router1(config)#[no] cdp run

To enable/disable CDP on an interface we use:

    Router1(config-if)#[no] cdp enabled

To set the timer different from the default value, use:

    Router1(config)# cdp timer SECONDS

We can configure the holdtime in the same way:

    Router1(config)# cdp holdtime SECONDS

And we can enable CDP version 2, which is the default state, with:

    Router1(config)# [no] cdp advertise-v2


<h4 align="center">Link Layer Discovery Protocol (LLDP)</h4>

LLDP, an industry standard made after CDP, is usually disabled on Cisco devices by default, so it must be manually enabled. \
A device can run CDP and LLDP at the same time, but usually we just use one. \
LLDP messages are periodically sent to multicast MAC address 0180.c200.000E \
When a device receive an LLDP message, it processes and discards the message, it does NOT forward it to other devices. So only directly connected devices becomes neighbors. \
By default LLDP messages are sent once every <b>30 seconds</b>, half the CDP time. \
By default the LLDP holdtime is <b>120 seconds</b>. \
LLDP has an additional timer called the 'reinitialization delay'. If LLDP is enables (globally or on an interface), this timer will delay the actual initialization of LLDP. <b>2 seconds</b> by default.


<h4 align="center">LLDP Configuration</h4>

To enable LLDP we need to enable it globally and then on wanted interfaces.

To enable it globally:

    Router1(config)# lldp run

To enable the interface to send LLDP messages(tx):

    Router1(config-if)# lldp transmit

This interface will now send LLDP messages(rx), but not receive them, they will be discarded unless we do:

    Router1(config-if)#lldp receive

Here are the commands for the timers:

    Router1(config)# lldp timer SECONDS

    Router1(config)# lldp holdtime SECONDS

    Router1(config)# lldp reinit SECONDS

Here are show commands similar to the CDP we saw above:

    Router1#show lldp

    Router1#show lldp traffic

    Router1#show lldp interface

    Router1#show lldp neighbors

In the neighbor table there are couple of differences:

- the hold-time will show us the time configured and not the actual countdown.
- There is no Switch capabilities, LLDP uses B for bridge.
- There is no platform column.

And as before, for full details:

    Router1#show lldp neighbor detail

Or single interface details:

    Router1#show lldp entry DEVICENAME


Here is a wireshark capture, have a look on Destination field and the Cisco Discovery Protocol information:

![Wireshark](https://github.com/FrenzisRed/My_CCNA_Notes/blob/main/images/wireshark_CDP.png?raw=true "CDP packet")
