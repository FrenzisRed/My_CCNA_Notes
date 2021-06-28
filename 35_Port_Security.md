<h2 align="center">Port Security</h2>

<h4 align="center">Intro to Port Security</h4>

Port security is a security feature of Cisco switches. It allows us to control which
source MAC address(es) are allowed to enter the switchport.
If an unauthorized source MAC address enters the port, an action will be taken. The default
one is to place the interface in an "err-disabled" state.

Let's say I bring my laptop at work. I unplug my workstation rj45 cable and plug it
into my laptop. With the first packet the switch will see a new MAC address that is not
been authorized and will turn the interface in err-disabled mode. My laptop will be
unable to connect to the network. So I take back the RJ45 to my desktop....Surprise,
my workstation cannot connect neither.

When we enable port security on an interface with the default settings, one MAC
address is allowed. We can configure the allowed MAC address manually. If we do not
the switch will allow the first source MAC address that enters the interface.

We can change the maximum number of MAC addresses allowed. For example when using
an IP phone. We have a PC connected to the IP phone and then to the switch. Two MAC
will be going through the switch, we need to allow more than one address. Even with
multiples MAC addresses we can still configure the list dynamically.


<h4 align="center">Why use Port Security</h4>

Port security allows networks admins to control which devices are allowed to access
the network, in theory someone cannot just simply plug into a switch and gain access to
the network.

Unfortunately MAC address spoofing is a simple task. It's easy to configure a device to
send frames with a different source MAC address. So rather than manually specifying the
MAC addresses allowed on each port, port security's ability to limit the number of
MAC addresses allowed on an interface is more useful.

For example, let's take the DHCP starvation attack we recently talked about.
The attacker spoofed thousands of fake MAC addresses, the DHCP server assigned IP addresses
to these fake MAC addresses, exhausting the DHCP pool. The switch's MAC address table can
also become full due to such attack.
Limiting the number of MAC addresses on an interface can protect against those attacks.


<h4 align="center">Port Security configuration</h4>

Port security is enabled directly on the switch's interface:

    Switch1(config)#interface g0/1

First we need to check the DTP with the :

    do show interface g0/1 switchport

The line that interest us is the Administrative mode that by default is on dynamic auto, and if it
is, the _switchport port-security_ command will be rejected. The reason been that
port security can be enabled on access or trunk ports, NOT on desirable or Auto.

So we need to manually configure it. As we already know, we do:

    Switch1(config-if)#switchport mode access

Now we can configure the port security.

    Switch1(config-if)#switchport port-security

When using this command by itself, we enable the default settings that we can check with:

    Switch1#show port-security interface g0/1

The outcome would be similar to this:

    Port Security              : Enabled
    Port Status                : Secure-down
    Violation Mode             : Shutdown
    Aging Time                 : 0 mins
    Aging Type                 : Absolute
    SecureStatic Address Aging : Disabled
    Maximum MAC Addresses      : 1
    Total MAC Addresses        : 0
    Configured MAC Addresses   : 0
    Sticky MAC Addresses       : 0
    Last Source Address:Vlan   : 0000.0000.0000:0
    Security Violation Count   : 0

First, port security is enabled, and the port is secure-down, I did not connect it to
anything.
The default Violation mode is Shutdown, err-disabled as explained above.
The Aging time of 0 mins means that they will never age out, there is no timer.
Then we can see details of the MAC addresses, at the moment the maximum is 1, no manually
added, no known addresses and no sticky MAC addresses.
Finally no violation have happened.

In case a violation happens, the port will go to secure-shutdown, the total MAC address
will go to 0 and loose all the learned MAC addresses. The violation will rise to 1.
To re enable the interface we need to disconnect the unauthorized device and re enable
the interface like this:

    Switch1(config)#interface g0/1

    Switch1(config-if)#shutdown

    Switch1(config-if)#no shutdown

There is another way to re-enable an err-disabled interface, the ErrDisable Recovery
It enables err-disabled interfaces to be automatically re-enabled after a certain
period of time. Use the following command to list all ErrDisable reasons:

    show errdisable Recovery

By default it's disabled for all reasons. The one linked to this chapter is called the
psecure-violation and the default timer is 5 min (300 seconds).

To enable the psecure-violation we do:

    Switch1(config)#errdisabe recovery cause psecure-violation

If we want to change the timer, we do:

    Switch1(config)#errdisable recovery interval SECONDS


It's worth noting the why we disconnect the device causing the error. If we did configure
manually the authorized MAC address, once the interface comes back up, it will go right away down.

If we did not configure it manually, the interface will come up and learn dynamically the
new authorized MAC address and will allow this external device and block the original PC.

<h4 align="center">Violation modes</h4>

There are three different violation modes that determinate what the switch will do
if an unauthorized frame enters an interface configured with port security.

- Shutdown

    Effectively shuts down the port by placing it in an err-disabled state.
    Generates a Syslog and/or SNMP message when the interface is disabled.
    Only ONE message is generated even if devices are still trying to get through.
    The violation counter is set to 1 when the interface is disabled.

- Restrict

    The switch discard traffic from unauthorized MAC addresses.
    The interface is NOT Disabled.
    It will generate a Syslog and/or SNMP message each time an unauthorized MAC is detected.
    The violation is incremented by 1 for each unauthorized frame.

- Protect

    The switch discard traffic from unauthorized MAC addresses.
    The interface is NOT Disabled.
    It does NOT generate Syslog/SNMP messages for unauthorized traffic.
    It does NOT increment the violation counter.
    It just silently discard unauthorized traffic.

To set the type of violation mode we do it directly on the interface:

    Switch1(config-if)#switchport port-security violation VIOLATIONMODE

<h4 align="center">Secure MAC addresses aging</h4>

MAC addresses dynamically learned or statically configured on a port-security enabled
port are called secure MAC addresses.

By default secure MAC addresses will not 'age out' (aging time : 0 mins seen above)
We can configure a timer to clean the list:

    switchport port-security aging time MINUTES

If we do this, the aging type seen above will be _Absolute_. Let's have a look to the options:

- Absolute

    After the secure MAC address is learned, the aging timer starts and the MAC is removed
    after the timer expires, even if the switch continues receiving frames from that
    source MAC address. Even if it expire, it can be dynamically re-learned once it sends
    a new frame.

- Inactivity

    After the secure MAC address is learned, the aging timer starts but is reset every time
    a frame from that source MAC address is received on the interface.

To set aging type we do:

    switchport port-security aging type TYPE

By default only the dynamically learned MAC addresses will get washed out by the aging timer.
The static MAC address will not be deleted. But we can make them age too with the command:

    switchport port-security aging static

A very useful command for the switch overview is:

    show port-security

This will show us which interface have port security enabled, the max and current number
of secure addresses on those interfaces, their security violation count and their
security action.

<h4 align="center">Sticky Secure MAC addresses</h4>

_Sticky_ secure MAC address learning can be enabled with the following command:

    Switch1(config-if)#switchport port-security mac-address sticky

When enabled, dynamically-learned secure MAC addresses will be added to the running
config like this:

    switchport port-security mac-address sticky MAC-ADDRESS

The Sticky secure MAC addresses will NEVER age out, even with static aging enabled.
However we need to save the running-config to the startup-config to make them truly
permanent, or they will be lost in case of switch restart.

When we issue the <h4>switchport  port-security mac-address sticky</h4> command, all
currently dynamically-learned secure MAC addresses will be converted to sticky secure
MAC addresses.

The opposite is true as well if we issue the <strong>no switchport  port-security mac-address sticky</strong>

Secure MAC addresses will be added to the MAC address table like any other MAC address.
Sticky and Static secure MAC addresses will have a type of STATIC.
Dynamically-learned secure MAC addresses will have a type of DYNAMIC.
We can view all secure MAC addresses with <strong>show mac address-table secure</strong>
