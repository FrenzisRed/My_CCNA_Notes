<h2 align="center">SSH = Secure Shell</h2>


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


<h4 align="center">Layer2 switch management IP</h4>

<h4 align="center">Telnet</h4>

<h4 align="center">SSH</h4>
