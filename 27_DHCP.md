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

#### <h4 align="center">Wireshark is higly recommended to capture and follow along</h4>

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
        I had a look for my curiosity only, this is not needed in the CCNA exam.

    3rd message: DHCP Request message.

      It is sent from the DHCP client to the server, telling the server that it
      want to use the IP address it was offered. This is important as there may
      be multiple DHCP servers on the local network, and all of them will reply
      to the client's Discover message with an Offer. The client has to tell which
      server it is accepting the offer from and request to use that IP address.
      Typically, the client will accept the first Offer it receives.

      Let's look at that request message in Wireshark:
        Once again in layer two it's a broadcast message. If there are multiple
        DHCP servers in the Network, all of them will receive this message.
        One of the later fields will indicate which server the PC accepted the
        offer from.

        The source IP still 0.0.0.0 as the offer is not been configured yet.

        In the request message, we find again the flags field telling the server
        to send its messages using unicast, even if the client is actually using
        broadcast. In the option we can mention option 54 that gives the server
        address. If there are multiple DHCP servers on the local network, this is
        how the client says which server it selected.

    4th message: The DHCP Ack, acknowledgement.

      This is sent from the server to the client, confirming that the client may
      use the requested IP address. Once this message is received the client finally
      configures the IP address on its network interface.

      In Wireshark we can note:
        On layer 2 and 3 destinations addresses are unicast.
        In the Ack message the bootp flags field still on unicas because the client
        requested unicast messages. Just like the DHCP Offer message, the DHCP Ack
        message can be either unicast or broadcast, depending on what client requests.



If we capture the release of an IP address with Wireshark we can notice that an IPv4
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

Now let's talk about the <h5>DHCP relay</h5>.
It is possible to configure  routers to act as the DHCP server for its connected LANs
However in large enterprises it's better to have a centralized DHCP server, which
will be assign IP addresses to DHCP clients in all subnets throughout the enterprise
network.

If the server is centralized, it won't receive the DHCP clients broadcast DHCP
messages as broadcast messages do not leave the local subnet, routers do not
foward them. However to fix this we can configure a router to act as a
DHCP relay agent. If we do so, the router will foward the clients Broadcast
DHCP messages to the remote DHCP server as unicast messages. This is easily
done with one command. I keep all the command for configuration together in the
section below.


<h4 align="center">Configuring DHCP in Cisco IOS: </h4>

Let's configure a router to be a DHCP server:

    ip dhcp excluded-address IP-RANGE # lower ip - higher ip of the range

This command will crete a list of IPs that will be excluded and not given to clients
Always usefull to reserve IPs for configuration uses.

Now to create a pool do:

    ip dhcp pool POOLNAME # always good to give clear name to organize.

It's a good practice to create separated pools for each network the router is
acting as a DHCP server for.

To specify the range of addresses to assign to clients di:

    network STARTIP NETWORKMASK/PREFIX  

Then we should configure the DNS server that clients should use:

    dns-server IPofSERVER

The we can configure the domain of the network:

    domain-name DOMAINNAME

Then we do the default router command for the getaway:

    default-router IPADDRESS

And let's set the lease time:

    lease DAYS HOURS MINUTES

We can use infinite too, but it's not recommended.

    lease infinite

Use the following command to show all DHCP clients that have IP addresses:

    show ip dhcp binding

Now let's see the relay agent:

    to configure the router into been a relay agant we need to configure it in
    the interface that the subnet with the clients is connected to.

Once in the interface we need only one command:

    ip helper-address DHCPSERVERIP

We can verify the good configuration checking the interface with the do show ip
interface command.
