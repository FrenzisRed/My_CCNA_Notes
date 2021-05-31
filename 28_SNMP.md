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

Little example using this network:

----------           --------              --------               -------
| internet|----------| Router|-------------|Switch|---------------| PC1 |
----------           ---------             --------      |        -------
                                               |         |________| PC2 |
                                               |         |         ------
                                               |         |________| PC3 |
                                           ------------            ------
                                           |NMS Server|
                                           ------------


<h4 align="center">Versions</h4>


<h4 align="center">Messages</h4>


<h4 align="center">Basic Configuration</h4>
