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

<h4 align="center">Syslog facilities and severity levels</h4>


<h4 align="center">Basic Syslog configuration</h4>
