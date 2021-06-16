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

DDos (Distribute Denial-of-Service) attack is the same as DoS, but the attacker
infected many target computers with malware and uses them all to initiate a DoS
attack. The group of computers is called <strong>botnet</strong>.

2)To <strong>Spoof</strong> an address is to use a fake source address (IP or Mac address).
  - Numerous attacks involve spoofing, it's not a single kind of attack.
  - An example is a <strong>DHCP exhaustion</strong> attack. Similar to the SYN attack.
  - An attacker uses spoofed MAC addresses to flood DHCP Discovery messages.
  - The target server's DHCP pool becomes fool resulting in a DoS to other devices.

3) In a <strong>reflection</strong> attack, the attacker sends traffic to a _reflector_, and spoofs
the source address of its packets using the target's IP address.
  - The _reflector_ (ie. a DNS server) sends the reply to the target's IP address.
  - If the amount of traffic sent to the target is large enough, this can result in a DoS.
  - A reflection attack becomes an <strong>amplification</strong> attack when the amount of
  traffic sent by the attacker is small, but it triggers a large amount of traffic to be sent from
  the reflector to the target.
 4) in a man-in-the-middle attack, the attacker places himself between the source and destination
 to eavesdrop on communications, or to modify traffic before it reaches the destination.
  - A common example is <strong>ARP spoofing</strong>, also konwn as <strong>ARP poisoning</strong>.
  - A host sends an ARP request, asking for the MAC address of another device.
  - The target of the request sends an ARP reply, informing the requester of its MAC address.
  - The attacker waits and sends another ARP reply after the legitimate replier.
  - If the attacker's ARP reply arrives last, it will overwrite the legitimate ARP entry
  in PC1's ARP table.
  - The result is that in PC1's ARP table, the entry for the server IP will have the attacker MAC address.
  - When PC1 tries to send traffic to the server, it will be forwarded to the attacker instead.
  - The attacker can inspect/modify the messages and then forward them to the server.
  - This type of attack compromise the C and I of communications.

4) Reconnaissance attacks aren't attacks themselves, but they are used to gather information
about a target which can be used for a future attack.
  - This is often  publicly available information.
  - ie: nslookup to learn the IP address of a site.
  - Or a WHOIS query  to learn email addresses, phone numbers, physical address, etc..

5) Malware (Malicious Software) refers to a variety of harmful programs that can infect a computer.
  - <strong>Viruses</strong> infect other software (a 'host program'). The virus spreads as the software
  is shared by users. Typically they corrupt or modify files on the target computer.
  - <strong>Worms</strong> do not require a host program. They are standalone malware
  and they are able to spread on their own, without user interaction. The spread of worms
  can congest the network, but the 'payload' of a worm can use additional harm to target devices.
  - <strong>Trojan Horses</strong> are harmful software that is disguised as legitimate software.
  They are spread through user interaction such as opening email attachments, or downloading
  a file from the internet.

  They can all exploit different type of vulnerabilities.

6) Social engineering attacks target the most vulnerable part of any system - people!
  - They involve psychological manipulation to make the target reveal confidential information
  or perform some action.
  - <strong>Phishing</strong> typically involves fraudelent emails that appear to come
  from a legitimate business (Amazon, bank, credit card company, etc..) and contains
  links to a fraudulent website that seems legitimate. Users are told to login to
  the fraudulent website, proving their login credentials to the attacker.
  - <strong>spear phishing</strong> is a more targeted from phishing, ie. aimed at employees of a certain company.
  - <strong>whaling</strong> is phishing targeted at high-profile individuals, ie. a company president.
  - <strong>Vishing</strong> is phishing over the phone.
  - <strong>Smishing</strong> is phishing using SMS text messages.
  - <strong>Watering hole</strong> attacks compromise sites that the target victim frequently
  visit. If a malicious link is placed on a website the target trust, they might not hesitate to click it.
  - <strong>Tailgating</strong> attacks involve entering restricted, secured areas by simply
  walking in behind an authorized person as they enter. Often the target will hold the door open for the
  attacker to be polite, assuming the attacker is also authorized to enter.

7) Password related attacks.
  - Most systems use a username/password combination to authenticate users.
  - The username is often simple/easy to guess (ie. user's email address), and the strength and secrecy
  of the password is relied on the provided necessary security.
  Attackers can learn a user's password via multiple methods:
  - Guessing - very rare, but it does exist.
  - <strong>Dictionary attack</strong>: a program runs through a 'dictionary' of common
  words/passwords to find the target's password.
  - <strong>Brute force attack</strong>: A program tries every possible combination of letters,
  numbers and special characters to find the target's password.
  - Strong passwords should contain: at least 8 characters, preferably more, a mixture of Upper case and lowercase letters.
  A mixture of letters and numbers. One or more special characters (#@?! etc..). It should
  be changed regularly.

<h4 align="center">Password/Multi-Factor Authentication (MFA)</h4>

<strong>Multi-factor authentication</strong> involves providing more than just a username/password
to prove your identity. It usually involves providing two of the following ( two factor authentication):
  - <strong>Something you know</strong>: a username/password combination, a PIN, etc..
  - <strong>Something you have</strong>: Pressing a notification that appears on your phone,
  a badge that is scanned, etc..
  - <strong>Something you are</strong>: biometrics such as a face scan, palm scan, fingerprint scan
  retina scan, etc..

Requiring multiple factors of authentication greatly increases the security. Even
if an attacker learns the target's password (<strong>Something you know</strong>), they won't
be able to login to the target's account.

<strong>Digital certificates</strong> are another form of authentication used to prove the identity
of the holder of the certificate. They are used for websites to verify that the website being
accessed is legitimate.
- Entities that want a certificate to prove their identity send a CSR (Certificate Signing Request)
to a CA (Certificate Authority), which will generate and sign the certificate.

<h4 align="center">Authentication, Authorization, Accounting (AAA)</h4>

AAA it's a framework for controlling and monitor users of a computer system.

<strong>Authentication</strong> is the process of verifying a user's identity. Logging = authentication.
<strong>Authorization</strong> is the process of granting the user appropriate access and permissions.
Granting the user access to some files/services, restricting access to other files/services = authorization.
<strong>Accounting</strong> is the process of recording the user's activity on the system.
Logging when a user makes a change to a file = accounting.

Enterprises typically use a AAA server to provide AAA service. ISE (Identity Service Engine) is
Cisco's AAA server.

AAA servers usually support the following two AAA protocols:
  - RADIUS: an open standard protocol, uses UDP ports 1812 ans 1813.
  - TACACS+: a Cisco propriety protocol, uses TCP port 49.

<h4 align="center">Security Program Elements</h4>

<strong>User awareness</strong> programs are designed to make employees aware of potential security threats
and risks. IE. a company might send out false phishing emails to make employees click a
link and sign in with their login credentials.
Although the emails are harmless, employees who fall for the false emails will be informed
that it is part of a user awareness program and they should be more careful about phishing.

<strong>User training</strong> programs are more formal than user awareness programs.
Dedicated training sessions which educate users on the corporate security policies, how to create
strong passwords, and how to avoid potential threats.

<strong>Physical access control</strong> protects equipment and data from potential
attackers by only allowing authorized users into protected areas such as network closets or data center floors.
Multifactor locks can protect access to restricted areas.
