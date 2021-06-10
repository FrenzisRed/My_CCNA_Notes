<h2 align="center">FTP & TFTP - File Transfer Protocol & Trivial File Transfer Protocol</h2>


<h4 align="center">Purpose</h4>

FTP and TFTP are industry standard protocols used to transfer files over the network.

They both use a client-server model, clients can use FTP or TFTP to copy files
from a server or to a server.

As a network engineer, the most common use for this protocol is in the process
of upgrading the operating system of a network device. We can download the newest
version of the IOS directly in the device and then reboot it to install it.

For the CCNA we will think it this way:

The network Admin will download from his workstation the newest router IOS from
Cisco website. We will the transfer it on a local server that the router can reach.
We will then use FTP/TFTP to retrieve the image directly from the router.



<h4 align="center">FTP/TFTP functions & differences</h4>

TFTP was first standardized in 1981. Named Trivial because it is simple and has
only basic features compared to FTP. It only allows a client to copy a file to
or from a server.
It was released after FTP, but it's not a replacement for FTP. It is another tool
to use when lightweight simplicity is more important than functionality.

No authentication is needed, so servers will respond to all TFTP requests

Also no encryption, all data is sent in plain text.

For these lacks of security, TFTP is best used in a controlled environment to
transfer small files quickly.

TFTP servers listen on UDP port 69. UDP is connectionless and doesn't provide
reliability with retransmissions. However TFTP has a similar built-in features
within the protocol itself.

Every TFTP data message is acknowledged, if the client transferring a file to
the server, the server will send Ack messages for each message.
If the server is transfering a file to the client, the client will do the same
and send Ack messages.
Timers are used, and if an expected message isn't received in time, the waiting
device will re-send it's previous message.

TFTP uses "lock-step" communication. The client and server alternately send
messages and then wait for a reply. (+retransmissions are sent if needed)

TFTP file transfers have three phases:

    1)Connection

TFTP client sends a request to the server, and the server respond back initializing
the connection.

    2)Data Transfer

The client and server exchange TFTP messages. One sends data and the other sends
acknowledgments.

    3)Connection Termination

After the last data message has been sent, a final acknowledgment is sent to
terminate the connection.

A little interesting note, not for the CCNA exam, but still worth noting:

When the client sends the first message to the server, the destination port is
UDP 69 and the source port is a random ephemeral port.
This random port is called a 'transfer identifier'(TID) and identifies the data transfer.
The server then also select a random TID to use as the source port when it replies, not 69.
When the client sends the next message, the destination port will be the server TID
and not the UPD 69.

<h4 align="center">IOS File Systems</h4>

<h4 align="center">Using FTP/TFTP in IOS</h4>
