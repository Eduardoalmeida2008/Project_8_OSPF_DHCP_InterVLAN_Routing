# Project_8_OSPF_DHCP_InterVLAN_Routing
Advanced network lab focusing on OSPF, DHCP, inter-VLAN routing, and basic ACL testing across multiple routers and switches.

# Project 8: OSPF, DHCP, and Inter-VLAN Routing Lab

## Overview
This lab simulates a medium-to-large corporate network with multiple VLANs, routers, and switches. The main objectives are to configure **OSPF**, **DHCP**, **inter-VLAN routing**, and test a **basic ACL** for VLAN 10.

## Topology
- **Routers:** 3
- **Switches:** 6
- **VLANs:** 12
- **PCs per VLAN:** 4

## Objectives
1. Implement **router-on-a-stick** with subinterfaces for each VLAN.
2. Configure **inter-VLAN routing** to enable communication within VLAN groups.
3. Configure **DHCP pools** for each VLAN and ensure clients receive IP addresses correctly.
4. Implement **basic ACL** testing on VLAN 10 (allowing access only to VLAN 10-20, deny all other traffic).
5. Establish **OSPF** between all routers and verify neighbor relationships.
6. Troubleshoot connectivity across multiple VLANs and routers.

## Devices & Interfaces
- Routers: Cisco 2911
- Switches: Cisco 2960
- PC connections: Access ports for each VLAN
- Trunk ports for switch-to-switch and switch-to-router links
- VLAN-to-router mapping (subinterfaces on router for each VLAN)

## VLANs
| VLAN ID | Name       | Switch  |
|---------|-----------|---------|
| 10      | VLAN10    | SW1     |
| 20      | VLAN20    | SW1     |
| 30      | VLAN30    | SW2     |
| 40      | VLAN40    | SW2     |
| 50      | VLAN50    | SW3     |
| 60      | VLAN60    | SW3     |
| 70      | VLAN70    | SW4     |
| 80      | VLAN80    | SW4     |
| 90      | VLAN90    | SW5     |
| 100     | VLAN100   | SW5     |
| 110     | VLAN110   | SW6     |
| 120     | VLAN120   | SW6     |

## Key Configurations

### Router-on-a-Stick Example (Router 1)
```bash
interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown

interface g0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
 no shutdown
...
DHCP Pool Example
Copiar código
Bash
ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
ACL Example (VLAN 10 Test)
Copiar código
Bash
access-list 100 permit ip 192.168.10.0 0.0.0.255 192.168.10.0 0.0.0.255
access-list 110 deny ip any any
OSPF Example
Copiar código
Bash
router ospf 1
 network 192.168.0.0 0.0.255.255 area 0
Switch Port Configurations
Access ports: switchport mode access, switchport access vlan <VLAN_ID>
Trunk ports: switchport mode trunk, allowing all VLANs
Native VLAN: 1
Testing & Verification
Ping within VLANs and across VLANs on the same router.
Ping across routers (inter-VLAN communication).
Verify OSPF neighbors: show ip ospf neighbor
Verify ACLs: show access-lists
Verify DHCP leases: show ip dhcp binding
Notes
This lab uses a basic ACL only on VLAN 10 for testing purposes.
All other VLANs allow full connectivity within their VLAN group.
DHCP pools are distributed across routers; no DHCP relay is used.
Observations
Inter-VLAN routing is confirmed via pings between PCs in different VLANs on the same router.
OSPF neighbors reach FULL state across all routers.
ACL successfully blocks unauthorized traffic outside VLAN 10-20.
Lessons Learned
Proper trunk configuration is essential for inter-VLAN communication.
Router-on-a-stick simplifies routing multiple VLANs on a single interface.
ACLs can easily block traffic but must be carefully applied to avoid disrupting DHCP or necessary communication.
OSPF ensures dynamic routing between routers for larger networks.
