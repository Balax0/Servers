This repo is for my DNS server setup
server ubuntu client kali
make sure to make them in a same network

starting by downloading bind9 and related packeges to it 
>> sudo apt install bind9
>> bind9 is just a alias name for named, so when we try to run the bind9 we should use
>> --- systemctl enable named
>> --- systemctl start named
>> --- systemctl status named ("check for the server status, it should be active")
>>
>> navigate to file at /etc/bind/named.conf.local ("main file to add zone details")
>>save and quit
>> 
>> now create zone file (just copy from a base file db.local)
>> cp /etc/bind/db.local /etc/bind/db.bala.local
>> nano /etc/bind/db.bala.local -- edit this file
>>
>> cp /etc/bind/db.127 /etc/bind/db.192
>> nano /etc/bind/db.192 -- edit this file 
>>
>> Check the configurations systax
>> sudo named-checkconf
>> sudo named-checkzone bala.local /etc/bind/db.bala.local
>> sudo named-checkzone 148.168.192.in-addr.arpa /etc/bind/db.192
>>---------It should be a OK message------------
>>
>> restart bind9 ---- systemctl restart bind9
>>
>> configure the client kali machine ----------
>>
>>get into nano /etc/resolve.conf
>> nameserver 192.168.148.128
>> 
>>

>>
>> 
