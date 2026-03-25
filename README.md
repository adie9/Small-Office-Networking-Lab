# Small-Office-Networking-Lab

## Overview

This project simulates a small office enterprise network designed and implemented using Cisco Packet Tracer. The lab focuses on core CCNA routing, switching, and IP services concepts, including VLAN segmentation, inter-VLAN routing, and centralized DHCP.

## Network Topology

The simulated office network consists of:

- One router performing Router-on-a-Stick for inter-VLAN routing
- One distribution switch
- Two access switches
- Multiple end-user PCs across departmental VLANs
- A centralized DHCP server connected directly to the router on a routed subnet

  ![Topology Image](Topology/Small-Office-Network.png)

## VLAN and IP Addressing Scheme

| VLAN ID | Department | Subnet       | Default Gateway |
| ------- | ---------- | ------------ | --------------- |
| 10      | Admin      | 192.168.10.0 | 192.168.10.1    |
| 20      | HR         | 192.168.20.0 | 192.168.20.1    |
| 30      | Sales      | 192.168.30.0 | 192.168.30.1    |
| 40      | IT         | 192.168.40.0 | 192.168.40.1    |

### Server Network (Routed Interface)

- Subnet: 192.168.50.0
- Router Interface: 192.168.50.1
- DHCP Server: 192.168.50.2

## Technologies and Concepts Implemented

- VLAN creation and segmentation
- 802.1Q trunking between switches and router
- Router-on-a-Stick (inter-VLAN routing)
- Centralized DHCP Server
- DHCP relay using `ip helper-address`
- Extended ACLs for inter-department access control
- Foundational network security through segmentation and least-privilege design

## Access Control Policies (ACL Implementation)

To enforce department-level security, extended ACLs were applied inbound on router sub-interfaces. Policies include:

### **Admin (VLAN 10)**

- Full access to all internal networks
- Trusted department

### **HR (VLAN 20)**

- Allowed to access the Server VLAN
- Restricted from accessing Admin (VLAN 10)
- Restricted from accessing IT (VLAN 40)

### **Sales (VLAN 30)**

- Allowed to access the Server VLAN
- Restricted from Admin, HR, and IT VLANs

### **IT (VLAN 40)**

- Full access to all internal networks
- Trusted department

### **Server VLAN (50)**

- Accessible from all business VLANs
- Centralized services (DHCP, future expansion)

ACLs were designed using a least-privilege model to ensure departments only access resources required for their role.

## DHCP Design

The centralized DHCP server provides IP addressing for all user VLANs. Because the DHCP server resides on a separate routed subnet, DHCP requests are forwarded using DHCP relay (ip helper-address) configured on each router sub-interface.

## Repository Structure

```
small-office-network-lab/
│
├── README.md
├── topology/
│   └── Small_Office_Network.png
├── configs/
│   ├── router_config.txt
│   ├── switch_DS1.txt
│   ├── switch_AS1.txt
│   └── switch_AS2.txt
└── packet-tracer/
    └── Small_Office_Lab.pkt
```

## Testing and Verification

The following were verified in the lab:

- End devices reeive IP addresses from the correct DHCP scopes
- Devices can communicate within and across VLANs via the router
- DHCP relay functions correctly across routed subnets
- Default gateway and IP addressing operate as expected
- ACLs correctly restrict HR and Sales from accessing unauthorized VLANs
- Server VLAN is reachable from all departments

## Future Enhancements

Planned improvements for this lab:

- NAT
- Port security on access switch interfaces
- etc.

## Notes

- Configuration files are exported running configurations from each device
- This project is for learning, demonstration, and portfolio purposes
