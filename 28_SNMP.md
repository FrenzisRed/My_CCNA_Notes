<h2 align="center">SNMP = Simple Network Management Protocol</h2>

<h4 align="center">Overview</h4>

SNMP is an industry standard framework and protocol that was originally released
in 1988. It is not a single protocol, it fits into a larger framework of networks
management, as well referred to as SNMP framework.

The first RFC (request for comment, used by the IETF -Internet Engineering Task Force)
was the 1065, structure and identification of management for TCP/IP based internets.

The second RFC was the 1066, management information based for networks management
of TCP/IP based internets.

The third was 1067, a simple network management protocol.

These 3 RFC are the SNMP V1.

There are many more RFC since then defining new versions of the SNMP.

SNMP can be used to monitor the status of devices, make configuration changes, etc..
There are two main types of devices in SNMP, the managed devices and the network
management station (NMS).

Type one are devices like routers or switches.
Type two are the Devices managing the managed type one devices. This is called
the SNMP 'server' although typically we would just call it NSM instead.

let's use this example network to demonstrate how NMS works:

    ----------           --------              --------               -------
    | internet|----------| Router|-------------|Switch|---------------| PC1 |
    ----------           ---------             --------      |        -------
                                                   |         |________| PC2 |
                                                   |         |         ------
                                                   |         |________| PC3 |
                                              ------------             ------
                                              |NMS Server|
                                              ------------
In this network we have a router connecting to the internet and back to a switch
connected to a NMS server and 3 PCs.

Here we are using NMS to manage the router and the switch.

The three main operations we are looking at are:

    1) Managed devices can notify the NMS of events, for example switch port to
       PC1 has a failure and the status goes to down. The switch can send a Messages
       to the NMS telling it that the port has gone down.

The actual NMS might be configured to notify the network administrator on events.

    2) The NMS can ask the managed devices for information about their status.
       For example, NMS could ask the router what is its current CPU usage.   

    3) NMS tells the managed devices to change aspects of their configuration.
       For example, the NMS could ask the router to change the IP address of one
       of its interfaces.

How does it works?

Two main components in the NMS:
The NMS is probably just the admin pc with a NMS software

    The NMS Manager is the software on the NMS that interacts with the managed devices.
    It receives notifications, sends requests for information, send configuration
    changes, etc..

    The SNMP Application provides an interface for the network admin to interact with.
    It display alerts, statistics, charts, etc..

In the managed devices there are as well two components:

    SNMP Agent is the SNMP software running on the managed devices that interacts
    with the SNMP Manager on the NMS.
    It sends notifications or receive messages from the NMS.

    The Management Information Base (MIB) is the structure that contains the variables
    that are managed by SNMP. Each variable is defined with an Object ID (OID)
    Some variables contain Interface status, traffic throughput, CPU usage, temp..

SNMP Objects IDs are organized in an hierarchical structure.
Example of an OID and Breakdown:

    .1.3.6.1.2.1.1.5

    .ISO.Identified-organization.dod.internet.mgmt.mib-2.system.sysName.

The OID is used to identify the system name, the host name, of the managed device.

More information on oid-info.com but not necessary for the CCNA.

SNMP ports are:

    SNMP Agent   = UDP 161
    SNMP Manager = UDP 162


<h4 align="center">Versions</h4>

There are several versions of the SNMP, but only 3 have achieved wide-spread use.

<strong>SNMPv1</strong> The original one.

<strong>SNMPv2c</strong> It allows NMS to retrieve large amount of information in a single
request, so it is more efficient and it produce less network traffic.
The 'c' refers to 'community strings' used as passwords in SNMPv1, removed
in SNMPv2, and then added back for SNMPv2c.

<strong>SNMPv3</strong> A much more secure version of SNMP that supports strong encryption
and authentication/ Whenever possible, this version should be used.
This security makes it possible to have only the SNMP intended devices are able
to read the messages, they can't be interpreted and read by an attacker

<h4 align="center">Messages</h4>

    --------------------------------------------------------------------------------
    Messages Class|                   Description                         Messages
    --------------------------------------------------------------------------------
      Read        | Messages sent by NMS to read information from       |get,GetNext
                  | the managed devices. (ie. What's your CPU usage?)   |GetBulk
    --------------------------------------------------------------------------------
      Write       |Messages sent by the NMS to change information on the|  set
                  |managed devices. (ie. change IP address)             |
    --------------------------------------------------------------------------------
     Notification |Messages dent by the managed devices to alert the NMS|   Trap
                  |of a particular event.(ie. interface going down)     |  Inform
    --------------------------------------------------------------------------------
     Response     |Messages sent in response to a previous              | Response
                  |Message/Request                                      |
    --------------------------------------------------------------------------------

More in depth:

<strong>Get</strong>: A request from the manager to the agent to retrieve the value
of a variable (OID), or multiple variables. The agent will send a _Response_ messages
with the current value of each variable.

<strong>GetNetx</strong>: A request sent from the manager to the agent to discover
the available variables in the MIB.

<strong>GetBulk</strong>: A more efficient version of <strong>GetNext</strong>
message (introduced in SNMPv2).

<strong>Set</strong>: A request sent from the manager to the agent to change the
value of one or more variables. The agent will send a _Response_ message with
the new values.

<strong>Trap</strong>: A notification sent from the agent to the manager. The
manager does not send a _Response_ message to acknowledge that it received the
Trap, so these messages are 'unreliable'. Worth noting that SNMP uses UDP and
not TCP, so there are no retransmissions. This type will probably appear as an
alert on the server, or might trigger an email to the administrator.

<strong>Inform</strong>: A notification message that is acknowledged with a
_Response_message. Originally used for communications between managers, but later
updates allow agents to send Inform messages to managers. Still UDP.

<strong>Response</strong>: A message to respond to previous messages.



<h4 align="center">Basic Configuration</h4>

Configuring SNMP is out of the scope of the CCNA. Definitely the NMS configuration,
but here is a little SNMP agent configuration information. Let's take the previous
network topology.
For example, to  configure the router to be a SNMP agent we can use these commands:

Create community strings:

    snmp-server community STRING ro/rw #ro = Read only, rw = read/write

If the NMS try to use a string with 'ro' value, it will not be able to use the
'set' command to change configuration.

Then specify the address of the NMS, Server1, at its IP:

    snmp-server host IP version 2c STRING

In the above command we can specify not only the server, but as well what version
and community string to use.

Next step is to configure what kind of 'Traps' to send to the NMS:

    snmp-server enable traps snmp linkdown linkup #status changes on interfaces

    snmp-server enable traps config #Notify for any config changes

OPTIONALS:

Configure a contact:

    snmp-server contact admin@admin.com

Configure location:

    snmp-server location SITELOCATION
