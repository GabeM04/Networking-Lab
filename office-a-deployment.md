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

## Device Hardening

Right after the network devices are deployed, it is best practice to create a security baseline. A malicious actor could easily exploit a poorly configured network device, so it's best to prioritize the security over the initial network setup.

Since I am configuring several network devices with the same settings, I decided to create a baseline security config in a notepad, which I could then paste into each device's CLI. The security config is as follows:
````
ip domain-name Lab.local
crypto key generate rsa
2048
ip ssh version 2
!
username labadmin privilege 15 secret Lab321
enable secret LabLogin
service password-encryption
login block-for 120 attempts 3 within 60
!
line con 0
login local
exec-timeout 10
!
line vty 0 15
login local
exec-timeout 5
transport input ssh
````
* The **first** set of commands creates a domain name and enables SSH. It is important to choose a strong key with at least 2048 bits and also enable SSH version 2 for stronger encryption.
* The **second** set of commands sets Secrets for both entering "Privileged Exec" mode and for logging into the "labadmin" user, which has Privilege 15 (Highest permissions). It also stores all clear-text passwords as Type 7 using the "service password-encryption" command. Type 7 encryption is not strong but it is used to prevent clear-text passwords being shown in the running config.
* The **third** set of commands enables local logins on the console line and sets a 10-minute timeout for the console. Since it is extremely rare for the console to be exploited by a malicious actor as they would need physical access to the device, a longer exec-timeout is fine.
* Finally, the **fourth** set of commands configures the terminal lines. It enables local logins, sets a shorter 5-minute timeout, and forces SSH to be used. This is best practice as Telnet is unencrypted and a vulnerability.
