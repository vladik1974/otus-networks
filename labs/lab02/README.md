# LANs Redundancy. STP
# Lab №2. – Building a Switched Network with Redundant Links .

### Network Diagram

![](network.jpg)

### 	Addressing Table
Device | Interface | IP Address | Subnet Mask
----- | ----- | ----- | -----
S1 | VLAN 1 | 192.168.1.1 | 255.255.255.0
S2 | VLAN 1 | 192.168.1.2 | 255.255.255.0
S3 | VLAN 1 | 192.168.1.3 | 255.255.255.0

### The Task:
#### [Part 1:	Build the Network and Configure Basic Device Settings](README.md#часть-1-создание-сети-и-настройка-основных-параметров-устройства-1)
#### [Part 2:	Determine the Root Bridge](README.md#часть-2-выбор-корневого-моста-1)
#### [Part 3:	Observe STP Port Selection Based on Port Cost](README.md#часть-3-наблюдение-за-процессом-выбора-протоколом-stp-порта-исходя-из-стоимости-портов-1)
#### [Part 4:	Observe STP Port Selection Based on Port Priority](README.md#часть-4-наблюдение-за-процессом-выбора-протоколом-stp-порта-исходя-из-приоритета-портов-1)
#### [Reflection](README.md#вопросы-для-повторения-1)
#### [Configuration files ](README.md#конфигурационные-файлы-здесь)

### Solution:
#### Part 1. Build the Network and Configure Basic Device Settings

*Step 1:	Cable the network as shown in the topology.*

Attaching the devices as shown in the topology diagram, and cable as necessary.

![](network_lab03_eve.png)

*Step 2. Initialize and reload the switches as necessary.*

*Step 3. Configure basic settings for each switch..*
  
Example for S1

Disabling DNS lookup and configuring the device name according to the topology.
``` bash
Switch(config)#hostname S1
S1(config)#no ip domain-lookup
S1(config)#exit
S1#wr
```
Assigning **class** as the encrypted privileged EXEC mode password.
``` bash
S1(config)#service password-encryption 
S1(config)#enable secret class
```
Assigning **cisco** as the console and vty passwords and enable login for console and vty lines. Configuring _logging synchronous_ for  console line. 

Assigning the password for  _console_
``` bash
S1(config)#line console 0
S1(config-line)#password cisco
S1(config-line)#exec-timeout 0 0 
S1(config-line)#logging synchronous
S1(config-line)#login
S1(config-line)#exit
```
Assigning the password for  _vty lines_
``` bash
S1(config)#line vty 0 15
S1(config-line)#password cisco
S1(config-line)#exec-timeout 0 0 
S1(config-line)#logging synchronous
S1(config-line)#login
S1(config-line)#exit
```

Configure a message of the day (MOTD) banner to warn users that unauthorized access is prohibited.
``` bash
S1(config)#banner motd #Unauthorized access is prohibited!!!#
```
Configure the IP address listed in the Addressing Table for VLAN 1 on all switches
and copy the running configuration to the startup configuration.
``` bash
S1#conf t
S1(config)#int vlan 1
S1(config)#ip address 192.168.1.1 255.255.255.0
S1(config)#no shutdown
S1(config)#exit
S1#copy running-config startup-config
```


In the same way, configuring  S2 и S3 switches.

*Step 4. Testing connectivity.*

Verify that the switches can ping one another.<br/>
Can S1 ping S2?

Can S1 ping S3?

Can S2 ping S3?

Troubleshoot until you are able to answer yes to all questions.

The answer: In order the switches can ping one another,  interface Vlan1 and  ports  F0/1 - F0/4 should be activated on all switches.

#### Part 2:	Determine the Root Bridge

*Step 1. Deactivate all ports on the switches.*

Example on S1. In the same way on S2 and S3 
``` bash
S1#conf t
S1(config)#interface range f0/1 - 24, G0/1 - 2
S1(config-if-range)#shutdown
```
*Step 2:	Configure connected ports as trunks.*

Example on S1. In the same way, on S2 and S3.
``` bash
S1(config)#int range f0/1 - 4
S1(config-if-range)#switchport mode trunk
S1(config-if-range)#
```
*Step 3.	Activate ports F0/2 and F0/4 on all switches.*

Example on S1. In the same way, on S2 and S3.
``` bash
S1#configure terminal
S1(config)#interface range f0/2, f0/4
S1(config-if-range)#no shutdown
```
Verifying ports activation
``` bash
S1#show interfaces status
```
``` bash

Port      Name               Status       Vlan       Duplex  Speed Type
Fa0/1                        disabled     1            auto   auto 10/100BaseTX
Fa0/2                        connected    trunk      a-full  a-100 10/100BaseTX
Fa0/3                        disabled     1            auto   auto 10/100BaseTX
Fa0/4                        connected    trunk      a-full  a-100 10/100BaseTX
```
*Step 4: Display spanning tree information.*

Switch S1
``` bash
S1# show spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0008.302d.4580
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0008.302d.4580
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec
Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Fa0/2               Desg FWD 19        128.2    P2p 
Fa0/4               Desg FWD 19        128.4    P2p 
```

Switch S2
``` bash
S2#show spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0008.302d.4580
             Cost        19
             Port        4 (FastEthernet0/4)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0015.fac1.4500
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time 300
Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Altn BLK 19        128.2    P2p 
Fa0/4            Root FWD 19        128.4    P2p 
S2#
```

Switch S3
``` bash
S3#show spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0008.302d.4580
             Cost        19
             Port        4 (FastEthernet0/4)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0011.2142.4580
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time 300
Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Desg FWD 19        128.2    P2p 
Fa0/4            Root FWD 19        128.4    P2p 
S3#
```

In the diagram below, recorded the Role and Status (Sts) of the active ports on each switch in the Topology
![](spanning-free.png)

Answers to the questions:
Which switch is the root bridge? 
> The root bridge is S1.

Why did spanning tree select this switch as the root bridge?
> S1 was selected as  the root bridge based on the lowest MAC address. Since the sums of priotity bridge + vlan id values are the same on all switches, the MAC addresses are compared and the lowest one is selected.

Which ports are the root ports on the switches?  
> Ports that are connected to the superior switch. In this case, those that are connected to the root bridge. Root Fwd (root forward).
> 
> On S2 - f0/4
> 
> On S3 - f0/4

Which ports are the designated ports on the switches? 
> Ports used for data transfer are designated Desg Fwd (designated forward)<br/> 
These are f0/2 and f0/4 on S1, and f0/2 on S3

What port is showing as an alternate port and is currently being blocked? 
> Port f0/2 on switch S2.

Why did spanning tree select this port as the non-designated (blocked) port?
> After the root bridge is selected, S2 and S3 will continue to forward BPDU packets from the root bridge through all ports except the root (root fwd), changing the values in them to their own (bridge id, port id, root path cost)
> 
>After receiving the BPDU packets from each other, and saw the same root bridge values in them, the switches will understand that there is redundancy (a loop) and one of the ports needs to be blocked.
Next, yit's needed to choose on which of the switches to block the port. The choice is made according to three criteria (the best value is the lowest)
> - Root Path Cost.
> - Bridge ID.
> - Port ID.
>
>S3 wins because it has a lowest bridge id, accordingly, the f0/2 port of S2 stops sending any packets and switches to listening mode for BPDU packets from S3, i.e. goes into blocked mode.

#### Part 3:	Observe STP Port Selection Based on Port Cost

*Step 1:	Locate the switch with the blocked port.

``` bash
S2#show spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0008.302d.4580
             Cost        19
             Port        4 (FastEthernet0/4)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0015.fac1.4500
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time 300
Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Altn BLK 19        128.2    P2p 
Fa0/4            Root FWD 19        128.4    P2p 
S2#
```
``` bash
S3#show spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0008.302d.4580
             Cost        19
             Port        4 (FastEthernet0/4)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0011.2142.4580
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time 300
Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Desg FWD 19        128.2    P2p 
Fa0/4            Root FWD 19        128.4    P2p 
S3#
```
The blocked port is  Fa0/2  on S2.

*Step 2:	Change port cost.*
In addition to the blocked port, the only other active port on  switch S2 is the port designated as the root port.
Lower the cost of this root port to 18 by issuing the *spanning-tree cost 18* interface configuration mode command.
``` bash
S2#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
S2(config)#interface f0/4
S2(config-if)#spanning-tree cost 18
```
*Step 3:	Observe spanning tree changes.*

Re-issuing the show spanning-tree command on both non-root switches.

Switch S2
``` bash
S2#show spanning-tree
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0008.302d.4580
             Cost        18
             Port        4 (FastEthernet0/4)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0015.fac1.4500
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time 15 
Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Desg FWD 19        128.2    P2p 
Fa0/4            Root FWD 18        128.4    P2p 
S2#
```

Switch S3
``` bash
S3#show spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0008.302d.4580
             Cost        19
             Port        4 (FastEthernet0/4)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0011.2142.4580
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time 300
Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Altn BLK 19        128.2    P2p 
Fa0/4            Root FWD 19        128.4    P2p 
S3#
```
Now the STP blocks the port Fa0/2 on S3, and Fa0/2 on S2 switches to Desg Fwd state.

Why did spanning tree change the previously blocked port to a designated port, and block the port that was a designated port on the other switch?
> Root Path Cost takes precedence over SID and Port ID.

*Step 4:	Remove port cost changes.*

* Issue the _no spanning-tree cost 18_ interface configuration mode command to remove the cost statement that has been created earlier.
``` bash
S2#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
S2(config)#int f0/4
S2(config-if)#no spanning-tree cost 18
S2(config-if)#end
```

* Re-issue the show spanning-tree command to verify that STP has reset the port on the non-root switches back to the original port settings.

Switch S2
``` bash
S2#show spanning-tree
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0008.302d.4580
             Cost        19
             Port        4 (FastEthernet0/4)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0015.fac1.4500
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time 15 
Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Altn BLK 19        128.2    P2p 
Fa0/4            Root FWD 19        128.4    P2p 
S2#
```

Switch S3
``` bash
S3#show spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0008.302d.4580
             Cost        19
             Port        4 (FastEthernet0/4)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0011.2142.4580
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time 300
Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Desg FWD 19        128.2    P2p 
Fa0/4            Root FWD 19        128.4    P2p 
S3#
```

#### Часть 4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов

* Включите порты F0/1 и F0/3 на всех коммутаторах.

Пример для коммутатора S1. Для S2 и S3 аналогичные настройки.
``` bash
S1#conf t
S1(config)#int range e0/0,e0/2
S1(config-if-range)#no shut
```

* Выполнить команду _show spanning-tree_ на коммутаторах некорневого моста.

``` bash
S2#sh spann

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             Cost        100
             Port        1 (Ethernet0/0)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.2000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  15  sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Root FWD 100       128.1    Shr
Et0/1               Altn BLK 100       128.2    Shr
Et0/2               Desg FWD 100       128.3    Shr
Et0/3               Desg FWD 100       128.4    Shr
```

Коммутатор S3
``` bash
S3#sh spann

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             Cost        100
             Port        3 (Ethernet0/2)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.3000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  15  sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Altn BLK 100       128.1    Shr
Et0/1               Altn BLK 100       128.2    Shr
Et0/2               Root FWD 100       128.3    Shr
Et0/3               Altn BLK 100       128.4    Shr
```

Какой порт выбран протоколом STP в качестве порта корневого моста на каждом коммутаторе некорневого моста?
> На S2 e0/0
> 
> На S3 e0/2
> 
Почему протокол STP выбрал эти порты в качестве портов корневого моста на этих коммутаторах?

> На коммутаторе S2 на _root bridge_ смотрят два порта, _e0/0_ и _e0/1_. В качестве _root fwd_ выбран порт _e0/0_ из-за наименьшего _port id_.
> На коммутаторе S3 на _root bridge_ смотрят _e0/2_ и _e0/3_. Наименьший _port id_ у порта _e0/2_.

________________________

#### 	Reflection

  1. After a root bridge has been selected, what is the first value STP uses to determine port selection?
        > - Root Path Cost.


  2. If the first value is equal on the two ports, what is the next value that STP uses to determine port selection?
        > - Bridge ID.

  3. If both values are equal on the two ports, what is the next value that STP uses to determine port selection?
        > - Port ID.


#### Configuration files [зде
