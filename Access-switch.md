# Access Security :
In this lab we are going to configure a router on stick . where the router will be a DHCP server and the switch is an access switch that contains two vlans .

## Router basic configuration :
### The creation of DHCP server for each vlan :
```
(config)#ip dhcp pool <dhcp_pool_name>
(config)#network <dhcp_pool_network> <dhcp_pool_netmask>
(config)#default-router <default_gateway>
```
### The creation of subinterface for each vlan :
```
(config)#interface gigaEthernet0/0.<vlan_id> ! it's not mandatory to make it vlan_id
(config-if)#no shutdown
(config-if)#encapsulation dot1Q
(config-if)#ip address <vlan_gateway> <netmask>
```

## Access switch basic configuration :
### Commands to recover the port from errdisable :
```
(config)#errdisable recovery cause all 
(config)#errdisable recovery interval 300
```
### The creation of vlans : 
```
(config)#vlan <vlan_id>
(config-vlan)#name <vlan_name>
```
### Enabling dhcp snooping  
ip dhcp snooping vlan 
ip dhcp snooping vlan 10,20
ip arp inspection vlan 10,20
ip arp inspection validate dst-mac ip <h1><h1>
no ip dhcp snooping information option 

ip dhcp snooping database flash:/dhcp-snooping-database.txt
ip dhcp snooping database write-delay 30 


inter range fa0/2-12 
description this ports are for end-devices in vlan 10  
no sh 
switchport host
switch mode access 
switch access vlan 10
spanning-tree portfast
switchport port-security
switchport port-sec max 3
switchport port-sec violation sh vlan
switchport port-sec AGING TYPE INA
switchport port-sec AGING Time 30
ip verify source port-security
ip dhcp snooping limit rate 30
ip arp inspection limit rate 30
storm-control bro level 50 30
storm-control mul level pps 30k 20k
storm-control action trap
storm-control action sh
NO LLDP REC
NO LLDP TR
no cdp en


inter fa 0/1
description this link to the dhcp server
swit mode trunk 
ip dhcp snooping trust
ip arp inspection trust
exit
