
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
   In Part 2, you will create VLANs, as specified in the table above, on both switches. You will then assign the VLANs to the appropriate interface. The show vlan command is          used to verify your configuration settings. Complete the following tasks on each switch
* **Step 1: Create VLANs on both switches.**  

      a.  Create and name the required VLANs on each switch from the table above.
      b.  Configure the management interface and default gateway on each switch using the IP address
          information in the Addressing Table. 
      c.  Assign all unused ports on both switches to the ParkingLot VLAN, configure them for static access mode,
          and administratively deactivate them.

* **Step 2:  Assign VLANs to the correct switch interfaces..**   

      a.  Assign used ports to the appropriate VLAN (specified in the VLAN table above) and configure them for
          static access mode. Be sure to do this on both switches
      b.  Issue the show vlan brief command and verify that the VLANs are assigned to the correct interfaces.
      
#### [Part 3:  Configure an 802.1Q Trunk Between the Switches ](README.md#часть-2-настройка-динамического-протокола-транкинга-dtp)
  In Part 3, you will manually configure interface F0/3 as a trunk.
  
* **Step 1:  Manually configure trunk interface F0/3.** 

      a.  Change the switchport mode on interface F0/3 to force trunking. Make sure to do this on both switches.
      b.  As a part of the trunk configuration, set the native VLAN to 8 on both switches. You may see error messages
          temporarily while the two interfaces are configured for different native VLANs.
      c.  As another part of trunk configuration, specify that VLANs 3, 4, and 8 are only allowed to cross the trunk
      d.  Issue the show interfaces trunk command to verify trunking ports, the Native VLAN and allowed VLANs
          across the trunk.
          
* **Step 2:  Manually configure S1’s trunk interface F0/5.** 

      a.  Configure the F0/5 on S1 with the same trunk parameters as F0/3. This is the trunk to the router
      b.  Save the running configuration to the startup configuration file on S1 and S2
      c.  Issue the show interfaces trunk command to verify trunking.
             Why does F0/5 not appear in the list of trunks?
             
#### [Part 4:  Configure Inter-VLAN Routing on the Router ](README.md#часть-2-настройка-динамического-протокола-транкинга-dtp) 

      a.  Activate interface G0/1 on the router.
      
      b.  Configure sub-interfaces for each VLAN as specified in the IP addressing table. All sub-interfaces use
          802.1Q encapsulation. Ensure the sub-interface for the native VLAN does not have an IP address
          assigned. Include a description for each sub-interface.
          
      c.  Use the show ip interface brief command to verify the sub-interfaces are operational. 
      
 #### [Part 5: Verify Inter-VLAN Routing is Working ](README.md#часть-2-настройка-динамического-протокола-транкинга-dtp) 
 
* **Step 1:  Complete the following tests from PC-A. All should be successful.**

      a.  Ping from PC-S1 to its default gateway.
      b.  Ping from PC-S1 to PC-S2
      c.  Ping from PC-S1 to S2
      
* **Step 2:  Complete the following test from PC-B.**     

      From the command prompt on PC-B, issue the tracert command to the address of PC-A.
      Question:
        What intermediate IP addresses are shown in the results?

      
### Solution:
#### Part 1: Build the Network and Configure Basic Device Settings

Configuring basic settings for the router
``` bash
Router>enable
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname R1
R1(config)#no ip domain-lookup 
R1(config)#enable secret class
R1(config)#line console 0
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#line vty 0 15
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
R1(config)#service password-encryption 
R1(config)#banner login #Unauthorized access is prohibited!!!# 
R1(config# do copy running-config startup-config
Destination filename [startup-config]? 
Building configuration...
[OK] 
R1(config)#clock timezone IST 3

*Jul 14 11:29:48.115: %SYS-6-CLOCKUPDATE: System clock has been updated from 11:29:48 UTC Tue Jul 14 2020 to 14:29:48 IST Tue Jul 14 2020, configured from console by console.
R1(config)#exit
R1#
*Jul 14 11:30:07.035: %SYS-5-CONFIG_I: Configured from console by console   
R1#clock set 14:30:00 Jul 14 2020
R1#
*Jul 14 11:30:00.000: %SYS-6-CLOCKUPDATE: System clock has been updated from 14:30:46 IST Tue Jul 14 2020 to 14:30:00 IST Tue Jul 14 2020, configured from console by console.
R1#clock update-calendar 
R1#
```

Configuring basic settings for both switches (example for S1 shown bellow):
``` bash
Switch>enable
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname S1
S1(config)#no ip domain-lookup 
S1(config)#enable secret class
S1(config)#line console 0
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#line vty 0 15
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#service password-encryption 
S1(config)#banner login #Unauthorized access is prohibited!!!# 
S1(config# do copy running-config startup-config
Destination filename [startup-config]? 
Building configuration...
[OK] 
S1(config)#clock timezone IST 3

*Jul 14 11:29:48.115: %SYS-6-CLOCKUPDATE: System clock has been updated from 11:29:48 UTC Tue Jul 14 2020 to 14:29:48 IST Tue Jul 14 2020, configured from console by console.
S1(config)#exit
S1#
*Jul 14 11:30:07.035: %SYS-5-CONFIG_I: Configured from console by console   
S1#clock set 14:30:00 Jul 14 2020
R1#
*Jul 14 11:30:00.000: %SYS-6-CLOCKUPDATE: System clock has been updated from 14:30:46 IST Tue Jul 14 2020 to 14:30:00 IST Tue Jul 14 2020, configured from console by console.

S1#
```
      
#### Part 2: Create VLANs and Assign Switch Ports

* **Step 1: Create VLANs on both switches.**<br />

  S1: 
``` bash
S1(config)#vlan 3
S1(config-vlan)#name Management
S1(config-vlan)#vlan 4
S1(config-vlan)#name Operations
S1(config-vlan)#vlan 7
S1(config-vlan)#name ParkingLot
S1(config-vlan)#vlan 8
S1(config-vlan)#name Native
S1(config-vlan)#
S1(config)#interface vlan 3
S1(config-if)#ip address 192.168.3.11 255.255.255.0
S1(config-if)#no shutdown
S1(config-if)#exit
S1(config)#ip default-gateway 192.168.3.1
S1(config)#interface range f0/1 - 2, f0/4, f0/6 - 14, f0/16 - 24, G0/1 - 2
S1(config-if-range)#
S1(config-if-range)#switchport mode access
S1(config-if-range)#switchport access vlan 7
S1(config-if-range)#shutdown

```

  S2: 
``` bash
S1(config)#vlan 3
S1(config-vlan)#name Management
S1(config-vlan)#vlan 4
S1(config-vlan)#name Operations
S1(config-vlan)#vlan 7
S1(config-vlan)#name ParkingLot
S1(config-vlan)#vlan 8
S1(config-vlan)#name Native
S1(config-vlan)#
S1(config)#interface vlan 3
S1(config-if)#ip address 192.168.3.12 255.255.255.0
S1(config-if)#no shutdown
S1(config-if)#exit
S1(config)#ip default-gateway 192.168.3.1
S1(config)#interface range f0/1 - 2, f0/4 - 14, f0/16 - 24, G0/1 - 2
S1(config-if-range)#
S1(config-if-range)#switchport mode access
S1(config-if-range)#switchport access vlan 7
S1(config-if-range)#shutdown

```
* **Step 2: Assign VLANs to the correct switch interfaces.**<br />
 
  S1: 
``` bash
S1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#interface FastEthernet0/15
S1(config-if)#switchport mode access
S1(config-if)#switchport access vlan 3
S1(config-if)#

```
  Verifying that the VLANs are assigned to the correct interfaceson S1:
``` bash
S1#show vlan brief
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/5
3    Management                       active    Fa0/15
4    Operations                       active    
7    ParkingLot                       active    Fa0/1, Fa0/2, Fa0/4, Fa0/6
                                                Fa0/7, Fa0/8, Fa0/9, Fa0/10
                                                Fa0/11, Fa0/12, Fa0/13, Fa0/14
                                                Fa0/16, Fa0/17, Fa0/18, Fa0/19
                                                Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                Fa0/24, Gi0/1, Gi0/2
8    Native                           active   
```
  S2: 
``` bash
S1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#interface FastEthernet0/15
S1(config-if)#switchport mode access
S1(config-if)#switchport access vlan 4
S1(config-if)#

```

 Verifying that the VLANs are assigned to the correct interfaces on S2:
``` bash

S2#show vlan brief
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    
3    Management                       active    
4    Operations                       active    Fa0/15
7    ParkingLot                       active    Fa0/1, Fa0/2, Fa0/4, Fa0/5
                                                Fa0/6, Fa0/7, Fa0/8, Fa0/9
                                                Fa0/10, Fa0/11, Fa0/12, Fa0/13
                                                Fa0/14, Fa0/16, Fa0/17, Fa0/18
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23, Fa0/24, Gi0/1, Gi0/2
8    Native                           active    
```

#### Part 3: Configure an 802.1Q Trunk Between the Switches

* **Step 1:  Manually configure trunk interface F0/3.**<br />

  S1:
``` bash
S1(config)#interface FastEthernet 0/3
S1(config-if)# description Trunk link to S2
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk native vlan 8
S1(config-if)# switchport trunk allowed vlan 3,4,8

```

  S2:
``` bash
S1(config)#interface FastEthernet 0/3
S1(config-if)# description Trunk link to S1
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk native vlan 8
S1(config-if)# switchport trunk allowed vlan 3,4,8

```

   Verify trunking ports, the Native VLAN and allowed VLANs across the trunk.
   ``` bash
   S1#show interfaces trunk
Port        Mode             Encapsulation  Status        Native vlan
Fa0/3       on               802.1q         trunking      8
Port        Vlans allowed on trunk
Fa0/3       3-4,8
Port        Vlans allowed and active in management domain
Fa0/3       3-4,8
Port        Vlans in spanning tree forwarding state and not pruned
Fa0/3       3-4,8
   ```
   
   
 ``` bash 
S2#show interfaces trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/3       on           802.1q         trunking      8
Port      Vlans allowed on trunk
Fa0/3       3-4,8
Port        Vlans allowed and active in management domain
Fa0/3       3-4,8
Port        Vlans in spanning tree forwarding state and not pruned
Fa0/3       3-4,8
S2#
 ```
 
* **Step 2:  Manually configure S1’s trunk interface F0/5.**<br />
``` bash 
S1(config)#interface FastEthernet 0/5
S1(config-if)#description Trunk link to R1
S1(config-if)#switchport mode trunk
S1(config-if)#switchport trunk native vlan 8
S1(config-if)#switchport trunk allowed vlan 3,4,8
S1(config-if)#
```
Issue the **show interfaces trunk** command to verify trunking.
 ``` bash 
S2#show interfaces trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/3       on           802.1q         trunking      8
Port      Vlans allowed on trunk
Fa0/3       3-4,8
Port        Vlans allowed and active in management domain
Fa0/3       3-4,8
Port        Vlans in spanning tree forwarding state and not pruned
Fa0/3       3-4,8
S2#
 ```
As we can see, interface F0/5 does not appear in the list of trunks, since  interface Gi0/1 on R1 is deactivated by default,
so the trunk has not been established yet
 
#### Part 4: Configuring Inter-VLAN Routing on the Router

  Activating interface Gi0/1 on the router:
 ``` bash
 R1(config)# interface GigabitEthernet0/1
R1(config-if)#description Trunk link to S1
R1(config-if)#no ip address
R1(config-if)#no shutdown

  ```
 Configuring sub-interfaces for each VLAN as specified in the IP addressing table:
``` bash
R1(config-if)#interface GigabitEthernet0/1.3
R1(config-subif)#description Default Gateway for VLAN 3
R1(config-subif)#encapsulation dot1Q 3
R1(config-subif)#ip address 192.168.3.1 255.255.255.0
R1(config-subif)#
R1(config-subif)#interface GigabitEthernet0/1.4
R1(config-subif)#description Default Gateway for VLAN 4
R1(config-subif)# encapsulation dot1Q 4
R1(config-subif)#ip address 192.168.4.1 255.255.255.0
R1(config-subif)#
R1(config-subif)#interface GigabitEthernet0/1.8
R1(config-subif)# description Native VLAN 8
R1(config-subif)#encapsulation dot1Q 8 native
R1(config-subif)#no ip address 
R1(config-subif)#
```
Verifying the sub-interfaces are operational:
``` bash
R1#show ip interface brief
Interface                  IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0         unassigned      YES unset  administratively down down    
GigabitEthernet0/1         unassigned      YES manual up                    up      
GigabitEthernet0/1.3       192.168.3.1     YES manual up                    up      
GigabitEthernet0/1.4       192.168.4.1     YES manual up                    up      
GigabitEthernet0/1.8       unassigned      YES manual up                    up      
Serial0/0/0                unassigned      YES unset  administratively down down    
Serial0/0/1                unassigned      YES unset  administratively down down    
R1#
```
#### Part 5: Verify Inter-VLAN Routing is Working

* **Step 1: Connectivity tests from PC-S1.**<br />

Ping from PC-S1 to its default gateway:
  
``` bash
C:\Users\student>ping 192.168.3.1

Pinging 192.168.3.1 with 32 bytes of data:
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.3.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

 ``` 
Ping from PC-S1 to PC-S2:
``` bash
C:\Users\student>ping 192.168.4.3

Pinging 192.168.4.3 with 32 bytes of data:
Reply from 192.168.4.3: bytes=32 time<1ms TTL=127
Reply from 192.168.4.3: bytes=32 time<1ms TTL=127
Reply from 192.168.4.3: bytes=32 time<1ms TTL=127
Reply from 192.168.4.3: bytes=32 time<1ms TTL=127

Ping statistics for 192.168.4.3:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

```
Ping from PC-S1 to S2:
``` bash
C:\Users\student>ping 192.168.3.12

Pinging 192.168.3.12 with 32 bytes of data:
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.3.12:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 1ms, Maximum = 1ms, Average = 1ms
```
* **Step 2: Connectivity tests from PC-S2.**<br />

Issuing the **tracert** command to the address of PC-S1
``` bash
C:\Users\Vlad>tracert 192.168.3.3

Tracing route to CCNA-RS-PC04 [192.168.3.3]
over a maximum of 30 hops:

  1    <1 ms    <1 ms    <1 ms  192.168.4.1
  2    <1 ms    <1 ms    <1 ms  CCNA-RS-PC04 [192.168.3.3]

Trace complete.
```
The intermediate IP address  shown in the results, is **192.168.4.1**, that is the default gateway for VLAN 4 

#### Configuration files 
[here](configs/).
