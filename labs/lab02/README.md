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
S1(config)#banner login #Unauthorized access is prohibited!!!#
```
Configure the IP address listed in the Addressing Table for VLAN 1 on all switches
and copy the running configuration to the startup configuration.
``` bash
S1#conf t
S1(config)#int vlan 1
S1(config)#ip address 192.168.1.1 255.255.255.0
S1(config)#exit
S1#copy running-config startup-config
```


Аналогично настраиваем коммутаторы S2 и S3.

*Шаг 4. Проверить связь.*

Проверть способность компьютеров обмениваться эхо-запросами.
Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S2?

Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S3?

Успешно ли выполняется эхо-запрос от коммутатора S2 на коммутатор S3?

Выполняйте отладку до тех пор, пока ответы на все вопросы не будут положительными.

Ответ: Чтобы проходили пинги, необходимо включить (активировать) интерфейсы Vlan1 и порты с e0/0 по e0/3 на всех коммутаторах.

#### Часть 2. Выбор корневого моста

*Шаг 1. Отключите все порты на коммутаторах.*

Пример на S1. На S2, S3 аналогично.
``` bash
S1#conf t
S1(config)#int range e0/0-3
S1(config-if-range)#shut
```
*Шаг 2. Настройте подключенные порты в качестве транковых.*

Пример на S1. На S2, S3 аналогично.
``` bash
S1(config)#int range e0/0-3
S1(config-if-range)#swit
S1(config-if-range)#switchport trunk encapsulation dot1q
S1(config-if-range)#switchport mode trunk
S1(config-if-range)#
```
*Шаг 3. Включите порты e0/1 и e0/3 на всех коммутаторах.*

Пример на S1. На S2, S3 аналогично.
``` bash
S1#conf t
S1(config)#int range e0/1,e0/3
S1(config-if-range)#no shut
```
Проверка активации портов
``` bash
S1#sh int status
```
``` bash
Port      Name               Status       Vlan       Duplex  Speed Type
Et0/0                        disabled     1            auto   auto unknown
Et0/1                        connected    trunk        auto   auto unknown
Et0/2                        disabled     1            auto   auto unknown
Et0/3                        connected    trunk        auto   auto unknown
```
*Шаг 4. Отобразите данные протокола spanning-tree.*

Коммутатор S1
``` bash
S1#sh spann

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.1000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/1               Desg LIS 100       128.2    Shr
Et0/3               Desg LIS 100       128.4    Shr
```

Коммутатор S2
``` bash
S2#sh spann

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             Cost        100
             Port        2 (Ethernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.2000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  15  sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/1               Root LRN 100       128.2    Shr
Et0/3               Desg LRN 100       128.4    Shr
```

Коммутатор S3
``` bash
S3#sh spann

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             Cost        100
             Port        4 (Ethernet0/3)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.3000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  15  sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/1               Altn BLK 100       128.2    Shr
Et0/3               Root FWD 100       128.4    Shr
```

На схеме ниже записаны роли и состояние (Sts) активных портов на каждом коммутаторе в топологии.
![](spanning-free.png)

Ответы на вопросы:
Какой коммутатор является корневым мостом?
> корневым мостом является S1.

Почему этот коммутатор был выбран протоколом spanning-tree в качестве корневого моста?
> S1 был выбран корневым исходя из наименьшего значения MAC-адреса. Т.к.суммы значений priotity bridge + vlan id однаковы на всех коммутаторах, то сравниваются значения MAC-адресов и выбирается наименьший

Какие порты на коммутаторе являются корневыми портами? 
> Порты, которые подключены к вышестоящему коммутатору. В данном случае те, которые подключены к корневому мосту. Обозначаются Root Fwd (root forward).
> 
> На S2 - e0/1
> 
> На S3 - e0/3

Какие порты на коммутаторе являются назначенными портами?
> Порты, используемые для  пересылки данных, обозначаются Desg Fwd (designated forward)

Какой порт отображается в качестве альтернативного и в настоящее время заблокирован?
> Порт e0/1 коммутатора S3.

Почему протокол spanning-tree выбрал этот порт в качестве невыделенного (заблокированного) порта?
> После того, как выбран root bridge, S2 и S3 продолжут отправлять BPDU-пакеты от root bridge через все порты, кроме корневых (root fwd), изменив в них значения на свои (bringe id, port id,root path cost)
> 
>Получив пакеты BPDU друг от друга, увидели в них одинаковые значения root bridge, коммутаторы поймут, что есть избыточность (петля) и нужно заблокировать один из портов.
Далее, нужно выбрать на каком из коммутаторов блокировать порт. Выбор происходит по трём критериям (лучшее значение наименьшее):
> - Root Path Cost.
> - Bridge ID.
> - Port ID.
>
>Побеждает S2, т.к. у него меньший bridge id, соответственно порт e0/1 у S3 перестаёт отправлять любые пакеты и переходит в режит прослушивания BPDU пакетов от S2, т.е. переходит в режим blocked.

#### Часть 3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов

*Шаг 1. Определить коммутатор с заблокированным портом.

``` bash
S2#sh spann

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             Cost        100
             Port        2 (Ethernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.2000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/1               Root FWD 100       128.2    Shr
Et0/3               Desg FWD 100       128.4    Shr
```
``` bash
S3#sh spann

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             Cost        100
             Port        4 (Ethernet0/3)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.3000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/1               Altn BLK 100       128.2    Shr
Et0/3               Root FWD 100       128.4    Shr
```
Заблокирован порт e0/1 на S3.

*Шаг 2. Измените стоимость порта.*

Уменьшить стоимость порта e0/3 корневого моста до 90:
``` bash
S3#conf t
S3(config)#int e0/3
S3(config-if)#spanning-tree cost 90
```
*Шаг 3. Просмотреть изменения протокола spanning-tree.*

Выполнить команду _show spanning-tree_ на обоих коммутаторах некорневого моста.

Коммутатор S2
``` bash
S2#sh spann

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             Cost        100
             Port        2 (Ethernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.2000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/1               Root FWD 100       128.2    Shr
Et0/3               Altn BLK 100       128.4    Shr
```
Коммутатор S3
``` bash
S3#sh spann

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             Cost        90
             Port        4 (Ethernet0/3)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.3000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/1               Desg FWD 100       128.2    Shr
Et0/3               Root FWD 90        128.4    Shr
```
Теперь STP блокирует порт e0/3 на S2, а e0/1 на S3 перешёл в состояние Desg Fwd.

Почему протокол spanning-tree заменяет ранее заблокированный порт на назначенный порт и блокирует порт, который был назначенным портом на другом коммутаторе?
> Приоритет стоимости пути (Root Path Cost) имеет бОльший приоритет относительно SID и Port ID.

*Шаг 4. Удалить изменения стоимости порта.*

* Вернём первоначальные настройки стоимости пути, выполнив команду _no spanning-tree cost 90_
``` bash
S3#conf t
S3(config)#int e0/1
S3(config-if)#no spanning-tree cost 90
```

* Повторно выполнить команду _show spanning-tree_, чтобы подтвердить, что протокол STP сбросил порт на коммутаторе некорневого моста, вернув исходные настройки порта.

Коммутатор S2
``` bash
S2#sh spann

VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     aabb.cc00.1000
             Cost        100
             Port        2 (Ethernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.2000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/1               Root FWD 100       128.2    Shr
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
             Port        4 (Ethernet0/3)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     aabb.cc00.3000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/1               Altn BLK 100       128.2    Shr
Et0/3               Root FWD 100       128.4    Shr
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
