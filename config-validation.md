## Config Validation (1 - MGMT-ACL)

In this section, my plan is to immediately test the **MGMT-ACL** functionality first in order to confirm Network Device security. To do this, I first tried running the command ```ssh -l labadmin 1.1.1.1``` on PC1, which is on VLAN 10. The terminal **refused the connection**, confirming VTY access is blocked for non-VLAN 90 devices.

<img width="700" height="450" alt="image" src="https://github.com/user-attachments/assets/0a97b778-3921-4807-a3b9-0c9b4361f0c1" />

I then connected a laptop to ASW1-A on a VLAN 90 Access Port. I manually assigned the laptop the IP address 10.1.90.15 and tried running the same command ```ssh -l labadmin 1.1.1.1```. I was then prompted for a password which, when input, led me to R1-A's terminal.
<img width="700" height="450" alt="image" src="https://github.com/user-attachments/assets/04675801-6d29-4c3b-94ca-fd661c1cd243" />

* These two tests **confirm that the MGMT-ACL is working as intended**.

## Config Validation (2 - HSRP)

Next, I want to confirm that HSRP is functioning properly and providing VLAN Default Gateway Redundancy. First, I confirmed that CSW1-A is the **Active** router for VLAN 10.

<img width="542" height="131" alt="image" src="https://github.com/user-attachments/assets/38ba41cb-a578-47fd-a91b-55c69add2d79" />

Next, I accessed PC4's terminal, which has the HSRP 10.10.10.1 gateway as its Default Gateway. I ran the command ```tracert 1.1.1.1``` to confirm the device's path to R1-A.

<img width="388" height="137" alt="image" src="https://github.com/user-attachments/assets/fd1baf85-e44c-436b-8ed9-195297ac869c" />

* As shown in the above command, the ICMP packet first hits 10.10.10.2 (CSW2-A) before reaching 1.1.1.1. This confirms CSW1-A is currently functioning as the default gateway for VLAN 10.

After confirming this behavior, I brought CSW1-A offline via toggling the power switch in Packet Tracer. Afterwards, I ran the command ```tracert 1.1.1.1``` again and confirmed that the ICMP packet first hit 10.10.10.3 (CSW2-A).

<img width="375" height="136" alt="image" src="https://github.com/user-attachments/assets/0090d18a-cdd4-49fa-ab0e-ddc1123e6f7f" />

This confirms that HSRP is working properly and the VLANs are not being routed through a single point of failure, which drastically improves network resilience and availability.

## Config Validation (3 - BPDU Guard)

After confirming HSRP functionality, I wanted to continue ensuring proper security measures. I decided to test BPDU Guard by disconnecting PC4 from ASW2-A's F0/1 port and instead connecting a Rogue Switch.

<img width="380" height="410" alt="image" src="https://github.com/user-attachments/assets/19d00253-6ac1-453e-8494-b0ca6314eafc" />

I then brought the interface online and confirmed that the port was instantly placed into an **err-disabled** state, confirming BPDU Guard successfully prevented a Rogue Switch being connected to an Edge Port.

<img width="907" height="290" alt="image" src="https://github.com/user-attachments/assets/0b577050-041a-49e1-963e-9425356b1a67" />

<img width="569" height="97" alt="image" src="https://github.com/user-attachments/assets/deac04e4-fb34-42a4-8c1b-7f803784ca14" />
