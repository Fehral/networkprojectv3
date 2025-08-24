<p align=center>
  <ins>Overview of Concepts</ins>
</p>

Serial DCE and DTE  
  * Data Communication Equipment refers to hardware devices that process telecommunications systems and converts them to the proper signal for a given medium
  * DCE is used to establish, maintain, and terminate network sessions between a data source and destination; transmits clock signals
    * In Cisco Packet Tracer, DCE and DTE interfaces can be differentiated by the presence of a clock symbol on an interface tooltip
      <p align=center>
        <img src="https://github.com/Fehral/networkprojectv3/blob/main/serialdcedte.png?raw=true">
      </p>
  * Data Terminal Equipment refers to end devices from which telecommunications signals originate, e.g., computers, phones, fax machines, printers
  * DTE sends telecommunications signals to a DCE for processing and conversion

SSH
  *  Secure SHell is a cryptographic network protocol used for operating network services
  *  SSH encrypts data transmitted between two devices
  *  SSH is most notably used for remote login and command-line execution  
      *  SSH was created to replace Telnet, through which data is transmitted in-the-clear, not encrypted

OSPF  
  * Open Shortest Path First is an algorithmic link-state routing protocol intent on routing data within an Autonomous System efficiently
    * AS refers to a large network, or group of networks, that use a single routing policy
  * OSPF routers share their link-state information among others to accurately map out the network topology

<p align=center>
  <img src="https://github.com/Fehral/networkprojectv3/blob/main/networkprojectv3topology.png?raw=true">
</p>

F1-Router Configuration
```
Router>en
Router#conf t
Router(config)#hostname F1-Router
F1-Router(config)#ip domain name Floor1
F1-Router(config)#username cisco password cisco
F1-Router(config)#crypto key generate rsa
How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
F1-Router(config)#line vty 0 15
*Mar 1 0:0:42.319: %SSH-5-ENABLED: SSH 1.99 has been enabled
F1-Router(config-line)#login local
F1-Router(config-line)#exit
F1-Router(config)#int s0/0/0
F1-Router(config-if)#ip address 10.10.10.1 255.255.255.252
F1-Router(config-if)#no shutdown
F1-Router(config-if)#int s0/0/1
F1-Router(config-if)#no shutdown
F1-Router(config-if)#exit
F1-Router(config)#router ospf 10
F1-Router(config-router)#network 10.10.10.0 0.0.0.3 area 0
F1-Router(config-router)#network 10.10.10.8 0.0.0.3 area 0
F1-Router(config-router)#network 192.168.6.0 255.255.255.0 area 0
F1-Router(config-router)#network 192.168.7.0 255.255.255.0 area 0
F1-Router(config-router)#network 192.168.8.0 255.255.255.0 area 0
F1-Router(config-router)#exit
F1-Router(config)#service dhcp
F1-Router(config)#ip dhcp pool Logistics
F1-Router(dhcp-config)#network 192.168.6.0 255.255.255.0
F1-Router(dhcp-config)#default-router 192.168.6.1
F1-Router(dhcp-config)#dns-server 192.168.6.1
F1-Router(dhcp-config)#domain-name Logistics.com
F1-Router(dhcp-config)#ip dhcp pool Store
F1-Router(dhcp-config)#network 192.168.7.0 255.255.255.0
F1-Router(dhcp-config)#default-router 192.168.7.1
F1-Router(dhcp-config)#dns-server 192.168.7.1
F1-Router(dhcp-config)#domain-name Store.com
F1-Router(dhcp-config)#ip dhcp pool Reception
F1-Router(dhcp-config)#network 192.168.8.0 255.255.255.0
F1-Router(dhcp-config)#default-router 192.168.8.1
F1-Router(dhcp-config)#dns-server 192.168.8.1
F1-Router(dhcp-config)#domain-name Reception.com
F1-Router(config)#int g0/0.60
F1-Router(config-subif)#encapsulation dot1Q 60
F1-Router(config-subif)#ip address 192.168.6.1 255.255.255.0
F1-Router(config-subif)#int g0/0.70
F1-Router(config-subif)#encapsulation dot1Q 70
F1-Router(config-subif)#ip address 192.168.7.1 255.255.255.0
F1-Router(config-subif)#int g0/0.80
F1-Router(config-subif)#encapsulation dot1Q 80
F1-Router(config-subif)#ip address 192.168.8.1 255.255.255.0
F1-Router(config-subif)#int g0/0
F1-Rotuer(config-if)#no shutdown
F1-Router(config-if)#exit
F1-Router(config)#exit
F1-Router#copy run start
Destination filename [startup-config]? [Enter]
Building configuration...
[OK]
```

F1-Switch Configuration
```
Switch>en
Switch#conf t
Switch(config)#int range fa0/1-3
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 60
Switch(config-if-range)#int range fa 0/4-5
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 70
Switch(config-if-range)#int range fa0/6-7
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 80
Switch(config-if-range)#int gig0/1
Switch(config-if)#switchport mode trunk
Switch(config-if)#exit
Switch(config)#exit
Switch#copy run start
Destination filename [startup-config]? [Enter]
Building configuration...
[OK]
```

F2-Router Configuration
```
Router>en
Router#conf t
Router(config)#hostname F2-Router
F1-Router(config)#ip domain name Floor2
F1-Router(config)#username cisco password cisco
F1-Router(config)#crypto key generate rsa
How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
F1-Router(config)#line vty 0 15
*Mar 1 0:0:42.319: %SSH-5-ENABLED: SSH 1.99 has been enabled
F1-Router(config-line)#login local
F1-Router(config-line)#exit
F1-Router(config)#int s0/0/1
F1-Router(config-if)#clock rate 64000
F1-Router(config-if)#ip address 10.10.10.2 255.255.255.252
F1-Router(config-if)#no shutdown
F1-Router(config-if)#int s0/0/0
F1-Router(config-if)#clock rate 64000
F1-Router(config-if)#no shutdown
F1-Router(config-if)#exit
F1-Router(config)#router ospf 10
F1-Router(config-router)#network 10.10.10.4 0.0.0.3 area 0
F1-Router(config-router)#network 10.10.10.8 0.0.0.3 area 0
F1-Router(config-router)#network 192.168.1.0 255.255.255.0 area 0
F1-Router(config-router)#network 192.168.2.0 255.255.255.0 area 0
F1-Router(config-router)#exit
F1-Router(config)#service dhcp
F1-Router(config)#ip dhcp pool Sales
F1-Router(dhcp-config)#network 192.168.3.0 255.255.255.0
F1-Router(dhcp-config)#default-router 192.168.3.1
F1-Router(dhcp-config)#dns-server 192.168.3.1
F1-Router(dhcp-config)#domain-name Sales.com
F1-Router(dhcp-config)#ip dhcp pool HR
F1-Router(dhcp-config)#network 192.168.4.0 255.255.255.0
F1-Router(dhcp-config)#default-router 192.168.4.1
F1-Router(dhcp-config)#dns-server 192.168.4.1
F1-Router(dhcp-config)#domain-name HR.com
F1-Router(dhcp-config)#ip dhcp pool Finance
F1-Router(dhcp-config)#network 192.168.5.0 255.255.255.0
F1-Router(dhcp-config)#default-router 192.168.5.1
F1-Router(dhcp-config)#dns-server 192.168.5.1
F1-Router(dhcp-config)#domain-name Finance.com
F1-Router(config)#int g0/0.60
F1-Router(config-subif)#encapsulation dot1Q 60
F1-Router(config-subif)#ip address 192.168.6.1 255.255.255.0
F1-Router(config-subif)#int g0/0.70
F1-Router(config-subif)#encapsulation dot1Q 70
F1-Router(config-subif)#ip address 192.168.7.1 255.255.255.0
F1-Router(config-subif)#int g0/0.80
F1-Router(config-subif)#encapsulation dot1Q 80
F1-Router(config-subif)#ip address 192.168.8.1 255.255.255.0
F1-Router(config-subif)#int g0/0
F1-Rotuer(config-if)#no shutdown
F1-Router(config-if)#exit
F1-Router(config)#exit
F1-Router#copy run start
Destination filename [startup-config]? [Enter]
Building configuration...
[OK]
```

F2-Switch Configuration
```
Switch>en
Switch#conf t
Switch(config)#int range fa0/1-3
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 30
Switch(config-if-range)#int range fa 0/4-5
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 40
Switch(config-if-range)#int range fa0/6-7
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 50
Switch(config-if-range)#int gig0/1
Switch(config-if)#switchport mode trunk
Switch(config-if)#exit
Switch(config)#exit
Switch#copy run start
Destination filename [startup-config]? [Enter]
Building configuration...
[OK]
```

F3-Router Configuration
```
Router>en
Router#conf t
Router(config)#hostname F2-Router
F1-Router(config)#ip domain name Floor3
F1-Router(config)#username cisco password cisco
F1-Router(config)#crypto key generate rsa
How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
F1-Router(config)#line vty 0 15
*Mar 1 0:0:42.319: %SSH-5-ENABLED: SSH 1.99 has been enabled
F1-Router(config-line)#login local
F1-Router(config-line)#exit
F1-Router(config)#int s0/0/1
F1-Router(config-if)#clock rate 64000
F1-Router(config-if)#ip address 10.10.10.10 255.255.255.252
F1-Router(config-if)#no shutdown
F1-Router(config-if)#int s0/0/0
F1-Router(config-if)#clock rate 64000
F1-Router(config-if)#ip address 10.10.10.6 255.255.255.252
F1-Router(config-if)#no shutdown
F1-Router(config-if)#exit
F1-Router(config)#router ospf 10
F1-Router(config-router)#network 10.10.10.0 0.0.0.3 area 0
F1-Router(config-router)#network 10.10.10.4 0.0.0.3 area 0
F1-Router(config-router)#network 192.168.3.0 255.255.255.0 area 0
F1-Router(config-router)#network 192.168.4.0 255.255.255.0 area 0
F1-Router(config-router)#network 192.168.5.0 255.255.255.0 area 0
F1-Router(config-router)#exit
F1-Router(config)#service dhcp
F1-Router(config)#ip dhcp pool IT
F1-Router(dhcp-config)#network 192.168.1.0 255.255.255.0
F1-Router(dhcp-config)#default-router 192.168.1.1
F1-Router(dhcp-config)#dns-server 192.168.1.1
F1-Router(dhcp-config)#domain-name IT.com
F1-Router(dhcp-config)#ip dhcp pool Admin
F1-Router(dhcp-config)#network 192.168.2.0 255.255.255.0
F1-Router(dhcp-config)#default-router 192.168.2.1
F1-Router(dhcp-config)#dns-server 192.168.2.1
F1-Router(dhcp-config)#domain-name Admin.com
F1-Router(config)#int g0/0.10
F1-Router(config-subif)#encapsulation dot1Q 10
F1-Router(config-subif)#ip address 192.168.1.1 255.255.255.0
F1-Router(config-subif)#int g0/0.20
F1-Router(config-subif)#encapsulation dot1Q 20
F1-Router(config-subif)#ip address 192.168.2.1 255.255.255.0
F1-Router(config-subif)#int g0/0
F1-Rotuer(config-if)#no shutdown
F1-Router(config-if)#exit
F1-Router(config)#exit
F1-Router#copy run start
Destination filename [startup-config]? [Enter]
Building configuration...
[OK]
```

F3-Switch Configuration
```
Switch>en
Switch#conf t
Switch(config)#int range fa0/1-3
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 10
Switch(config-if-range)#int range fa 0/4-5
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 20
Switch(config-if-range)#int gig0/1
Switch(config-if)#switchport mode trunk
Switch(config-if)#exit
Switch(config)#exit
Switch#copy run start
Destination filename [startup-config]? [Enter]
Building configuration...
[OK]
```
