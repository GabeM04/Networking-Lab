## Config Validation (1 - MGMT-ACL)

In this section, my plan is to immediately test the **MGMT-ACL** functionality first in order to confirm Network Device security. To do this, I first tried running the command ```ssh -l labadmin 1.1.1.1``` on PC1, which is on VLAN 10. The terminal **refused the connection**, confirming VTY access is blocked for non-VLAN 90 devices.

<img width="700" height="450" alt="image" src="https://github.com/user-attachments/assets/0a97b778-3921-4807-a3b9-0c9b4361f0c1" />

I then connected a laptop to ASW1-A on a VLAN 90 Access Port. I manually assigned the laptop the IP address 10.1.90.15 and tried running the same command ```ssh -l labadmin 1.1.1.1```. I was then prompted for a password which, when input, led me to R1-A's terminal.
<img width="700" height="450" alt="image" src="https://github.com/user-attachments/assets/04675801-6d29-4c3b-94ca-fd661c1cd243" />

* These two tests **confirm that the MGMT-ACL is working as intended**.
