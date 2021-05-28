<h2 align="center">DHCP = Dynamic Host Configuration Protocol</h2>

<h5>Purpose:</h5>

DHCP learn to devices much more than just the IP address to use.
DHCP is used to allow hosts to automatically/dynamically learn various aspects
of their configuration, such as IP addresses, subnet mask, default getaway,
DNS server, etc... Without manual/static configuration.

It's an essential part of modern Networks. For example, when you connect a
phone/laptop to WiFi, do you ask the network admin which IP address, subnet mask,
default getaway, etc, for the device to use? Don't think so, even if it could be
done, but would be unpractical in large networks or other type of environments.

DHCP is typically used for 'client devices' such as phones, workstations, etc..
Devices such as routers, servers, etc.. are usually manually configured. This is
as well that these types of devices do need a fixed IP to perform their functions.

Imagine if the network getaway kept changing its IP, the network would be affected.

In small networks or home networks, the router typically acts as the DHCP server
for hosts in the LAN.

In larger Networks, the DHCP server is usually a Windows/Linux server.


<h5>Basic Functions:</h5>

#### Wireshark is higly recommended to capture and follow along

In windows, with command ipconfig /all you can check if it's enable or not

Under you can see the IP assigned to your PC
You might encounter the mention 'preferred' after the given IP, this mean that
this PC has already got this IP from the previous request and that it's been
re assigned to it again on a new request.

More under you can find the lease time, here you can see when the IP was obtained
and when it will expire. DHCP servers 'lease' IP addresses to clients. These leases
are usually not permanent, ant the client must give up the address at the end of
the lease. A client can actually release it before expire if it does not longer need it.

We say usually just before as it is possible to configure the DHCP to permanently
assign IP addresses, however is often, if not always, best to set an expiry time.

Imagine if in a public place like a cafe or airport the IP were permanent for
all clients passing by...

At the bottom of the list you can see the default gateway, DHCP server and DNS servers.

Here are the 4 messages used to lease an IP address from a DHCP server:

Note: DHCP servers use UDP 67 and DHCP clients use UDP 68.

Let's use an example of renewing an IP address.

    1st message: The DHCP Discovery message.
      it's a broadcast message from the client. Basically asking if there are any
      DHCP server on the local network telling I need an IP.
      In the capture you can see on layer 2 the destination been the Broadcast, all Fs
      On layer 3 you can see the source been all 0s as we do not have an IP yet,
      and the destination been all 255s as it is a broadcast message.
      You can see that the destination is pour 67 and source is port 68.
      On layer 5 you can see the message and options, few mentions:

        The Bootp flags field. Bootp is actually the precedessor of DHCP.
        it's strange, the value it's all 0s, this is unicast, but we knw it's Broadcast.
        If we look under the client is 0.0.0.0, since the PC has no IP.
        The PC MAC is also here.
        A bit under we have the options field. Let's point out option 50, request
        IP address. Because the PC had already had an assigned IP from this DHCP,
        you can see that it's requesting the same again. If available, it will be granted.


    2nd message: DHCP offer message.

      It is sent from the DHCP server to the client, offering and address for the
      client to use, as well as other information like default getaway, DNS server...

      In Wireshark we can see in layer2 that this Offer message is sent as a unicast
      frame to the client's MAC address since the server learned the client's MAC
      address from the Discovery message.

      It's also unicast at layer 3, the destination is the IP address being offered
      to the client.

      Let's look at the offer message:
        Bootp flags, it's an unicast flag again. When the PC sent the DHCP discover
        this field was also at 0x0000 (unicast). This mean that the server will send
        its messages using unicast.
        However that's not always the case. The DHCP Offer message can be sent to
        the client using either broadcast or unicast. It depends on the client.
        In this case scenario my pc told the router to send the Offer using unicast,
        so it did it. If my PC did ask the router to send the Offer using broadcast,
        the destination MAC of this message would be all Fs and the destination
        IP would be 255.255.255.255.

        At this point in the DHCP process, the client's IP address isn't actually
        configured. Some clients won't accept unicast messages before their IP is
        actually configured, so that's why sometimes broadcast must be used instead
        of unicast.

        Finally, in the options we can notice the option 51 that indicates the
        lease time, option 6 is 'domain name server' that's the DNS server, and
        option 3 is 'router', which tells the client the default gateway.



If we capture the release of an IP address with wiresherk we can notice that an IPv4
header is inside from PC to router.

Inside you have an UDP header, notice the ports, source port 68, destination port 67.
DHCP servers use UDP 67 and DHCP clients use UDP 68.
As an example in DNS only the server port is decided, the client uses an ephemeral port.

Inside the UDP segment we have the release message. The client IP is indicated here.
Notice these 'options' inside at the bottom. DHCP has varous options used for
different purposes. For example option 53 indicate what kind of message it is.

Note in your capture, just above the options, you have the magic cookie.
This comes from the BOOTP that has identical header. The magic cookie make it
possible to identify that the message comes from DHCP.





<h4 align="center">Configuring DHCP in Cisco IOS: </h4>
