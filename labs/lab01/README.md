
# Scaling VLANs.
# Lab №1. Configure Router-on-a-Stick Inter-VLAN Routing 

### Physical network topology diagram

![](physical_network_topology.jpeg)

### Logical network topology diagram

![](logical_network_topology.jpg)


### Addressing Table
| Device | Interface | IP Address   | Subnet Mask   | Default Gateway |
| ------ | --------- | ------------ | ------------- | --------------- |
| R1     | G0/0/1.3  | 192.168.3.1  | 255.255.255.0 | N/A             |
| R1     | G0/0/1.4  | 192.168.4.1  | 255.255.255.0 | N/A             |
| R1     | G0/0/1.8  | N/A          | N/A           | N/A             |
| S1     | VLAN 3    | 192.168.3.11 | 255.255.255.0 | 192.168.3.1     |
| S2     | VLAN 3    | 192.168.3.12 | 255.255.255.0 | 192.168.3.1     |
| PC-S1   | NIC       | 192.168.3.3  | 255.255.255.0 | 192.168.3.1     |
| PC-S2   | NIC       | 192.168.4.3  | 255.255.255.0 | 192.168.4.1     |


| VLAN |    Name    |                      Interface Assigned                      |
| ---- | :--------: | :----------------------------------------------------------: |
| 3    | Management |               S1: VLAN 3  S2: VLAN 3  S1: F0/6               |
| 4    | Operations |                          S2: F0/18                           |
| 7    | ParkingLot | S1: F0/2-4, F0/7-24, G0/1-2<br/>S2: F0/2-17, F0/19-24, G0/1-2 |
| 8    |   Native   |                             N/A                              |
