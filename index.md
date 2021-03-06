# COE 444 FINAL EXAM SUMMARY

### In addition to this document, you must know the interpretations of show commands. (**Very Important**)

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

### The calculation of Metric or Cost:
Metric(Cost) = Reference-bandwidth/interface-bandwidth (**It cannot be less than 1**)

### Cisco Packet Tracer commands for STP
- spanning-tree vlan <#> priority <#>
- spanning-tree vlan <#> root primary
- spanning-tree vlan <#> root secondary
- do show spanning-tree

## PVST and Dot1Q
**VLANs (Virtual LANs)** are logical grouping of devices in the same broadcast domain. VLANs are usually configured on switches by placing some interfaces into one broadcast domain and some interfaces into another. Each VLAN acts as a subgroup of the switch ports in an Ethernet LAN.

**Per VLAN Spanning Tree (PVST)** is a Cisco proprietary protocol that allows a Cisco device to have multiple spanning trees.

**Note:** VLANs cannot communicate directly but they can communicate  through a Router or a Layer 3 Switch.

**Dot1Q** allows you to use a router interface as a trunk port to a switch. This is also known as ???Router on a stick??? because the switch uses the router to route between VLANs.

### Important Notes
- The port between the router and the swtiches need to be in trunk mode
- The ports betwen the switches need to in trunk mode
- The ports between the switches and end devices need to be in access mode
- It is preferable if you set the ports connected to the end devices to portfast and bdup gaurd enable

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
**Routing Information Protocol** (RIP) is a dynamic routing protocol (*Distance Vector*) which uses hop count as a routing metric to find the best path between the source and the destination network.

**Hop Count** is the number of routers occurring in between the source and destination network.

### Features of RIP 
1. Updates of the network are exchanged periodically.  
2. Updates (routing information) are always broadcast.  
3. Full routing tables are sent in updates.  
4. Routers always trust on routing information received from neighbor routers. This is also known as  _Routing on rumours_.

### RIP versions
- RIP Version1
    * known as _Classful_ Routing Protocol because it doesn???t send information of subnet mask in its routing update.
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
1.  **Router I???d ???**  It is the highest active IP address present on the router. First, highest loopback address is considered. If no loopback is configured then the highest active IP address on the interface of the router is considered.
2.  **Router priority ???**  It is a 8 bit value assigned to a router operating OSPF, used to elect DR and BDR in a broadcast network.
3.  **Designated Router (DR) ???**  It is elected to minimize the number of adjacency formed.  
4.  **Backup Designated Router (BDR) ???**  BDR is backup to DR in a broadcast network. When DR goes down, BDR becomes DR and performs its functions.

### DR and BDR election
 DR and BDR election takes place in broadcast network or multi-access network. Here are the criteria for the election:

1.  Router having the highest router priority will be declared as DR.
2.  If there is a tie in router priority then highest router I???d will be considered. First, the highest loopback address is considered. If no loopback is configured then the highest active IP address on the interface of the router is considered.

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

### Commands for OSPF
- router ospf [1-65535 Process ID]
- router-id [ID in the form N.N.N.N]
- network [Network ip] [wild card] area [OSPF area ID]
- #redistribute rip subnets
- redistribute eigrp [1-65535] subnets
- default-information originate
- area [OSPF area ID] range [summarized network IP] [summarized network subnet mask]

## EIGRP Protocol
**Enhanced Interior Gateway Routing Protocol (EIGRP)** is a dynamic routing Protocol (*link-state*) which is used to find the best path between any two layer 3 device to deliver the packet.

### Feasible and reported distance
-   **Feasible distance (FD)**  ??? the metric of the best route to reach a network. That route will be listed in the routing table.
-    **Reported distance (RD)**  ??? the metric advertised by a neighboring router for a specific route. It other words, it is the metric of the route used by the neighboring router to reach the network.
![](https://i.ibb.co/ys6CNd7/eigrp.png)

### Successor and feasible successor
- **Successor** is the route with the best metric to reach a destination.
- **Feasible successor** is a backup path to reach that same destination that can be used immediately if the successor route fails.

### Setting a router as a successor
1.  You can change the bandwith in the interface leading to the router (**i.e bandwith 32**)
2.  You can change the dealy in the interface leading to the router(**i.e delay 100000**)

### Rule of choosing Feasible Successor (FS):
Administrative Distance of Feasible Successor should be lower than Feasible Distance of the successor

### Important Notes
- Eigrp uses 90 as an administrative distance for Eigrp routes and more than 90 for external routes the default is 170 (**It can be found in the routing table next to the route [administrative/number]**)

### Commands for EIGRP
- router eigrp [1-65535 AS num]
- no auto-summary
- network [ip] [wild card]
- redistribute rip metric [10000][100][255][1][1500]
- redistribute ospf [1-65535 Process ID] metric [10000][100][255][1][1500]
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
1.  **Standard Access-list ???**  These are the Access-list which are made using the source IP address only. These ACLs permit or deny the entire protocol suite. They don???t distinguish between the IP traffic such as TCP, UDP, Https etc. By using numbers 1-99 or 1300-1999, router will understand it as a standard ACL and the specified address as source IP address.
2.  **Extended Access-list ???**  These are the ACL which uses both source and destination IP address. In these type of ACL, we can also mention which IP traffic should be allowed or denied. These use range 100-199 and 2000-2699.

### Rules for ACL
1.  The standard Access-list is generally applied close to the destination (but not always).
2.  The extended Access-list is generally applied close to the source (but not always).
3.  We can assign only one ACL per interface per protocol per direction, i.e., only one inbound and outbound ACL is permitted per interface.
4.  We can???t remove a rule from an Access-list if we are using numbered Access-list. If we try to remove a rule then whole ACL will be removed. If we are using named access lists then we can delete a specific rule.
5.  Every new rule which is added into the access list will be placed at the bottom of the access list therefore before implementing the access lists, analyses the whole scenario carefully.
6.  As there is an implicit deny at the end of every access list, we should have at least a permit statement in our Access-list otherwise all traffic will be denied.
7.  Standard access lists and extended access lists cannot have the same name.

### Important Notes
-   **Inbound access lists ???** (**Usually used with Extended Access-list**)  When an access list is applied on inbound packets of the interface then first the packets will processed according to the access list and then routed to the outbound interface.
-   **Outbound access lists ???** (**Usually used with Standard Access-list**) When an access list is applied on outbound packets of the interface then first the packet will be routed and then processed at the outbound interface.

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

### Important Notes
- IPV6 allows you to have more one address at the same interface
- Contiguous 0s are compressed (**i.e 47CD::A456:0124**)
- IPV6 is compatible with IPV4 (**128.42.1.87 in IPV4 is ::802A:157 or ::802A:0157 in IPV6**)
- YOU cannot have more than one :: in the IPV6 address (**2031::130F::09C0:876A:130B is Wrong**)
- Leading zeros in a field are optional (**2031:0:130F:0:0:9C0:876A:130B**)




### Converting IPV4 to IPV6
![](https://www.researchgate.net/profile/Sumit-Khandelwal-2/publication/271294793/figure/fig1/AS:392055806283779@1470484796300/IPv4-to-IPv6-Conversion-Method1-In-this-method-firstly-to-convert-the-Decimal-IPv4.png)

### Commands for IPV6 
- ipv6 unicast-routing
- ipv6 address FE80::# link-local
- ipv6 address [ipv6]

## RIPng
### Important Notes
- Unlike RIP, RIPng needs to be declared at every interface of the router
- Defualt router information need to be shared between routers and the command is written at every router

### Commands for RIPng
- ipv6 rip [NAME] enable
- ipv6 rip [NAME] default-information originate
- ipv6 router rip [NAME]
- redistribute ospf [<1-65535 ID] metric [0-16]
- redistribute eigrp [<1-65535 ID] metric [0-16]
- redistribute connected 

## OSPF V3
### Important Notes
- Unlike OSPF, OSPFV3 commands is written in the interfaces conneting the routers
- It is better to give the router a router id

### Commands for OSPF V3
- ipv6 ospf [1-65535 Process ID] area [OSPF area ID]
- ipv6 router ospf [1-65535 Process ID]
- router-id [N.N.N.N]
- default-information originate
- redistribute rip [NAME]
- redistribute eigrp [1-65535]
- redistribute connected 

## EIGRPV6
### Important Notes
- Unlike EIGRP, EIGRPV6 commands is written in the interfaces conneting the routers
- You must have a router-id
- You must write no shutdown to bring the protocol up

### Commands for EIGRPV6
- ipv6 router eigrp [1-65535 Process ID]
- no shutdown
- eigrp router-id [N.N.N.N]
- redistribute rip metric [10000][100][255][1][1500]
- redistribute ospf [1-65535 Process ID] metric [10000][100][255][1][1500]
- redistribute static
- redistribute connected 

## NAT 
**Network Address Translation (NAT)** is a process in which one or more local IP address is translated into one or more Global IP address and vice versa in order to provide Internet access to the local hosts.
### NAT inside and outside addresses
Inside refers to the addresses which must be translated. Outside refers to the addresses which are not in control of an organisation. These are the network Addresses in which the translation of the addresses will be done.
![](https://media.geeksforgeeks.org/wp-content/uploads/stati1c.png)
-   **Inside local address ???**  An IP address that is assigned to a host on the Inside (local) network. The address is probably not a IP address assigned by the service provider i.e., these are private IP address. This is the inside host seen from the inside network.
-   **Inside global address ???**  IP address that represents one or more inside local IP addresses to the outside world. This is the inside host as seen from the outside network.
-   **Outside local address ???**  This is the actual IP address of the destination host in the local network after translation.
-   **Outside global address ???**  This is the outside host as seen form the outside network. It is the IP address of the outside destination host before translation.
### Commands for NAT
![](https://i.ibb.co/g6BgZd4/NAT1.png)
![](https://i.ibb.co/R0bGJx9/NAT2.png)
![](https://i.ibb.co/hXYwXfc/NAT3.png)
![](https://i.ibb.co/4ZvzrJF/NAT4.png)

## Show Commands interpretations (Ipv6 commands and results are the same just write ipv6 instead of ip)
- show ip interface brief
  * This command provides a quick overview of all interfaces on the router including their IP addresses and status.
  
![](https://www.computernetworkingnotes.org/images/cisco/ccna-study-guide/show-ip-interface-brief.png)
- show controllers
  * This command is used to check the hardware statistic of interface including clock rate and cable status such as cable is attached or not. One end of serial cable is physically DTE, and other end is DCE. If cable is attached, it will display the type of cable.
  
![](https://www.computernetworkingnotes.org/images/cisco/ccna-study-guide/show-controllers.png)
- show ip route
  * Routers use routing table to take packet forward decision. This command displays routing table.
 
![](https://www.computernetworkingnotes.org/images/cisco/ccna-study-guide/show-ip-route.png)
- show spanning-tree command
  * To view the information about the STP operation, you can use the show spanning-tree command from the privileged-exec mode. The output of this command can be divided into three subsets. The first set contains information about the Root Bridge. The second set contains information about the switch itself. The third set lists the status of active interfaces that are participating in the STP operation.
 
![](https://www.computernetworkingnotes.org/images/cisco/ccna-study-guide/csg39-02-stp-show-spanning-tree.png)
- show vtp status
  * which shows you the VTP configuration ??? minus the password ??? for your switch. Note that you see the revision number here, as well as the mode and domain name
 
![](https://geek-university.com/wp-content/uploads/2015/10/show_vtp_status.jpg)
- show vtp counters
  * counters is very useful because it shows you the advertisements that you sent and received. If you are not receiving these, check the Trunk settings.
  
![](https://cdn.briefmenow.org/wp-content/uploads/200-125-v1/61.jpg)
- show ip protocols
  * The *show ip protocols* command displays the parameters and other information about the current state of any active IPv4 routing protocol processes configured on the router. The *show ip protocols* command displays different types of output specific to each routing protocol.
 
![](https://i.pinimg.com/originals/5d/f7/47/5df74779f8274c0f4b2d66b124f4c183.jpg)
![](http://humairahmed.com/blog/wp-content/uploads/2012/01/ospf_show_ip_prot-e1325672534768.png)
![](https://itexamanswers.net/wp-content/uploads/2015/06/i221226v1n3_221226-1.png)
- show ip rip
  * To display general RIP information, enter show ip rip at any context level. The resulting display will appear similar to the following:
 
![](https://i.ibb.co/Zm62KDv/Capture.png)
![](https://i.ibb.co/YBjQLL7/Capture.png)
- show ip ospf general
  * General output for the show ip ospf command
 
![](https://i.ibb.co/gRmLGNN/Capture.png)
- show ip ospf area [ ospf-area-id]
  * The [ospf-area-id] parameter shows information for the specified area. If no area is specified, information for all the OSPF areas configured is displayed.
  
![](https://i.ibb.co/qJrBRjN/Capture.png)
![](https://i.ibb.co/P1xvLMv/Capture.png)

- show ip ospf neighbor
  * Shows information about all neighbors.

![](https://www.thebryantadvantage.com/wp-content/uploads/2016/07/OSPF-16-Show-IP-OSPF-Neighbor-R3-R4.jpg)
![](https://i.ibb.co/4jqgTrw/Capture.png)

- show ip eigrp neighbors
  * this table consists of information of neighbor routers whose information is collected using Hellos. It consists of all directed neighbors.

![](https://www.certiology.com/wp-content/uploads/2015/10/Neighbor-Table.bmp)
![](https://i.ibb.co/zSnqd4B/Capture.png)

- show ip eigrp topology
  * The EIGRP topology table contains all of the routes that are known to each EIGRP neighbor. As an EIGRP router learns routes from its neighbors, those routes are installed in its EIGRP topology table.
  
![](https://www.certiology.com/wp-content/uploads/2015/10/Topology-Table.gif)
![](https://i.ibb.co/N6c63Wd/Capture.png)

- show access-lists
  * Displays contents of all access lists or one specified access list.

![](https://cdn.pcwdld.com/wp-content/uploads/13-show-ip-access-list.png)

- show ip bgp
  * Displays entries in the BGP routing table for one network prefix or the entire BGP routing table.
 
![](https://www.digitaltut.com/images/ROUTE/BGP/BGP_show_ip_bgp.jpg)

- show ip bgp summary
  * Displays a summary of the status of all BGP connections.

![](https://cdn.briefmenow.org/wp-content/uploads/642-892/52.png)
![](https://i.ibb.co/jLQjb8H/Capture.png)

- Show ip nat statistics
  * You will often want to look at NAT statistics, including information on which interfaces use NAT, how many entries are in the NAT table, how often they have been used, and, most importantly, how often packets have bypassed NAT.

![](http://1.bp.blogspot.com/-owR1qOCVNgY/US0TXDvFCsI/AAAAAAAAAQ4/5k0ljNCWFQk/s1600/show_ip_nat_statistics.png)

- show ip nat translations
  * To display active Network Address Translation (NAT) translation
 
![](https://selftechs.files.wordpress.com/2012/09/11.jpg)
![](https://i.ibb.co/cytYrX8/Capture.png)


