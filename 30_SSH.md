<h2 align="center">SSH = Secure Shell + General Security</h2>


<h4 align="center">Console Port Security</h4>

By default, no password is needed to access the CLI of a Cisco IOS device via
the console port.
We can configure a password on the console _line_. When this is set up, the user
will have to enter a password to access the CLI via the console port.

Here is an example of setting this up:

    router(config)#line console 0

There is always a single console line, so the number is 0. Only one console
line means that there is always only one connection that can configure the device
at the time, no multiple configurations connections will be possible.

    router(config-line)#password YOURPASSWORD #setting the actual password

    router(config-line)#login

This tells the device to prompt for a password at login when connecting to the
CLI via the console port.

    router(config-line)#end

We can as well configure the console line to require  users to login using one of
the configured usernames on the device. Here is how:

    router(config)#username YOURUSER secret THEPASSWORD

    router(config)#line console 0

    router(config-line)#login local # this tells the device to use local users.

    router(config-line)#end

Another important command is the:

    exec-timeout

This will cause the device to log the user out after a certain time of inactivity.


<h4 align="center">Layer2 switch management IP</h4>

Routers and Layer3 Switches have IP addresses we can use to connect remotely to
manage the devices.
For Layer2 switches we can set up a SVI with an IP address so we can connect to
the switch CLI console remotely using SSH or Telnet.

For the rest of the notes I'm using this network topology:


    -------   ---------   ----------    ----------    ---------        -------
    | PC1 |---|Switch1|---| Router1|----| Router2|----|Switch2|--------| PC2 |
    -------   ---------   ----------    ----------    ---------        -------

The Network Admin will be using PC2

Reminder to configure an SVI you need these commands:

    Swithc(config)#interface vlanID

    Swithc(config-if)#ip address IPADDRESS MASK

    Swithc(config-if)#no shutdown

    Swithc(config-if)#exit

    Swithc(config)#ip default-getaway GETAWAYIPADDRESS

Here we are configuring an IP address on the SVI as we do for a multilayer switch

We configure the switch default gateway on Switch1 as PC2 is not in the same LAN.
Swithc2 does not need that as it's in the same LAN as PC2.

<h4 align="center">Telnet</h4>

First of all note that Telnet has a lack of security and it's not been used widely
anymore. Telnet (teletype network) is a protocol used to remotely access CLI of
a remote host.

<h4 align="center">SSH</h4>
