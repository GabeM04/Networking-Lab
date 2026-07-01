# Networking-Lab

## Overview

This project demonstrates a collapsed-core network architecture connecting a company's main office and branch office, with each office supporting ~100 users and being connected via an IPsec tunnel. The lab focuses on designing a network architecture from the ground-up, covering topics from initial device hardening to inter-office routing and network resilience.

## Focus Topics
* Network Engineering and Deployment
* Network Device Hardening
* HSRP
* EtherChannel
* Spanning Tree
* OSPF
* Port Security

## Status


### ✅ Completed
- Office A network architecture diagram created [View Diagram](office-a-design.md)
- Initial Office A topology deployed in Packet Tracer [View Deployment](office-a-deployment.md)
- Office A Device hardening baseline configured (SSH, local users, security settings)
- VLAN Creation
- Access port assignment and trunking
- Spanning Tree configuration and Layer 2 HSRP

### 🟡 In Progress

- OSPF routing between core and routers
  
### ⏳ Planned

- DHCP implementation (via Domain Controller + relay)
- Wireless deployment (WLC + AP provisioning)
- Management ACLs on VTY lines
- Failover testing and validation

### 🔜 Future Enhancements
- Office B network design and deployment
- Inter-office connectivity (simulated IPsec tunnel due to Packet Tracer constraints)



