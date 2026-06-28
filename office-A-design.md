
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


