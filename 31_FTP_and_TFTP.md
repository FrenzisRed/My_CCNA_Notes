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

<strong>TFTP :</strong>
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


<strong>FTP :</strong>

FTP was first standardized in 1971, this is actually before TCP and IP, so FTP
is a very old protocol, although it has of course been updated since.
FTP uses now TCP ports 20 and 21.

Usernames and passwords are needed for authentication, however there is no encryption.

For greater security, FTPS (FTP over SSL/TLS) can be used.

SSH File Transfer Protocol (SFTP) can also be used for greater security.

FTP is more complex than TFTP and allows not only file transfer, but clients can
also navigate file directories, add and remove directories, list files, etc...

The client sends FTP _commands_ to the server to perform these functions.

Why two ports? Because FTP uses two types of connections:

An <strong>FTP Control</strong> connection on TCO 21 is established and used to
send FTP commands and replies.

When files or data are to be transferred, separate <strong>FTP data TCP 20</strong> connections
are established and terminated as needed.

Two mode exist for FTP data connections :

    Default or active mode

The default method of establishing FTP data connections is <strong>active mode</strong>,
in which the server initiate the TCP connection.

    Passive mode

In FTP <strong>passive mode</strong>, the client initiates the data connection.
This is often necessary when the client is behind a firewall, which could block
the incoming connection from the server.

<h4 align="center">IOS File Systems</h4>

A file system is a way of controlling how data is stored and retrieved. We can
view the file system of a Cisco IOS device with:

    show file system

There are several, but we do not need to cover them all. It's worth noting the
Types of file systems. This refers to the storage devices such as flash memory.
This is usually where the Cisco IOS file itself is stored. When the device boots
up, it copies the IOS file from flash into RAM.
<strong>The Opaque </strong>disk type is used for specific internal functions. These refer to
logical internal systems, not actual separated storage devices.
<strong>The NVRAM </strong>type refers  to the NVRAM, non-volatile RAM, of the device.
<strong>The network </strong>type represent external file systems, for example FTP or TFTP servers.


<h4 align="center">Using FTP/TFTP in IOS</h4>

Let's imagine we have a Server, a switch and a router:

    --------            ---------            ----------  
    | SRV1 |------------|Switch1|------------| Router1|
    --------            ---------            ----------

Let's see how we can bring the new IOS from server to router.

We can check the version with the _show version_ command and check the flash content
with the _show flash_ command.

to copy the new version to the router you must use this command on the router CLI:

    Router#copy tftp: flash: #copy source destination

The the router will ask you the TFTP server IP, file name and destination filename:

    Address or name of remote host []? ENTER IP

    Source filename[]? NAME OF THE FILE # you have to know it before hand

    Destination filename[]? NAME OF FILE # hit enter to use same name.

Now to make the router use it we need to:

    Router(config)#boot system flash:NAMEOFTHENEWIOS

If you do not specify the name it will load the first available in the flash memory.

remember to exit and write to memory before reloading the device, or the BOOT SYSTEM
command won't take effect.

The simply use the _reload_ command to restart the device.

Once we checked that the new version is installed, let's delete the old version
to save space and avoid conflicts:

    delete flash:NAMEOFOLDIOS

Let's look how FTP works in the same scenario. First let's configure username
and password for connecting to the server:

    Router(config)#ip ftp username USERNAME

    Router(config)#ip ftp password PASSWORD

    Router#copy ftp: flash:

All the rest is the same as just seen.
