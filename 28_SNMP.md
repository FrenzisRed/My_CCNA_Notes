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

    the <h5>NMS Manager</h5> is the software on the NMS that interacts with the managed devices.
    It receives notifications, sends requests for information, send configuration
    changes, etc..

    The <h5>SNMP Application</h5> provides an interface for the network admin to interact with.
    It display alerts, statistics, charts, etc..


<h4 align="center">Versions</h4>


<h4 align="center">Messages</h4>


<h4 align="center">Basic Configuration</h4>
