DNS, DHCP, and Web Server Setup

This document describes the process I followed to set up DNS, DHCP, and a Web Server using Ubuntu Server 22.04 as the server and Kali Linux as the client.

VM Setup

Client: Kali Linux (already installed)
Server: Ubuntu Server 22.04 (installation guided by YouTube walkthroughs)

VM Configuration:

Both VMs must be on the same LAN network.

In VirtualBox:

Network Adapter: Same internal network for both VMs

NAT: Enabled for Server VM (for internet access)

Resources allocated:

VM	RAM	Storage
Kali	1.5GB	20GB
Ubuntu	1.5GB	20GB

My laptop has 8GB RAM, so resources were kept minimal.

Server credentials:

Username: server

Password: server-ub

DHCP Server Setup

Step 1: Install DHCP server package

sudo apt-get update
sudo apt-get install isc-dhcp-server


Step 2: Install net-tools to use ifconfig

sudo apt-get install net-tools


Step 3: Identify NIC

Example: enp0s3 (may differ for your system)

Step 4: Configure ISC DHCP server

sudo nano /etc/default/isc-dhcp-server


Update the line:

INTERFACESv4="enp0s3"


Step 5: Configure main DHCP file

cd /etc/dhcp/
sudo nano dhcpd.conf


Example configuration:

subnet 10.0.2.0 netmask 255.255.255.0 {
    range 10.0.2.100 10.0.2.200;
    option domain-name-servers 10.0.2.1;
    option routers 10.0.2.1;
    option broadcast-address 10.0.2.255;
    default-lease-time 600;
    max-lease-time 7200;
}


Step 6: Test configuration

sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf


Helped me identify a syntax error caused by a missing ;.

Step 7: Start DHCP server

sudo systemctl start isc-dhcp-server
sudo systemctl status isc-dhcp-server

Client Configuration (Kali Linux)

Step 1: Edit network interfaces

sudo nano /etc/network/interfaces


Add:

auto eth0
iface eth0 inet dhcp


Step 2: Restart networking service

sudo systemctl restart networking
sudo systemctl status networking


Step 3: Check IP

ifconfig


Successfully got IP: 10.0.2.101/24, broadcast 10.0.2.255

Troubleshooting

Use sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf to test DHCP configuration.

Check systemctl status networking.service to ensure network service is running.

Most connection issues were due to syntax errors in dhcpd.conf or wrong network adapter settings.

Next Steps

Install DNS server:

sudo apt-get install bind9 bind9utils bind9-doc


Install Web Server:

sudo apt-get install apache2


Configure DNS zones, web content, and test connectivity between server and client.

References

(Share online resources and links here)

Notes

Images of configuration steps and outputs are included below:










