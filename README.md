So, this will be my sample doc about the process i have done to setup the DNS, DHCP, Web Server 

client - for my client im gonna use kali linux i already have it, so im gonna use it as client 

server - for my server im installing ubuntu server 22.04, which i get to know about from the youtube walkthrough videos 

>> I need to make sure that both my client (kali) and server (ubuntu) should be in a same lan network 

>> To make that i think i need to make some changes in my virtual box like for both VM's same internal network adaptor && NAT for Server VM 

>> After installation of ubuntu server, I gonna install "bind9, bind9util, bind9doc" for DNS server setup, 

>>For dhcp I use "isc-dhcp-server" (a popular, open-source server that implemented Dynamic Host Configuration Protocol (DHCP) to automatically assign IP addresses and other network configurations to devices) 
  
>>For web server "apache2" (the Apache HTTP Server, a widely-used open-source web server software that serves websites) 

*****  And there are online resources that helped me to setup the servers if possible ill share the exact links of the resources  *****

And for my vm's setup -- kali is running in 1.5gb of ram and 20gb of storage
for my server ubuntu, I'm planning to allocate the same ram and storage memory, hence i only have a 8gb ram in my actuall laptop pc.


-------------------------server username and password (user name server, pass - server, server-ub )--------------------------------------------
Okay, i have done my VM server installation
now lets proceed with the server setups------------------------DHCP---------------------------------------------------------------------------- 



lets starts with the dhcp server setup
>> packages to be installed - isc-dhcp-server 

<img width="811" height="395" alt="isc install" src="https://github.com/user-attachments/assets/e12983e9-3a4d-4465-8ff1-c0057ba199e4" />



>> and to use ifconfig we should install nettools
for this the command is sudo apt-get install nettools



>> nic add -- enp0s3 (will be differ for you guys)



>> sudo nano /etc/default/isc-dhcp-server  > enter
  change the settings 
  interfacesv4= " enp0s3 "

<img width="800" height="600" alt="nic name" src="https://github.com/user-attachments/assets/58d62277-d561-4815-870e-997ced32dc15" />




>>main dhcp file configuration
  cd /etc/dhcp/
  ls
  sudo nano /etc/dhcp/dhcpd.conf  > enter
  update subnetting options
  subnet 10.0.2.1  netmask 255.255.255.0



<img width="800" height="600" alt="main-dhcp-updated" src="https://github.com/user-attachments/assets/8e7b7870-d648-4cdd-9acb-2238bddab391" />



>>command the dns, dn because we doesnt have a dns server yet( hence it is dhcp server connection)
  option subnetmask 255.255.255.0
  option boardcast 10.0.2.255
  crtl o enter crtl x



>> start the server 
sudo start isc-dhcp-server
sudo status "  "   "

<img width="800" height="600" alt="running server" src="https://github.com/user-attachments/assets/f6de53e9-72e4-4ec1-885b-a4f14a2b591d" />




after get into the client os to establish the connection
login into kali machine ( my client ) 

check ifconfig
>> sudo nano /etc/network/interfaces
>>update -- auto eth0, iface eth0 inet dhcp

<img width="955" height="910" alt="interface_up" src="https://github.com/user-attachments/assets/01c043be-665c-4035-a1ce-a6f4fcd35313" />




## troubleshooting is going on >i cant find the dhcp server with my client server its been 2 hrs still troubleshooting going on
that problem is i cant connect the two vms in a same network

<img width="955" height="910" alt="interface error" src="https://github.com/user-attachments/assets/16fc835e-dde5-4a39-84a3-fba23e17276c" />


 
>>to trouble shoot any errors in dhcpd.conf use " sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf "
hence it helped me to solve the problem

## finally found the error because of a " ; " .  :( :(


>> type systemctl status networking.service
and got the service running 
now my kali linux got the ip of 10.0.2.101/24, 10.0.2.255

<img width="955" height="910" alt="successfulconnection" src="https://github.com/user-attachments/assets/471f52b3-0b3f-40de-b004-e01e00e7dde7" />


<img width="1920" height="1080" alt="Screenshot 2025-10-03 181833" src="https://github.com/user-attachments/assets/abe1789b-c551-4e4b-a783-a86c12f74f2a" />













  
