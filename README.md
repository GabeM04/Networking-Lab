# Networking-Lab

## Overview

This project demonstrates a collapsed-core network architecture in a medium-sized enterprise office environment. The lab focuses on designing a network architecture from the ground-up, covering topics from initial device hardening to inter-office routing and network resilience.

Note: The scope of this lab originally included a second office, Office B. This was meant to display IPsec Tunnel configurations. Due to Packet Tracer having limited support for this feature, I decided to focus on creating a resilient single office network instead and explore IPsec in a separate lab.

## Focus Topics
* Network Engineering and Deployment
* Network Security
* HSRP
* EtherChannel
* Spanning Tree
* OSPF
* DHCP

## Sections

[Initial Network Design](office-a-design.md)

[Network Deployment Into Packet Tracer](office-a-deployment.md)

[Config Validation and Testing](config-validation.md)

## Status


### ✅ Completed
- Office A network architecture diagram created 
- Initial Office A topology deployed in Packet Tracer
- Office A Device hardening baseline configured (SSH, local users, security settings)
- VLAN Creation
- Access port assignment and trunking
- Spanning Tree configuration and Layer 2 HSRP
- OSPF routing between core and routers
- DHCP implementation (via Domain Controller + Relay)
- WLAN Deployment
- Management VLAN ACL
- Failover testing and validation

## Lessons Learned

This project was extremely exciting and was a first for me. While I have done individual smaller labs with each of these topics, this is the first time I have architected an entire network from the ground-up. Due to this being a new experience, there were several lessons I've learned and am excited to add to my networking knowledge.

* It's crucial to ensure subnets have a clear purpose and roles aren't being mixed up. This allows for easier route summarization and understanding of an IP Address's role in the network without having to reference documentation every single time.
* While OSPF is extremely beneficial in very large network topologies, it can still serve a strong purpose in a smaller environment by ensuring high availability and link redundancy.
* When creating DHCP Pools, it's best to ensure what static IP addresses are required first. For example, if the VLAN is using an HSRP Default Gateway, three IPs are automatically not useable. This must be taken into account to avoid DHCP providing end devices with duplicate IP addresses.
* While VTP is very efficient for quickly distributing VLANs across Network Devices, it creates a single point of failure. If anything happens to the VTP Server or if a newly introduced device is accidentally elected the VTP Server, the entire LAN's VLAN database could be wiped.
* It's best to create HSRP Groups with the same ID as the VLAN they represent. This is not techncially necessary but aids in quick identification and documentation.
* Deploying the same commands to multiple devices can be much easier via copy-and-paste deployments, but doing so can allow one mistake to propagate throughout the network. It's important to confirm configuration functionality on a singular device before deploying it en masse.
* I did not initially consider adding a **Config Validation** section to this lab. During the deployment, I realized that displaying the configurations means nothing if I can't confirm they function as intended. A Network is not fully deployed until it is confirmed operational.
