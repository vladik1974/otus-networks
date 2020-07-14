
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
#### [Part 1: Build the Network and Configure Basic Device Settings ](README.md#part-1-build-the-network-and-configure-basic-device-settings)

* **Step 1: Cable the network as shown in the topology**
* **Step 2: Configure basic settings for the router**

      a.  Console into the router and enable priviliged EXEC mode
      b.  Enter configuration mode
      c.  Assign a device name to the router
      d.  Disable DNS lookup to prevent the router from attempting to translate incorrectly entered commands as though they were host names.
      e.  Assign class as the privileged EXEC encrypted password
      f.  Assign cisco as the console password and enable login
      g.  Assign cisco as the VTY password and enable login
      h.  Encrypt the plaintext passwords.
      i.  Create a banner that warns anyone accessing the device that unauthorized access is prohibited
      j.  Save the running configuration to the startup configuration file.
      k.  Set the clock on the router
      
* **Step 3: Configure basic settings for each switch.**  
 
      a.  Console into the switch and enable priviliged EXEC mode
      b.  Enter configuration mode
      c.  Assign a device name to the switch
      d.  Disable DNS lookup to prevent the switch from attempting to translate incorrectly entered commands as though they were host names.
      e.  Assign class as the privileged EXEC encrypted password
      f.  Assign cisco as the console password and enable login
      g.  Assign cisco as the VTY password and enable login
      h.  Encrypt the plaintext passwords.
      i.  Create a banner that warns anyone accessing the device that unauthorized access is prohibited
      j.  Save the running configuration to the startup configuration file.
      k.  Set the clock on the switch
      
* **Step 4:  Configure PC hosts.**   
     _Refer to the Addressing Table for PC host address information._     
   

#### [Part 2: Create VLANs and Assign Switch Ports ](README.md#часть-2-настройка-динамического-протокола-транкинга-dtp)

### Solution:
#### Part 1: Build the Network and Configure Basic Device Settings

