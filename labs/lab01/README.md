
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


### VLAN Table
| VLAN |    Name    | Interface Assigned                               |
| ---- | :--------: | :----------------------------------------------- |
| 3    | Management | **S1**: VLAN 3 <br />  **S2**: VLAN 3  <br />  **S1**: F0/15 |
| 4    | Operations | **S2**: F0/15                                        |
| 7    | ParkingLot | **S1**: F0/1-2, F0/4, F0/6-14, F0/16-24, G0/1-2<br/>  **S2**: F0/1-2,  F0/4-14, F0/16-24, G0/1-2      |
| 8    |   Native   | N/A                                              |


# Objectives

​*         **Part 1: Build the Network and Configure Basic Device Settings**

​*         **Part 2: Create VLANs and Assign Switch Ports**

​*         **Part 3: Configure an 802.1Q Trunk between the Switches**

​*         **Part 4: Configure Inter-VLAN Routing on the Router**

​*         **Part 5: Verify Inter-VLAN Routing is working**


### Task:
#### [Part 1: Build the Network and Configure Basic Device Settings ](README.md#часть-1-настройка-vtp)

### Решение:
#### Часть 1: Настройка VTP
