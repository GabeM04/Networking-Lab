
## Network & VLANs

| VLAN | Purpose   | Office A Subnet |
|------|-----------|-----------------|
| 10   | Users     | 10.1.10.0/24    |
| 20   | Phones    | 10.1.20.0/26    |
| 30   | Servers   | 10.1.30.0/28    |
| 40   | Wi-Fi     | 10.1.40.0/24    |
| 90   | Management| 10.1.90.0/27    |

My thought process here is that using a 10.0.0.0/8 network allows me to easily separate the two offices at the second octet. This will make summarization for ACLs/routing a lot easier, as Office A is in 10.1.0.0/16 and Office B is in 10.2.0.0/16.
Variable Length Subnet Masking (VLSM) was used to appropriately size the subnets while still allowing room for growth. The “Management” VLAN in Office B is smaller than in Office A, as it is a smaller branch office and deploys less infrastructure

## Initial Network Planning

![Office A Diagram1](diagrams/OfficeA1.png)

The diagram above shows my initial topology for Office A. My primary goal is creating redundancy while maximizing throughput. Each Access Switch trunks to each Distribution Switch, which then trunks to each Core Switch. The Core Switches would then perform inter-VLAN routing and HSRP for redundancy. While designing this architecture, I noticed the Distribution Switches added increased Spanning Tree and routing complexity while performing no beneficial role in the network. With the Core Switches handling inter-VLAN routing and HSRP, the Distribution Switches are only acting as a relay with no Layer 3 or policy functionality in this design. This results in all Layer 3 networking being done at the Core Layer, leaving the Distribution Layer underutilized.

While larger enterprises may benefit from this “Three-Tier” network architecture design, the offices being simulated in this lab would better benefit from a “Collapsed Core” design. In this design, the Distribution and Core Layers are collapsed into one “Collapsed Core”, reducing complexity. After evaluating the needs of each office which serve no more than 100 users each, I made the architectural decision to convert to a “Collapsed Core” design.

## Planning the "Collapsed Core Architecture"

![Office A Diagram2](diagrams/OfficeARevised.png)

The above diagram represents the revised architectural design for Office A’s network. The Core Layer has been collapsed, combining the functions of the Distribution and Core layer within two Layer 3 switches. Layer 2 EtherChannel connects the two Core Switches, providing increased throughput and VLAN default gateway redundancy via HSRP. Each server, which will be described in more depth later, is connected to an Access Switch via LACP, providing the redundancy and throughput required for network resources. Each Access Switch is now dual-homed to each Collapsed Core switch, leveraging Spanning Tree for loop prevention and redundancy. This design significantly improves on the complexity of the previous architecture, while still focusing on high availability.

While both offices will have relatively small networks that would operate just fine using static routes, I also plan on implementing OSPF Dynamic Routing to leverage Equal-Cost Multi-Path (ECMP), route convergence, and automatic failovers. While HSRP configured between the routers will allow automatic failover in case one router goes down, it will still result in a loss of network reachability from the Core switches if the Active router is online but its route to the internet fails. OSPF will allow routes to be learned dynamically and failover if one route goes down.

I’ve also added a Wireless LAN Controller (WLC) trunked to one of the Collapsed Core switches via a trunk port, alongside two Lightweight Access Points (APs). The APs will be provisioned through Zero-Touch Provisioning via a DNS record configured on the Domain Controller. The trunk connection to the WLC will allow the Wi-Fi and Management VLANs, allowing CAPWAP control and data traffic from the APs to the WLC.

Finally, it’s important to determine how IPs will be provided in the network. While the routers/Core Switches can operate as a DHCP server, this is not best practice and will confuse roles. Instead, DHCP will be configured on the Domain Controller (DC). VLAN Default Gateways will be configured to relay DHCP requests to the DC, allowing automatic IP provisioning while keeping the DHCP Server Role off of the main network devices.

## HSRP/STP Alignment

| VLAN | HSRP/STP Active/Root | HSRP Standby/Secondary |
|------|----------------------|------------------------|
| 10   | CSW1                 | CSW2                   |
| 20   | CSW1                 | CSW2                   |
| 30   | CSW2                 | CSW1                   |
| 40   | CSW2                 | CSW1                   |
| 90   | CSW1                 | CSW2                   |





