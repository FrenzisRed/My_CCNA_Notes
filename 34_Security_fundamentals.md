<h2 align="center">Security Fundamentals</h2>

The principles of the <strong>CIA Triad</strong> from the foundation of security:

- <strong>C</strong>onfidentiality

  Only authorized users should be able to access data.
  Some information/data is public and can be accessed by anyone, some is secret
  and should only be accessed by specific people.

- <strong>I</strong>ntegrity

  Data should not be tampered with by unauthorized users.
  Data should be correct and authentic.

- <strong>A</strong>vailability

  The network/systems should be operational and accessible to authorized users.


Attackers can threaten the confidentiality, integrity and availability of an
enterprise's system and information.


<h4 align="center">Key security concepts</h4>

Some important keywords to remember:

<strong>Vulnerability</strong>: A potential weakness that can compromise the CIA
of a system/info. A _potential_ weakness isn't a problem on its own. A window can
be seen as a vulnerability for a house, but still, houses still have windows.

<strong>Exploit</strong>: Something that can potentially be used to exploit the Vulnerability.
Something that can _potentially_ be used as an exploit isn't a problem on its own.
A rock could be seen as an exploit for the windows to enter a house... but they are not
a problem on their own.

<strong>Threat</strong>: A potential of a vulnerability to be exploited. To use previous
example, a threat could be a rubber who wants to use a rock to enter the house.
Or more commonly, a hacker exploiting a vulnerability in your system is a threat.

<strong>Mitigation technique</strong>: is something that can protect against threats.
They should be implemented everywhere a vulnerability can be exploited: clients devices,
servers, switches, routers, firewalls, etc..
Think of it as well on a physical manner, preventing unauthorized persons having
access to servers/systems. Secure them in racks and locked rooms.

<h4 align="center">Always remember: No system is perfectly secure!</h4>


<h4 align="center">Common attacks</h4>

1) DoS (Denial of Service) attacks

2) Spoofing attacks

3) Reflection/amplification attacks

4) Man-in-the-middle attacks

5) Reconnaissance attacks

6) Malware

7) Social engineering attacks

8) Password-related attacks

More in details:

1) DoS attacks threaten the availability of a system one common DoS attack is the TCP SYN flood.
  - TCP three-way handshake: SYN | SYN-ACK | ... no ACK
  - The attacker sends countless TCP SYN messages to the target.
  - The target sends SYN-ACK messages in response to each SYN it receives.
  - The attacker never replies with the final ACK of the TCP three-way handshake.
  - The incomplete connections fill up the target's TCP connection table.
  - The attacker continues sending SYN messages.
  - The target is no longer able to make legitimate TCP connections.
  


<h4 align="center">Password/Multi-Factor Authentication (MFA)</h4>


<h4 align="center">Authentication, Authorization, Accounting (AAA)</h4>


<h4 align="center">Security Program Elements</h4>
