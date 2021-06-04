<h2 align="center">Syslog</h2>

<h4 align="center">Overview</h4>

Syslog is an industry standard protocol for messages logging log that records
events that happens on the device, for example interfaces going up or down,
OSPF neighbor relationship going up or down, System restarts, etc..

The log messages can be viewed in real time in the CLI to warn about events, or
the can be stored on the device or external device to be examined later.

If you train with packet tracer you can easily see Syslogs messages in the CLI.

Syslog and SNMP are both used for monitoring and troubleshooting of devices.
They are complementary, but their functionalities are different.



<h4 align="center">Message format</h4>

These are the fields you can expect in a syslog message:

    seq:time stamp: facility-severity-MNEMONIC:description

<strong>seq</strong>: Asequence number indicating the order/sequence of messages.

<strong>time stamp</strong>: A timestamp indicating the time the message was
generated. Thankfully we used NTP to sync all clocks.

 <strong>These two fields might not be displayed, it depends the device configuration.</strong>

<strong>facility</strong>: It indicates which process on the device generated the
message. Fo example, if PSPF generated a message when an OSPF neighbor came up,
OSPF would be displayed in the facility field.

<strong>severity</strong>: A number to indicate the severity of the event. There
are 8 severity levels and need to know them all.

<strong>MNEMONIC</strong>: A short code for the message, indicating what happened.
For example if the facility is OSPF, the MNEMONIC might be a code indicating that
the message is about OSPF neighbor adjacencies. If the facility is LINK, it might
be a code indicating that the message is about an interface going up or down.

<strong>description</strong>: This contain the detailed information about the event
being reported, about what actually happened.

<h4 align="center">Syslog facilities and severity levels</h4>

Here is a chart of the 8 severity levels:

    ---------------------------------------------------------------------------
    | LEVEL   |  Keyword    |                  Description                    |
    ---------------------------------------------------------------------------
    |   0     | Emergency   |                System is unusable               |
    ---------------------------------------------------------------------------
    |   1     |   Alert     |           Action must be taken immediately      |
    ---------------------------------------------------------------------------
    |   2     | Critical    |                 Critical condition              |
    ---------------------------------------------------------------------------
    |   3     |   Error     |                 Error condition                 |
    ---------------------------------------------------------------------------
    |   4     |  Warning    |               Warning condition                 |
    ---------------------------------------------------------------------------
    |   5     |  Notice     | Normal but significant condition (Notification) |
    ---------------------------------------------------------------------------
    |   6     |Informational|               Information message               |
    ---------------------------------------------------------------------------
    |   7     | Debugging   |               Debug-Level message               |
    ---------------------------------------------------------------------------

Be aware that the level of severity is not the same for each vendor. A juniper
Warning will not be the same as a Cisco warning. For the CCNA I'll focus on Cisco
levels.

Here is a couple of real message example:

    *Feb 11 03:02:55.304: %LINK-3-UPDOWN: Interface GigabitEthernet0/0, changed state to up

    *Feb 11 05:04:39.606: %OSPF-5-ADJCHG: Process 1, Nbr 192.168.1.2 on GigabitEthernet0/0 from LOADING to FULL, Loading Done

The messages can be sent in different locations:

    1)<strong>Console line</strong>: Syslog messages will be displayed in the CLI
    when connected to the device via the consol port. By default, all messages
    (Level 0 to Level 7) are displayed. You can easily test it in Packet Tracer.

    2)<strong>VTY lines</strong>: Syslog messages will be displayed in the CLI
    when connected to the device via Telnet/SSH. This is disabled by default.

    3)<strong>Buffer</strong>: Syslog messages will be saved to RAM. By default,
    all messages from Level 0 to 7 are displayed. We can view the messages with
    the <strong>show logging</strong> command.

    4)<strong>External server</strong>: We can as well configure Syslog to send
    the messages to an external server. This is very useful, especially in large
    networks, but also in small networks. Having a central server for the Syslog<strong>
    messages makes network management easier and makes it easier to compare the
    logs of multiple devices.
    Note that Syslog servers will listen for messages on <strong>UDP port 514</strong>.



<h4 align="center">Basic Syslog configuration</h4>

Configure logging to the console line:

    router(config)#logging console 6

Here above we used 6 as a base layer, that means we will receive only 6,5,4,3,2,1

Configure logging to the vty lines:

    router(config)#logging monitor informational #informational, just as above.

Configure logging to the buffer:

    router(config)#logging buffered 8192 6 # 8192 =  size of the buffer. 6 as above.    

Configure logging to an external server:

    router(config)#logging IPADDRESS

    router(config)#logging host IPADDRESS

    router(config)#logging trap debugging

Now, this is an extra command worth noting as even if the <strong>Logging monitor</strong> _level_
is enabled, by default Syslog messages will not be displayed when connected via
Telnet or SSH.

To display messages, use the following command:

    router#terminal monitor

Note that this command must be used <strong>every time you connect to the device via
Telnet or SSH</strong>.

By default, logging messages displayed in the CLI while we are in the middle of
typing a command will result in something like this:

    router#show ip in
    *Feb 11 09:38:41.607: %SYS-5-CONFIG_I: Configured from console by Frenzis on console
    terface brief

Here I was typing the command _show ip interface brief_ and the Syslog interrupted me.
The command will still execute fine, but it's hard to see where the message ends
and where my command start.

To avoid so we can do this, we should use the <strong>logging synchronous</strong> on the
appropriate _line_. (see Telnet/SSH chapter)

Here are the last two important commands to keep in mind:

    service timestamps

and

    service sequence-numbers

this is how you control if timestamps and sequence numbers will be displayed in
Syslog messages.

To enable timestamps for Syslog messages do:

    service timestamps log daytime/uptime

<strong>daytime</strong> will display the time the event occurred
<strong>uptime</strong> will display how long the device had been running when
the event occurred.

<h4 align="center">Little note</h4>

Let's do a short comparison between Syslog ans SNMP:

Syslog and SNMP are both used for monitoring and troubleshooting of devices. They
are complementary, but their functionalities are different.

<strong>Syslog</strong> is used for message logging. Events that occur within the
system are categorized based on facility/severity and logged.
It's used for system management, analysis and troubleshooting.
Messages are sent from the devices to the server. The server <strong>cannot</strong> actively pull
information from the devices like SNMP <strong>Get</strong> or modify variables like SNMP <strong>Set</strong>.

<strong>SNMP</strong> is used to retrieve and organize information about the
SNMP managed devices. Things like IP addresses, current interface status, temperature,
CPU usage, etc..
SNMP servers can use <strong>Get</strong> to query the clients and <strong>Set</strong> to modify
variables on the clients. 
