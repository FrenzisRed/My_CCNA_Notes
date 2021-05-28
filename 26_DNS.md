<h2 align="center">DNS = Domain Name System</h2>

Purpose:

DNS makes it easy to access resources on the network resolving names.

For example it makes us easy to access google.com instead on having to type an IP address.

The DNS will convert the domain name into the IP address required.

The DNS server can be manually configured or learned via DHCP.

You can see the details on the DNS by doing a nslookup of a website.
you will discover the IP address and the used DNS server.

If you check with wireshark you can follow the exchange with the DNS.
For example if we query for youtube.com we will get:
  from pc to DNS standard query 0x0002 A youtube.com
  from DNS to PC standard query 0x0002 A youtube.com A 172.217.25.110
  from PC to DNS standard query 0x0003 AAAA youtube.com
  from DNS to PC standard query 0x0003 AAAA youtube.com AAAA 2404:6800:4004:819::200e

A stands for mapping IPv4 addresses
AAAA stands for mapping IPv6 addresses

If you have a closer look on layer 4 you can see that DNS is using UDP, but we
know that it can use UPD and TCP.
Standard DNS queries/responses typically use UDP. TCP is used for DNS messages
greater than 512 bytes. In either case, port 53 is used.  

Note that devices will save the DNS server's responses to a local DNS cache.
This mean they don't have to query the server every single time they want to access
a particular destination. It save unnecessary traffic.

On a PC you can check the local cache by typing:

  ipconfig /displaydns

Don't be surprised id you do not have an IP address there, but find a CNAME.
A CNAME, canonical name, is another kind of DNS record that basically maps a name
to another name.
If you check the record for that CNAME you will probably find the IP record.

Note that you can clear the DNS cache on a PC with a simple command:

  ipconfig /flushdns

Note that in addition to a DNS cache most devices have a hosts file which is
simply a list of hosts and IP addresses.

Final note:
  For hosts in a Network to use DNS you do not need to configure DNS on the routers.
  The routers will simply forward the DNS messages like any other packets.
  However, the Cisco's router itself can be configured as a DNS server, although
  it's pretty rare.

  If an internal (a DNS in the local Network, not a public one like google) DNS
  server is used, usually it's a Windows or Linux server.

  A Cisco router can be configured as a DNS client, so you can execute PING and
  other commands using names instead of IP addresses.

<h4 align="center">Configuring DNS in Cisco IOS:</h4>

To configure a router as a DNS server use the "ip dns server" command.
Then add records like this:

  ip host NAME1 IP1
  ip host NAME2 IP2
  ip host NAME3 IP3
  ...
To add an external one to use do:

  ip name-server IPADDRESS

This external record will be used if the router does not have the record of the query.

At the end you should run:

  ip domain lookup # previous IOS version: ip domain-lookup

This will enable the router to perform DNS queries.   

To view both the configured hosts, as well as the hosts and cached via DNS, use
the command in the IOS:

  show hosts

in the list you can see flags that will show you temporary or permanent records.
Permanent records means that they have been manually configured.
Temporary means that the server learned it, when it will expire, it will have to
learn it again.

To configure a Cisco router as a DNS client do:

  ip name-server IPADDRESSOFDNS

  ip domain lookup

Done. But there is one more optional command that we can use:

  ip domain name DOMAINNAME # older but still supported: ip domain-name DOMAINNAME

When this command is applied, this will be a default domain name applied to all
hostnames without a specifies domain name.

For example, let's say we have 2 PC and we record it in the router as PC1 and PC2.
In the router we do:

  ip domain name CCNA.com

Now if from PC1 I do the command 'ping PC2' it will become 'ping pc2.ccna.com'
