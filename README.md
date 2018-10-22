# OSYS-3030
Gateway Services Wrap-Up

For the first 7 weeks of our Network Services using Linux course, we put together an appliance that was a miniture network. This appliance was able to support various services that are critical for
organizations to run properly and efficiently. From our servers, we were able to provide a client machine with a IP address assigned by DHCP, using DNS, we were able to resolve domain name issues 
and grant internet access to our clients. Continuing on, we installed a firewall, to help maintain and control what was able to come in and out of our network. Finally, we added a caching and 
forwarding service known as "Squid", which allowed us to speed up our web server by caching requests for people sharing network resources, as well as aiding in security by filtering traffic.

# DNS 

Domain Name Service (DNS) is a internet service which maps addresses fully qualified domain names (FQDN) to one another. A DNS server is a computer server that contains a database of puublic IP 
addresses and their hostnames, and serves to resolve or translate common names to IP addresses as requested.

# DNS Installation & Configuration

To first install DNS onto my name server, I first logged into the server under my username. After this, I entered the command " sudo install bind9 ". Once bind9 was installed, I entered
the command " sudo install dnsutils ", dnsutils is a package for testing and troubleshooting DNS issues. After this, I added forwarders for the IP addresses of my ISP's DNS server. Following after that
I created a forward zone file to add to "bind9", turning it into the primary master for the domain. Once done creating and editing the sysnija named.conf.local and reverse files, filling in my own
information needed, I restarted the "bind9" service so the changes would take effect. After chcking the "syslog" files to be sure it worked, I did some testing of the service and and with a few changes
made, the service was up and running.    


# DHCP

Dynamic Host Configuration Protocol (DHCP) is anetwork service that enables host computers to be automatically assigned settings from a server instead of manually configuring each network host. Once
and computer is configured with DHCP, then it has no control over what settings it recieves from the DHCP server. The 3 most common settings provided by DHCP are IP address and netmask, IP addresses
of the default-gateway being used and the IP addresses of the DNS servers to use.

# DHCP Installation & Configuration

To first install DHCP on my server, I used "sudo install isc-dhcp-server". After the package was installed, I procedded to edit the "dhcpd.conf" file so that it would randomly assign an IP addresses 
within a set range by changin the leases. Upon finishing the editing, I restarted the DHCP service so that the changes would take effect. After issuing the "sudo service isc-dhcp-server status" command,
I was able to verify the system was active, and after cheecking the leases, able to see the address range beomg assigned. 


# Firewall

Firewalls are extremly important to netowrks and organizations. The help restrict and deny anything that could be seen as harmful to our networks. The Linux kernel includes the "Netfilter" subsystem, 
which is used to manipulate or decide the fate of traffic destined for our networks or through our server. 

# Firewall Installation & Configuration

To configure my firewall settings, I first made sure I had "ufw" enabled. "ufw" refers to uncomplicated firewall, which is the default configuration tool for Ubuntu. Upon finishing this I made sure
to allow access to any ports that I may need such as 22 for SSH, and put in place any rules to deny and close any ports not being used. After editing the default ufw rules in the ufw directory, 
adding a rule so that the host was to route packets between interfaces in the sysctl.conf file and editing some before rules to allow forward traffic on my interfaces between server and client, My 
firewall was up and running. 

After we had our firewalls active, we enabled IP Masquerading. IP Masquerading's purpose is to allow machines with private, non-routable IP addresses on our network to access the internet through the 
machine doing the masquerading. Traffic from your private network destined for the internet is manipulated for replies to be routable back to the machine that made the request. You may set up IP 
masquerading in two ways, using ufw masquerading or iptables masquerading. After using one or the other to enable masquerading, checking your log files is a good way to troubleshoot your firewall rules
and notice anything acting unusual on your network. To turn logging on simply emter the command "sudo ufw logging on" if using ufw.    


# Squid

Squid is a web proxy cache server application which provides proxy and cache services for HTTP, FTP, and other network protocols. It is also able to cache DNS lookups and and proxying of SSL requests.
This proxy cache server is an wonderful soloution to a variety of needs and services. When selecting a computer dedicated for Squid, be sure it is configured with a large amount of physical memory. 

# Suqid Installation & Configuration

To install Squid, first enter the command "sudo install squid". Squid is configured by editing the naratives contained in the squid.conf configuration file. Using squids access control, configuring 
use of the internet services proxied by squid to be available only to users with certain IP addresses is made easier. First add the source IP address to the bottom of the ACL section of the squid.conf
file. After this, at the top of the http_access section allow the named network. If you choose so, Squid can configure use of internet services to be available only during normal business hours. After 
finishing your changes, restart the service to be sure effects take place.

# Description of Topology

Two servers, one running Ubuntu server 18.04, the other with CentOS 7 minimal. Both Servers have DNS, DHCP, Firewall and Squid services running on them to provide for my Fedora client machine. Both
servers have to network interface cards (NIC's), one internal and one external used for accessing and providing services. The two servers and client machine are apart of my port group which helps 
aggregate multiple ports under a common configuration providing a stable anchor point connecting to labeled networks. The servers are connected to a virtual switch providing connectivity between the
VM's.

All of this is was created from a datastore hosted on VMware ESXi on the 172.16.144.27 network.
