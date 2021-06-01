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


<h4 align="center">Versions</h4>

There are several versions of the SNMP, but only 3 have achieved wide-spread use.

<h5>SNMPv1</h5> The original one.

<h5>SNMPv2c</h5>It allows NMS to retrieve large amount of information in a single
request, so it is more efficient and it produce less network traffic.
The 'c' refers to 'community strings' used as passwords in SNMPv1, removed
in SNMPv2, and then added back for SNMPv2c.

<h5>SNMPv3</h5> A much more secure version of SNMP that supports strong encryption
and authentication/ Whenever possible, this version should be used.
This security makes it possible to have only the SNMP intended devices are able
to read the messages, they can't be interpreted and read by an attacker






<h4 align="center">Messages</h4>


<h4 align="center">Basic Configuration</h4>
