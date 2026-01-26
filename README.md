# VLAN Segmentation and Trunking Logic 🌐

## 📌 Overview

This project demonstrates the implementation of **Virtual Local Area Networks (VLANs)** to segment a switched network. 

By isolating different departments into logical groups, we optimize broadcast domains and enhance local network security, ensuring that data traffic is only accessible to authorized segments.

> **Status:** Fully Verified ✅

---

## 📐 Network Topology

The architecture uses a managed switch to handle multiple logical networks:

* **Central Switch:** Configured with VLAN tagging and port security.
* **End Devices:** Multiple workstations assigned to specific departments (VLANs) to test isolation and communication flow.

![Network Topology](01_topology_overview.png)

---

## 🛠️ Technical Specifications

* **Core Technology:** VLAN (Virtual LAN).
* **Trunking Standard:** IEEE 802.1Q.
* **Port Configurations:** Access and Trunk modes.
* **Verification Tool:** ICMP (Ping) tests for connectivity validation.

---

## ⚙️ Configuration Workflow

### 1. Logical Segmentation (VLANs)

VLANs were created and named to structure the network efficiently. This process limits the scope of broadcast traffic and organizes the infrastructure logically rather than physically.

* **VLAN 10:** Dedicated to the first logical segment.
* **VLAN 20:** Dedicated to the second logical segment.

### 2. Port and Trunk Assignment

* **Access Ports:** Assigned to specific PCs to ensure they belong to the correct VLAN.
* **Trunk Ports:** Configured on uplinks to allow multiple VLAN tags to pass through a single physical interface using the 802.1Q protocol.

![VLAN Configuration](02_SW1_VLANs.png)

---

## 🧪 Connectivity Results

Verification was performed to ensure that isolation is working as intended.

1.  **Intra-VLAN:** Communication between devices in the same VLAN is successful.
2.  **Inter-VLAN:** Traffic between different VLANs is isolated (as no router was added for Inter-VLAN routing in this specific lab), proving the security of the segmentation.

| Scenario | Result |
| :--- | :--- |
| Same VLAN Ping | Success (0% Loss) |
| Different VLAN Ping | Isolated (Timed Out) |

![Ping Verification](17_PC_VLAN10_ping.png)

---
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
ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8


ACL Example (VLAN 10 Test)
access-list 100 permit ip 192.168.10.0 0.0.0.255 192.168.10.0 0.0.0.255
access-list 110 deny ip any any


OSPF Example
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
````
**Developed by:** Eduardo Almeida  
*Focusing on Scalable and Secure Network Infrastructure.*
