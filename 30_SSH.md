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

    Switch(config)#interface vlanID

    Switch(config-if)#ip address IPADDRESS MASK

    Switch(config-if)#no shutdown

    Switch(config-if)#exit

    Switch(config)#ip default-getaway GETAWAYIPADDRESS

Here we are configuring an IP address on the SVI as we do for a multilayer switch

We configure the switch default gateway on Switch1 as PC2 is not in the same LAN.
Swithc2 does not need that as it's in the same LAN as PC2.

<h4 align="center">Telnet</h4>

First of all note that Telnet has a lack of security and it's not been used widely
anymore. It sends data in plain text, no encryption!
Telnet (teletype network from 1969) is a protocol used to remotely access CLI of
a remote host.

For information only, here is how to configure a device for Telnet connections:

    Switch(config)#enable secret PASSWORD

If no password is set, Telnet cannot be enabled tu use privileged exec mode.

    Switch(config)#username USERNAME secret password

This allows us as seen before to do a username and password log in.

    Switch(config)#access-list 1 permit host PC2IP

This is not necessary, but limiting the access to the VTY lines is always better.
(VTY = Virtual TeleType)

    Switch(config)#line vty 0 15

Telnet/SSH access id configured on the VTY lines. There are 16 lines available,
so up to 16 users can be connected at the same time.

    Switch(config-line)#login local

    Switch(config-line)#exec-timeout 5 0

    Switch(config-line)#transport input telnet

this allows only Telnet connections, other interesting lines are:

    transport input ssh

    transport input telnet ssh

    transport input all

    transport input none

I think it's pretty self explanatory.

    Switch(config-line)#access-class 1 in

Last one to apply the ACL input so that only PC2 will be able to connect to the CLI.

<h4 align="center">SSH</h4>

SSH (secure shell) was developed in 1995 to replace less secure protocols like Telnet.

SSHv2, a major revision of SSH, was released in 2006.

If a device supports both versions it is said to run version  1.99

SHH provides security features like data encryption and authentication.

Before configuring SSH on the device, make sure it is supported by using:

    Switch1#show version

Supporting images will have a "K9" in their name.

Cisco exports NPE (no payload Encryption) IOS images to countries that have
restrictions on encryption technologies. These no dot support SSH cryptographic keys.

To enable and use SSH we must generate RSA public and private keys pair.
The Keys are used for data encryption/decryption, authentication,etc..

Let's see the steps to configure it:

    Switch(config)#hostname HOSTNAME

Needed to generate the keys, name your devices.

    Switch(config)#ip domain name DOMAINNAME

This is necessary as the FQDN of the device is used to name the RSA Keys.
FQDN = Fully Qualified Domain Name (host name + domain name)

    Switch(config)#crypto key generate rsa

and then specify the length, or do:

    Switch(config)#crypto key generate rsa modulus 'length'

This will skip the question on how long (1024-2048-4096 etc..)
The length must be 768 bits or greater to use SSHv2
Once generated a Syslog message will be displayed to signal ssh is enabled:

    *Jun 09 10:54:35.708: %SSH-5-ENABLED: SSH 1.99 has been enabled

To check just do:

    Switch(config)#do show ip ssh

  you will note the SSH Enables line.

Now that it's enables, let's go over the configuration:

    Switch(config)#enable secret PASSWORD

    Switch(config)#username USERNAME secret password

    Switch(config)#access-list 1 permit host PC2IP

Just as before so we can restrict access.

    Switch(config)#ip ssh version 2

This last command is optional but recommended, we restrict to SSHv2.

        Switch(config)#line vty 0 15

        Switch(config-line)#login local

        Switch(config-line)#exec-timeout 5 0

        Switch(config-line)#transport input ssh

        Switch(config)#access-list 1 in
