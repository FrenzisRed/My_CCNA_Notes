NTP = Network Time Protocols

Why is this important? Well, all devices in the network have an internal clock
and need to be in sync.
Imagine having to troubleshoot a network and having logs that do not match
event time, this will quickly become a nightmare.

There are 2 available times in Cisco's devices, Software (called clock) and
hardware (called calendar). Be aware that Software and Hardware clock are independent.

The internal hardware clock of a device drift over time, so it is not the ideal
time source.

We can adjust both manually or sync one to the other one manually.

Manual configuration, some commands:

  In Cisco IOS you can view the time with "show clock" command.

  With "show clock detail" you can see the time source

  To manually configure the time use the "clock set" command.

    structure of the command:

      clock set H:M:S DAY Month Year

  To manually configure the Hardware clock use the "calendar set" command.

    Structure of the command:

      calendar set H:M:S DAY Month Year

  To sync the calendar to the clock use "clock update-calendar" command.
  To sync the clock to the calendar use "clock read-calendar" command.

To configure the time zone use the "clock timezone" command.
Be aware that the timezone must be set while global config mode as it's part of
the running device. All the rest is done in privilege exec mode.

  command structure:

    clock timezone ZONENAME OFFSET

Setting automatic daylight saving time if needed:

  "clock summer-time" from global config mode
   Structure:

      clock summer-time TIMEZONENAME DATE/Recurring Week WeekDay Month HOUR WEEK WeekDay Month HOUR [OFFSET]

    Options:

    date = set specific date
    recurring = summer time starts and end on the given dates
    week =  which week of the month it occur
    WeekDay = the week day it occur
    Month = Month it occurs
    Offset = Specify the offset, but default is 60 minutes

    Example, let's set it to occur on the second week of March on Sunday at 2:00
    and stop the First week of November at 2:00:

      Clock summer-time EU recurring 2 Sunday March 2:00 1 Sunday November 2:00


Manually configuring is not duable and not optimized.
NTP allows automatic syncing over a network.

We sync via the Network so NTP clients request time from NTP servers.
A device can be an NTP server and client at the same time.

NTP allows accuracy of time within ~1 millisecond if the server is in the same LAN,
or within ~50 millisecond if connecting over the WAN/Internet.

Some NTP servers are better than others. The distance of an NTP server from the
original reference clock is called "stratum".

NTP use UDP port 123

NOTE: a reference clock is usually a very accurate time device like an atomic Clock
      or a GPS clock.

      Reference clocks are Stratum 0 within the NTP hierarchy.
      NTP servers directly connected to reference clocks are Stratum 1.
      It goes up to stratum 15, over 15 is considered unreliable, the NTP will not
      syncronise to it.
      Same level Stratum can be connected to each other, calling this "symmetric active" mode.
      Cisco can operate in 3 NTP modes:

        Server mode
        Client mode
        Symmetric active mode

      They can bee in all modes at the same time.

      Stratus 1 are primary servers, the others are simply secondary servers.

To configure NTP use the command below, more then one server can be added:

    ntp server IPADDRESS1
    ntp server IPADDRESS2
    ntp server IPADDRESS..

The one that will be used will be automatically selected by the response time.
In case it will slow down or not respond at all, it will automatically switch to
another given server.

If you want to manually specify a preferred server, just add prefer after the IP:

    ntp server IPADDRESS prefer

To see the current state use the command "show ntp associations".

Example:

    address:        ref clock    st  when  poll  reach  delay     OFFSET    disp
    *~216.239.35.0    .GOOG.      1   43    64    17    62.007    1401.54   0.918
    +~216.239.35.8    .GOOG.      1   43    64    17    64.220    1415.54   0.939
    ...

This will show you the list of used servers.
An asterisk (*) will show the one NTP is using.
The plus sign (+) shows candidates that are ready, but not currently used.
The tilde (~) means they were configured, nothing else.
If there is a '-' or an 'x' it means that they will not be used to sync.

Use "show ntp status" command to check status.

Now to check clock and calendar details use the "do show clock detail" command.

Be aware that:

    NTP use only the UTC timezone, you must configure the appropriate
    time zone on each device.
    NTP does not update the calendar by default. Do it via "ntp update-calendar"

Why to sync the hardware clock? Because it tracks the date and time on the device
even if it restarts, power is lost, etc.. When the system is restarted, the hardware
clock is used to initialize the software clock.

In small Networks we would just configure all device to sync to public NTP servers
like Google's.
But in bigger Networks you would use internal routers to be servers for the other ones.

To start you should configure a loopback address on the 'server router':

    interface loopback0
    ip address 10.1.1.1 255.255.255.255
    exit
    ntp source loopback0  (this will be the source of the NTP messages for the network)

Why do we use a loopback? Because in the case of a failure in the shortest path,
The loopback address will still be advertised by the other routers and will
still be reachable. It makes it interface independent.

So now we just need to set "ntp server 10.1.1.1" on the other routers to be able to
sync with our NTP server.

Now, what if we do not sync to an NTP server, but we want our routers to sync?

We use the command "NTP master" to create a master clock in our network:

    ntp master [stratus level] # default level is 7

To configure symmetric active mode on same level of Stratum use these commands:

    ntp peer IPADDRESS

This helps for precision and backup

NTP authentication is the last part of NTP configuration and it's optional.
This allows NTP clients to ensure they only sync to the intended servers.

To configure the NTP authentication use:

    ntp authenticate # enable authentication

    ntp authenticate-key 'key-number' md5 key # creation of keys

key-number is the reference you give it
key is the password itself

Then you need to specify what key/keys are trusted via the command:

    ntp trusted-key 'key-number'

Then you specify which key to use for each server:

    ntp server IPADDRESS key 'key-number' #not needed on the server itself
