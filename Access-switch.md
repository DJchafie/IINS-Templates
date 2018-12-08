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
### Enabling dhcp snooping :
```
(config)#ip dhcp snooping vlan 
(config)#ip dhcp snooping vlan [vlan_id]
```
### Enabling arp inspection :
```
(config)#ip arp inspection vlan [vlan_id]
(config)#ip arp inspection validate dst-mac ip
```
### Removing the dhcp option 82 : 
```
(config)#no ip dhcp snooping information option 
```
### DHCP snooping database saving :
if the access switch goes down the snooping database will be deleted so we have to save it :
<br>
save it in the file dhcp-snooping-database.txt every delay_in_sec 
```
(config)#ip dhcp snooping database flash:/dhcp-snooping-database.txt
(config)#ip dhcp snooping database write-delay <dalay_in_sec> 
```
### Configuration interface to the DHCP Server : 

```
(config)#inter fastEthernet0/1
(config-if)#description this link to the dhcp server
(config-if)#switchport mode trunk 
(config-if)#ip dhcp snooping trust
```
### Configuration interfaces of each vlan : 

```
(config)#inter range fa0/2-12 
(config-if)#description this ports are for end-devices in vlan 10  
(config-if)#no sh 
(config-if)#switchport host
(config-if)#switch mode access 
(config-if)#switch access vlan 10
(config-if)#spanning-tree portfast
(config-if)#switchport port-security
(config-if)#switchport port-sec max 3
(config-if)#switchport port-sec violation sh vlan
(config-if)#switchport port-sec aging type inactivity
(config-if)#switchport port-sec aging time 30
(config-if)#ip verify source port-security
(config-if)#ip dhcp snooping limit rate 30
(config-if)#ip arp inspection limit rate 30
(config-if)#storm-control bro level 50 30
(config-if)#storm-control mul level pps 30k 20k
(config-if)#storm-control action trap
(config-if)#storm-control action sh
(config-if)#no lldp rec 
(config-if)#no lldp tr 
(config-if)#no cdp en
```
## Using ip verify source with port security : 
### Creation of dhcp class :
```
(config)#ip dhcp class <class_name>
(config)#relay agent information
```
### Assign the dhcp class to the dhcp server : 
```
(config)#ip dhcp pool <pool_name>
(config)#class <class_name>
(config)#address range <range_begin> <range_end>
```
### Make the switch and router trust end-devices with dhcp option 82 : 
```
(config)#ip dhcp relay information trust-all
```
