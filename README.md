# Comprehensive-CCNA-Networking-Project

**Project Description:**

This project involves designing and implementing a robust network infrastructure using CCNA concepts. Key elements include IP addressing, VLANs, trunking, VTP, SVIs, DHCP, HSRP, OSPF, NAT, and internet connectivity. Wireless access points and EtherChannel are configured for enhanced connectivity and performance. Failover testing is performed to ensure high availability and network resilience under various scenarios. This project demonstrates practical skills in network design, configuration, and troubleshooting.


Here's the configuration for CORE-SWITCH1:

```sh
# CORE-SWITCH1 Configuration
===========================================
# Creating trunk

show cdp neighbor

interface range fa0/2-7
 switchport trunk encapsulation dot1Q
 switchport mode trunk

show interface status  # to check
===========================================
# Creating VTP

vtp domain sohan

show vtp status
===========================================
# Creating VLANs

vlan 10   
 name voice   

vlan 30
 name 1stfloor

vlan 32
 name 2ndfloor

vlan 40
 name wifi

vlan 444
 name server
===========================================
# SVI creation

interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 description voice-vlan
 no shutdown

interface vlan 30
 ip address 192.168.30.1 255.255.255.0
 description firstfloor
 no shutdown

interface vlan 32
 ip address 192.168.32.1 255.255.255.0
 description 2ndfloor
 no shutdown

interface vlan 40
 ip address 192.168.40.1 255.255.255.0
 description wifi
 no shutdown

interface vlan 444
 ip address 192.168.50.1 255.255.255.0
 description server
 no shutdown
===========================================
# HSRP (priority and preempt)

interface vlan 10
 standby 10 ip 192.168.10.3
 standby 10 priority 105
 standby 10 preempt

interface vlan 30
 standby 30 ip 192.168.30.3
 standby 30 priority 105
 standby 30 preempt

interface vlan 32
 standby 32 ip 192.168.32.3

interface vlan 40
 standby 40 ip 192.168.40.3

interface vlan 444
 standby version 2
 standby 444 ip 192.168.50.3

show standby brief
===========================================
# DHCP for all VLANs

ip dhcp pool voice
 default-router 192.168.10.3
 network 192.168.10.0 255.255.255.0

ip dhcp pool 1stfloor
 default-router 192.168.30.3
 network 192.168.30.0 255.255.255.0

ip dhcp pool 2ndfloor
 default-router 192.168.32.3
 network 192.168.32.0 255.255.255.0

ip dhcp pool wifi
 default-router 192.168.40.3
 network 192.168.40.0 255.255.255.0

ip dhcp pool server
 default-router 192.168.50.3
 network 192.168.50.0 255.255.255.0

show ip dhcp pool

show running-config | section dhcp
===========================================
# Enable IP routing

ip routing
===========================================
# Connectivity between core switch1 and gw1 (router)

interface fa0/1
 no switchport 
 ip address 192.168.99.2 255.255.255.0
 description coresw1-gw1
 no shutdown

router ospf 1
 interface vlan 10
  ip ospf 1 area 0
 interface vlan 30
  ip ospf 1 area 0
 interface vlan 32
  ip ospf 1 area 0
 interface vlan 40
  ip ospf 1 area 0
 interface vlan 444
  ip ospf 1 area 0
 interface fa0/1
  ip ospf 1 area 0
```



Here's the configuration for CORE-SWITCH2:

```sh
# CORE-SWITCH2 Configuration
===========================================
# Creating trunk

show cdp neighbor

interface range fa0/2-7
 switchport trunk encapsulation dot1Q
 switchport mode trunk

===========================================
# Creating VTP

vtp domain sohan

show vtp status
===========================================
# SVI creation

interface vlan 10
 ip address 192.168.10.2 255.255.255.0
 description voice-vlan
 no shutdown

interface vlan 30
 ip address 192.168.30.2 255.255.255.0
 description firstfloor
 no shutdown

interface vlan 32
 ip address 192.168.32.2 255.255.255.0
 description 2ndfloor
 no shutdown

interface vlan 40
 ip address 192.168.40.2 255.255.255.0
 description wifi
 no shutdown

interface vlan 444
 ip address 192.168.50.2 255.255.255.0
 description server
 no shutdown

===========================================
# HSRP (priority and preempt)

interface vlan 10
 standby 10 ip 192.168.10.3

interface vlan 30
 standby 30 ip 192.168.30.3

interface vlan 32
 standby 32 ip 192.168.32.3
 standby 32 priority 105
 standby 32 preempt

interface vlan 40
 standby 40 ip 192.168.40.3
 standby 40 priority 105
 standby 40 preempt

interface vlan 444
 standby version 2
 standby 444 ip 192.168.50.3

show standby brief
===========================================
# DHCP for all VLANs

ip dhcp pool voice
 default-router 192.168.10.3
 network 192.168.10.0 255.255.255.0

ip dhcp pool 1stfloor
 default-router 192.168.30.3
 network 192.168.30.0 255.255.255.0

ip dhcp pool 2ndfloor
 default-router 192.168.32.3
 network 192.168.32.0 255.255.255.0

ip dhcp pool wifi
 default-router 192.168.40.3
 network 192.168.40.0 255.255.255.0

ip dhcp pool server
 default-router 192.168.50.3
 network 192.168.50.0 255.255.255.0

show ip dhcp pool

show running-config | section dhcp
===========================================
# Enable IP routing

ip routing
===========================================
# Connectivity between core switch2 and gw2 (router)

interface fa0/1
 no switchport 
 ip address 192.168.100.2 255.255.255.0
 description coresw2-gw2
 no shutdown

router ospf 1
 interface vlan 10
  ip ospf 1 area 0
 interface vlan 30
  ip ospf 1 area 0
 interface vlan 32
  ip ospf 1 area 0
 interface vlan 40
  ip ospf 1 area 0
 interface vlan 444
  ip ospf 1 area 0
 interface fa0/1
  ip ospf 1 area 0
```

Here's the configuration for ASW1, ASW2, ASW3, and ASW4:

```sh
# ASW1 Configuration
===========================================
interface range fa2/1, fa3/1
 switchport access vlan 30
 switchport voice vlan 10

===========================================
# ASW2 Configuration
===========================================
interface range fa2/1, fa3/1
 switchport access vlan 32

===========================================
# ASW3 Configuration
===========================================
interface fa2/1
 switchport access vlan 444

===========================================
# ASW4 Configuration
===========================================
interface range fa0/1, fa0/2
 switchport access vlan 444

interface range fa0/3-4
 switchport access vlan 40
```

Here is the configuration for GW1, AWS, GW2, and Backup AWS routers:

```sh
# GW1 (Router 1) Configuration
===========================================
interface fa0/0
 description gw1-coresw1
 ip address 192.168.99.1 255.255.255.0
 no shutdown

interface fa4/0
 description gw1-aws
 ip address 128.1.1.2 255.255.255.252
 no shutdown

===========================================
# Default Route
===========================================
ip route 0.0.0.0 0.0.0.0 128.1.1.1

===========================================
# OSPF Configuration
===========================================
router ospf 1
 default-information originate
 network 192.168.99.0 0.0.0.255 area 0

interface fa0/0
 ip ospf 1 area 0

===========================================
# NAT Configuration
===========================================
access-list 1 permit 192.168.30.0 0.0.0.255
access-list 1 permit 192.168.32.0 0.0.0.255
access-list 1 permit 192.168.40.0 0.0.0.255

ip nat inside source list 1 interface fa4/0 overload

interface fa4/0
 ip nat outside

interface fa0/0
 ip nat inside

===========================================
# AWS (Router) Configuration
===========================================
interface fa4/0
 ip address 128.1.1.1 255.255.255.252
 description aws-gw1
 no shutdown

interface lo0
 ip address 99.1.1.1 255.255.255.255

interface lo1
 ip address 100.1.1.1 255.255.255.255

===========================================
# GW2 (Router 2) Configuration
===========================================
interface fa0/0
 description gw2-coresw2
 ip address 192.168.100.1 255.255.255.0
 no shutdown

interface fa4/0
 description gw2-backupinternet
 ip address 129.1.1.2 255.255.255.252
 no shutdown

===========================================
# Default Route
===========================================
ip route 0.0.0.0 0.0.0.0 129.1.1.1

===========================================
# OSPF Configuration
===========================================
router ospf 1
 default-information originate
 network 192.168.100.0 0.0.0.255 area 0

interface fa0/0
 ip ospf 1 area 0

===========================================
# NAT Configuration
===========================================
access-list 1 permit 192.168.30.0 0.0.0.255
access-list 1 permit 192.168.32.0 0.0.0.255
access-list 1 permit 192.168.40.0 0.0.0.255

ip nat inside source list 1 interface fa4/0 overload

interface fa4/0
 ip nat outside

interface fa0/0
 ip nat inside

===========================================
# Backup AWS (Router) Configuration
===========================================
interface fa4/0
 ip address 129.1.1.1 255.255.255.252
 description aws-gw1
 no shutdown

interface lo0
 ip address 100.1.1.1 255.255.255.255

interface lo1
 ip address 99.1.1.1 255.255.255.255
```







