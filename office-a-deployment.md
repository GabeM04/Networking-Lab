## Initial Deployment Into Packet Tracer

![OfficeA-Initial-Deployment](diagrams/OfficeA-Initial-Deployment.png)

This initial deployment phase involves recreating the diagram in Packet Tracer. I decided to use Cisco 2911 Router, 3560-24PS Multilayer Switch, and 2960-24TT Switch. These devices are capable of performing all of the functions necessary in the lab, including OSPF, HSRP, and STP.

A few things I noticed while deploying this lab in Packet Tracer:
* The servers currently only have one NIC available. I will have to look into options for enabling LACP with dual-NIC.
* CSW1's local interface connected to the WLC is a FastEthernet Port, while the WLC's local interface is a GigabitEthernet Port. If there are any issues, I will have to look into ensuring the link speeds match.
* The Core Switches were not able to be turned on without manually adding **Power Supply Modules** into the "Physical" tab of the switch. I did not know Packet Tracer had in-depth physical modules and would definitely like to look into it more.

Next Steps:
* Creating secure, privileged accounts.
* Ensuring Switches follow security best practice (enabling SSH only, strong password).
