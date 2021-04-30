# COE 444 FINAL EXAM SUMMARY

## Subnetting
A **subnetwork** or **subnet** is a logical subdivision of an IP network. The practice of dividing a network into two or more networks is called **subnetting**.

![](https://i.ibb.co/vd2y7Fw/subnetting1.png)
![](https://i.ibb.co/MhYzg6x/subnetting2.png)

## Spanning Tree Protocol (STP)
The **Spanning Tree Protocol** (**STP**) is a network protocol that builds a loop-free logical topology for Ethernet networks. The basic function of STP is to prevent bridge loops and the broadcast radiation that results from them. Spanning tree also allows a network design to include backup links providing fault tolerance if an active link fails.

### Choosing Root Bridge
1. The switch with the lowest priority ( **Default is 32768**)
2. The switch with the lowest MAC address

### Ports Status
1. Designated
   * All the ports of the Root Bridge are **Designated**
   * Designated ports are in the **Forwarding Mode**
2. Root
   * A port that lead to the Root Bridge(**NOT Necessarily directly**)
   * Root ports are in the **Forwarding Mode**
 3. Non Designated
    * Non Designated Ports are between two non Root Bridge switches
    *  Non Designated Ports are in the **Blocking Mode**

 ### Ports Mode ( By Order)
 1. Blocking ( For 20 Seconds)
 2. Listening
    * For 15 seconds and will share Bridge Protocol Data Unit (**BDUP**)
 3. Learning
    * For 15 seconds and will share  **BDUP** and **MAC Address**
 4. Forwarding (Portfast)

### Cost of diffrent links' speed
- 10 Gigabyte = 2
- 1 Gigabyte = 4
-  100 Megabyte = 19
-  10 Megabyte = 100
- 
### Cisco Packet Tracer commands for STP
- spanning-tree vlan <#> priority <#>
- spanning-tree vlan <#> root primary
- spanning-tree vlan <#> root secondary
- do show spanning-tree

## PVST and Dot1Q
**VLANs (Virtual LANs)** are logical grouping of devices in the same broadcast domain. VLANs are usually configured on switches by placing some interfaces into one broadcast domain and some interfaces into another. Each VLAN acts as a subgroup of the switch ports in an Ethernet LAN.

**Per VLAN Spanning Tree (PVST)** is a Cisco proprietary protocol that allows a Cisco device to have multiple spanning trees.

**Note:** VLANs cannot communicate directly but they can communicate  through a Router or a Layer 3 Switch.

**Dot1Q** allows you to use a router interface as a trunk port to a switch. This is also known as “Router on a stick” because the switch uses the router to route between VLANs.

### Important Notes
- The port between the router and the swtiches need to be in trunk mode
- The ports betwen the switches need to in trunk mode
- The ports between the switches and end devices need to be in access mode
- It is preferable if you set the ports connected to the end devices to portfast and bdup gaurd enable
- 
### Cisco Packet Tracer commands for PVST and Dot1Q
- spanning-tree mode pvst
- switchport trunk mode
- switchport access mode
- switch port access vlan <#>
- spanning-tree portfase
- spanning-tree bdup garud enable
- interface f0/0.<#>
- encapsulation dot1q <#>
- ip address [Router interface] [Subnet]

## Multilayer Switch
A **Multilayer Switch** can perform the functions of a switch as well as that of a router at incredibly fast speeds.

### Commands for Multilayer Switch
- IP routing

## VTP
**VLAN Trunking Protocol (VTP)** is a Cisco proprietary protocol that propagates the definition of Virtual Local Area Networks (VLAN) on the whole local area network.

### VTP mode
1. Server (**Default**)
   * Can modify and change VTP, affected by changes in the VTP, and pass the VTP to other switches
2. Client
   * Affected by changes in the VTP, and pass the VTP to other switches
3. Transperant
   * **Not** affected by changes in the VTP, and pass the VTP to other switches
 ### Important Notes
 - There can be more than one server in the VTP
 - The ports between swtiches need to be in trunk mode
 - The higher VTP revision number is the fresher (**Last update**)
### Commands for VTP
- vtp domain [domain name]
- vtp pass [password]
- vtp mode [client,server,transparent]
- do show vtp status
- do show vlan

## Summarization and Static Routing
**Summary static routes** can be used to help minimize the number of static routes in the routing table.
![](https://www.ciscopress.com/content/images/chap2_9781587133237/elementLinks/02fig56.jpg)

### Commands for Summarization and Static Routing
- ip route [summarized network IP] [summarized network subnet mask] [Port]
- ip route 0.0.0.0 0.0.0.0 [Port] (**For default route**)

## RIP Protocol
**Routing Information Protocol** (RIP) is a dynamic routing protocol which uses hop count as a routing metric to find the best path between the source and the destination network.

**Hop Count** is the number of routers occurring in between the source and destination network.

### Features of RIP 
1. Updates of the network are exchanged periodically.  
2. Updates (routing information) are always broadcast.  
3. Full routing tables are sent in updates.  
4. Routers always trust on routing information received from neighbor routers. This is also known as  _Routing on rumours_.

### RIP versions
- RIP Version1
    * known as _Classful_ Routing Protocol because it doesn’t send information of subnet mask in its routing update.
- RIP Version2
    * known as _Classless_ Routing Protocol because it sends information of subnet mask in its routing update.

### Important Notes
- RIP uses 120 as an administrative distance for both exteranl and on the same  protocol routers (**It can be found in the routing table next to the route [administrative/number]**)

### Commands for RIP protocol
- router rip
- version 2
- no auto-summary
- network [Network ip]
- redistribute ospf [<1-65535 ID] metric [0-16]
- redistribute eigrp [<1-65535 ID] metric [0-16]
- redistribute static
## OSPF Protocol
**Open Shortest Path First (OSPF)** is a link-state routing protocol that is used to find the best path between the source and the destination router using its own Shortest Path First).


### OSPF terms 
1.  **Router I’d –**  It is the highest active IP address present on the router. First, highest loopback address is considered. If no loopback is configured then the highest active IP address on the interface of the router is considered.
2.  **Router priority –**  It is a 8 bit value assigned to a router operating OSPF, used to elect DR and BDR in a broadcast network.
3.  **Designated Router (DR) –**  It is elected to minimize the number of adjacency formed.  
4.  **Backup Designated Router (BDR) –**  BDR is backup to DR in a broadcast network. When DR goes down, BDR becomes DR and performs its functions.

### DR and BDR election
 DR and BDR election takes place in broadcast network or multi-access network. Here are the criteria for the election:

1.  Router having the highest router priority will be declared as DR.
2.  If there is a tie in router priority then highest router I’d will be considered. First, the highest loopback address is considered. If no loopback is configured then the highest active IP address on the interface of the router is considered.

### Stub Areas
- Stub area
   * No LSA 5 (**Subnet that is injected into OSPF from an external source**)
 - Totally stub area
   * No LSA 5
   * No LSA 3 (**Network summary and Inter Area**)
 - NSSA (not so stubby area)
    * No LSA 5
    * Uses LSA 7 for external routers
 - Totally NSSA (totally not so stubby area)
    * No LSA 3 
    * No LSA 5
    * Uses LSA 7 for external routers

### Important Notes
- OSPF uses 110 as an administrative distance all routers in the protocol whether its external or Inter Area (**It can be found in the routing table next to the route [administrative/number]**)
- 
### Commands for OSPF
- router ospf [1-65535 Process ID]
- router-id [ID in the form N.N.N.N]
- network [Network ip] [wild card] area [OSPF area ID]
- #redistribute rip subnets
- redistribute eigrp [1-65535] subnets
- default-information originate
- area [OSPF area ID] range [summarized network IP] [summarized network subnet mask]

## EIGRP Protocol
**Enhanced Interior Gateway Routing Protocol (EIGRP)** is a dynamic routing Protocol which is used to find the best path between any two layer 3 device to deliver the packet.

### Feasible and reported distance
-   **Feasible distance (FD)**  – the metric of the best route to reach a network. That route will be listed in the routing table.
-    **Reported distance (RD)**  – the metric advertised by a neighboring router for a specific route. It other words, it is the metric of the route used by the neighboring router to reach the network.
![](https://i.ibb.co/ys6CNd7/eigrp.png)

### Successor and feasible successor
- **Successor** is the route with the best metric to reach a destination.
- **Feasible successor** is a backup path to reach that same destination that can be used immediately if the successor route fails.

### Setting a router as a successor
1.  You can change the bandwith in the interface leading to the router (**i.e bandwith 32**)
2.  You can change the dealy in the interface leading to the router(**i.e delay 100000**)
### Important Notes
- Eigrp uses 90 as an administrative distance for Eigrp routes and more than 90 for external routes the default is 170 (**It can be found in the routing table next to the route [administrative/number]**)

### Commands for EIGRP
- eigrp [1-65535 AS num]
- no auto-summary
- network [ip] [wild card]
- redistribute rip metric [10000][10][255][1][1500]
- redistribute ospf [1-65535 Process ID] metric [10000][10][255][1][1500]
- redistribute static

## BGP
**Border Gateway Protocol (BGP)** is used to Exchange routing information for the internet and is the protocol used between ISP which are different ASes.

### Characteristics of Border Gateway Protocol (BGP)
-   **Inter-Autonomous System Configuration:**  The main role of BGP is to provide communication between two autonomous systems.
-   BGP supports Next-Hop Paradigm.
-   Coordination among multiple BGP speakers within the AS (Autonomous System).
-   **Path Information:**  BGP advertisement also include path information, along with the reachable destination and next destination pair.
-   **Policy Support:**  BGP can implement policies that can be configured by the administrator. For ex:- a router running BGP can be configured to distinguish between the routes that are known within the AS and that which are known from outside the AS.

### Commands for BGP
- router bgp [AS num]
- bgp router-id [N.N.N.N]
- neighbor [router interface ip] remote-as [AS num OF neighbor]
- network [connected Netwrk ip] mask [Network mask]

## iBGP-eBGP
If a BGP session is established between two neighbors in different autonomous systems, the session is _external BGP (EBGP)_, and if the session is established between two neighbors in the same AS, the session is _internal BGP_
![](https://www.networkcomputing.com/sites/default/files/1-6.jpg)
**IBGP** can use EIGRP or OSFP. RIP is not suitable.

## Access Control List (ACL)
**Access-list (ACL)** is a set of rules defined for controlling the network traffic and reducing network attack. ACLs are used to filter traffic based on the set of rules defined for the incoming or out going of the network.

### Types of ACL  
1.  **Standard Access-list –**  These are the Access-list which are made using the source IP address only. These ACLs permit or deny the entire protocol suite. They don’t distinguish between the IP traffic such as TCP, UDP, Https etc. By using numbers 1-99 or 1300-1999, router will understand it as a standard ACL and the specified address as source IP address.
2.  **Extended Access-list –**  These are the ACL which uses both source and destination IP address. In these type of ACL, we can also mention which IP traffic should be allowed or denied. These use range 100-199 and 2000-2699.

### Rules for ACL
1.  The standard Access-list is generally applied close to the destination (but not always).
2.  The extended Access-list is generally applied close to the source (but not always).
3.  We can assign only one ACL per interface per protocol per direction, i.e., only one inbound and outbound ACL is permitted per interface.
4.  We can’t remove a rule from an Access-list if we are using numbered Access-list. If we try to remove a rule then whole ACL will be removed. If we are using named access lists then we can delete a specific rule.
5.  Every new rule which is added into the access list will be placed at the bottom of the access list therefore before implementing the access lists, analyses the whole scenario carefully.
6.  As there is an implicit deny at the end of every access list, we should have at least a permit statement in our Access-list otherwise all traffic will be denied.
7.  Standard access lists and extended access lists cannot have the same name.

### Important Notes
-   **Inbound access lists –**  When an access list is applied on inbound packets of the interface then first the packets will processed according to the access list and then routed to the outbound interface.
-   **Outbound access lists –**  When an access list is applied on outbound packets of the interface then first the packet will be routed and then processed at the outbound interface.

### Commands for ACL
![](https://i.ibb.co/7VsHYYg/ACL1.png)
![](https://i.ibb.co/RT5Rk84/ACL2.png)
![](https://i.ibb.co/4K1VJxW/ACL3.png)
![](https://i.ibb.co/G9DPdwh/ACL4.png)
![](https://i.ibb.co/Bc3Frwx/ACL5.png)
## IPV6
IP v6 was developed by Internet Engineering Task Force (IETF) to deal with the problem of IP v4 exhaustion. IP v6 is 128-bits address having an address space of 2^128, which is way bigger than IPv4. In IPv6 we use Colon-Hexa representation. There are 8 groups and each group represents 2 Bytes.
![](https://media.geeksforgeeks.org/wp-content/uploads/ipv6-1-2-1024x284.png)
### Addressing methods 
- Unicast
    * Unicast Address identifies a single network interface. A packet sent to unicast address is delivered to the interface identified by that address.
- Multicast
   * Multicast Address is used by multiple hosts, called as Group, acquires a multicast destination address.
 - Anycast
    * Anycast Address is assigned to a group of interfaces. Any packet sent to anycast address will be delivered to only one member interface (mostly nearest host possible).

### Commands for IPV6 
- ipv6 unicast-routing
- ipv6 address FE80::# link-local
- ipv6 address [ipv6]

## RIPng
### Important Notes
- Unlike RIP, RIPng needs to be declared at every interface of the router
- Defualt router information need to be shared between routers and the command is written at every router

### Commands for RIPng
- ipv6 rip RIP1 enable
- ipv6 rip RIP1 default-information originate

## OSPF V3
### Important Notes
- Unlike OSPF, OSPFV3 commands is written in the interfaces conneting the routers

## EIGRPV6
### Important Notes
- Unlike EIGRP, EIGRPV6 commands is written in the interfaces conneting the routers

## NAT 
**Network Address Translation (NAT)** is a process in which one or more local IP address is translated into one or more Global IP address and vice versa in order to provide Internet access to the local hosts.
### NAT inside and outside addresses
Inside refers to the addresses which must be translated. Outside refers to the addresses which are not in control of an organisation. These are the network Addresses in which the translation of the addresses will be done.
![](https://media.geeksforgeeks.org/wp-content/uploads/stati1c.png)
-   **Inside local address –**  An IP address that is assigned to a host on the Inside (local) network. The address is probably not a IP address assigned by the service provider i.e., these are private IP address. This is the inside host seen from the inside network.
-   **Inside global address –**  IP address that represents one or more inside local IP addresses to the outside world. This is the inside host as seen from the outside network.
-   **Outside local address –**  This is the actual IP address of the destination host in the local network after translation.
-   **Outside global address –**  This is the outside host as seen form the outside network. It is the IP address of the outside destination host before translation.
### Commands for NAT
![](https://i.ibb.co/g6BgZd4/NAT1.png)
![](https://i.ibb.co/R0bGJx9/NAT2.png)
![](https://i.ibb.co/hXYwXfc/NAT3.png)
![](https://i.ibb.co/4ZvzrJF/NAT4.png)
