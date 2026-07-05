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

I then brought CSW1-A back online and confirmed that it resumed its role as the Active Router for VLAN 10, due to the **preempt** setting being enabled. This confirms that HSRP is working properly and the VLANs are not being routed through a single point of failure, which drastically improves network resilience and availability.

## Config Validation (3 - Spanning Tree)

Spanning Tree validation operates very similarly to HSRP validation. First, I connected to ASW1-A and confirmed CSW1-A is the Root Bridge for VLAN 10 by using ```show spanning-tree vlan 10```.

<img width="554" height="278" alt="image" src="https://github.com/user-attachments/assets/c943d289-811c-4678-bf75-e7790e382c1c" />

* As shown in the image, G0/2, which is the Secondary, is currently an **Alternate Port**.

Similar to the HSRP Failover validation, I then brought CSW1-A down by disconnecting its power. I ran the command ```show spanning-tree vlan 10``` again and confirmed interface G0/2 (CSW2-A Link) is now a **Root Port**.

<img width="554" height="284" alt="image" src="https://github.com/user-attachments/assets/1e90a3c2-867f-4391-bf3d-7db2449c522d" />

Bringing CSW1-A back up also confirms that it resumes its role as Root Bridge for VLAN 10 once Spanning Tree reconverges.

<img width="706" height="383" alt="image" src="https://github.com/user-attachments/assets/7648b2d4-9b7a-4ec5-8438-c9093d7264f9" />


## Config Validation (4 - BPDU Guard)

After confirming HSRP functionality, I wanted to continue ensuring proper security measures. I decided to test BPDU Guard by disconnecting PC4 from ASW2-A's F0/1 port and instead connecting a Rogue Switch.

<img width="380" height="410" alt="image" src="https://github.com/user-attachments/assets/19d00253-6ac1-453e-8494-b0ca6314eafc" />

I then brought the interface online and confirmed that the port was instantly placed into an **err-disabled** state, confirming BPDU Guard successfully prevented a Rogue Switch being connected to an Edge Port.

<img width="907" height="290" alt="image" src="https://github.com/user-attachments/assets/0b577050-041a-49e1-963e-9425356b1a67" />

<img width="569" height="97" alt="image" src="https://github.com/user-attachments/assets/deac04e4-fb34-42a4-8c1b-7f803784ca14" />

## Config Validation (5 - Port Security)

To test Poret Security functionality, I decided to force the F0/2 Interface on ASW1-A (Connected to IP Phone0 and PC0) to learn a third MAC Address, despite it's **port-security maximum being set to 2**. I did this by connecting a Hub to the Interface, and then a second PC to the Hub.

<img width="453" height="568" alt="image" src="https://github.com/user-attachments/assets/382cf2ca-c316-4e03-88ef-61969d7a822d" />

I set the new PC to acquire its IP address via DHCP to force it to send packets through ASW1-A. I then observed ASW1-A's SYSLOG behavior.

<img width="923" height="188" alt="image" src="https://github.com/user-attachments/assets/94044c05-5598-405b-9f82-6f259ab1257b" />

* The above image shows a **port-security violation** occuring on F0/2. Due to the violation mode being set to **Restrict**, the port is not err-disabled but instead drops traffic from the violating MAC Address, preventing it from accessing the LAN while still servicing approved MACs. I then connected to PC7's terminal and confirmed it was unable to access LAN resources by attempting to ping 1.1.1.1.

<img width="392" height="187" alt="image" src="https://github.com/user-attachments/assets/2b648e4f-5582-4d24-b505-b49338bce159" />

This confirms that Port-Security is functioning properly and preventing unexpected quantities of devices from accessing a certain port.

## Config Validation (6 - OSPF)

For our final validation, we want to ensure that OSPF reconverges successfully after links fail. While in this lab environment, OSPF failing would cause minimal issues, in a more complex environment the routing protocol can prevent an entire network from becoming unreachable. To test, I first confirmed CSW2-A's route to 1.1.1.1 (R1-A) using ```show ip route 1.1.1.1```.

<img width="502" height="107" alt="image" src="https://github.com/user-attachments/assets/829e3b45-d046-40c7-874a-8316d4cb0e6a" />

* CSW2-A currently reaches 1.1.1.1 via the directly connected 100.1.100.17 address on G1/0/1.

I then connected to R1-A and brought down the G0/1 Interface which is connected to CSW2-A, simulating a link failure on the neighboring router. Afterwards, I ran the command ```show ip route 1.1.1.1``` again to confirm a route still existed to 1.1.1.1 via R2-A.

<img width="938" height="127" alt="image" src="https://github.com/user-attachments/assets/4a310967-5d28-488c-a7bc-876d0e413cbc" />

* This output confirms the address 1.1.1.1 can still be reached even though the directly connected link to R1-A is down. The path now goes through R2-A (G1/0/2).





