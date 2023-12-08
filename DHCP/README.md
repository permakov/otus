## Лабораторная работа. Настройка DHCP  

## DHCPv4

### Топология сети
![alt-текст](https://github.com/permakov/otus/blob/main/DHCP/topology.jpg "Топология сети")  

### Таблица адресации

Device | Interface | IP Address | Subnet Mask | Default Gateway  
-------|-----------|------------|-------------|----------------  
R1 | G0/0/0 | 10.0.0.1 | 255.255.255.252 | N/A  
R1 | G0/0/1 | N/A | N/A | N/A  
R1 | G0/0/1.100 | 192.168.1.1 | 255.255.255.192 | N/A  
R1 | G0/0/1.200 | 192.168.1.65 | 255.255.255.224 | N/A  
R1 | G0/0/1.1000 |  |  | N/A  
R2 | G0/0/0 | 10.0.0.2 | 255.255.255.252 | N/A  
R2 | G0/0/1 | 192.168.1.97 | 255.255.255.240 | N/A  
S1 | VLAN 200 | 192.168.1.66 | 255.255.255.224 | 192.168.1.65  
S2 | VLAN 1 | 192.168.1.98 | 255.255.255.240 | 192.168.1.97  
PC-A | NIC | DHCP | DHCP | DHCP  
PC-B | NIC | DHCP | DHCP | DHCP    

### VLAN Table


VLAN | Name | Interface Assigned 
-------|-----------|------------  
1 | N/A | S2: Gi0/0, Gi0/03  
100 | Clients | S1: Gi0/2, Gi0/3  
200 | Management | S1: VLAN 200   
999 | Parking_Lot | S1: Gi0/0, Gi1/0-3  
1000 | Native | N/A  

### Цели:  
Создание и настройка сети  
Настройка и проверка DHCP сервера на маршрутизаторе R1  
Настройка и проверка DHCP Relay на маршрутизаторе R2  

### Создание и настройка сети  
Собрана сеть соггласно топологии предатсвленной выше.  
Проведена базовая настройка сетевого оборудования.  
Пример базовой настройки коммутатора:    
```no ip domain-lookup
enable secret class
line con 0
 password cisco
 logging synchronous
 login
line vty 0 4
 password cisco
 logging synchronous
 login
service password-encryption
banner motd ^CThis is switch S1^C
clock set 17:05:00 7 DEC 2023
```

Настройка межвлановой маршрутизации на R1
```
!
interface GigabitEthernet0/1
 no ip address
 duplex auto
 speed auto
 media-type rj45

!
interface GigabitEthernet0/1.100
 encapsulation dot1Q 100
 ip address 192.168.1.1 255.255.255.192
!
interface GigabitEthernet0/1.200
 encapsulation dot1Q 200
 ip address 192.168.1.65 255.255.255.224
!
interface GigabitEthernet0/1.1000
 encapsulation dot1Q 1000 native
```

Настройка статической маршрутизации на маршрутизаторах R1 и  R2
```
R1(config)#do sh ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR

Gateway of last resort is 10.0.0.2 to network 0.0.0.0

S*    0.0.0.0/0 [1/0] via 10.0.0.2
      10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C        10.0.0.0/30 is directly connected, GigabitEthernet0/0
L        10.0.0.1/32 is directly connected, GigabitEthernet0/0
      192.168.1.0/24 is variably subnetted, 4 subnets, 3 masks
C        192.168.1.0/26 is directly connected, GigabitEthernet0/1.100
L        192.168.1.1/32 is directly connected, GigabitEthernet0/1.100
C        192.168.1.64/27 is directly connected, GigabitEthernet0/1.200
L        192.168.1.65/32 is directly connected, GigabitEthernet0/1.200


R2#sh ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR

Gateway of last resort is 10.0.0.1 to network 0.0.0.0

S*    0.0.0.0/0 [1/0] via 10.0.0.1
      10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C        10.0.0.0/30 is directly connected, GigabitEthernet0/0
L        10.0.0.2/32 is directly connected, GigabitEthernet0/0
      192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.1.96/28 is directly connected, GigabitEthernet0/1
L        192.168.1.97/32 is directly connected, GigabitEthernet0/1
```

Проведена базовая настройка комутаторов. Настройка аналогична представленной выше для маршрутизатора R1.  
Созданы VLANы и портам мазначены соответствующие VLAN  

```
S1#sh vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active
100  Client                           active    Gi0/2, Gi0/3
200  MGMT                             active
999  Parking Lot                      active    Gi0/0, Gi1/0, Gi1/1, Gi1/2
                                                Gi1/3
1000 Native                           active
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
1    enet  100001     1500  -      -      -        -    -        0      0
100  enet  100100     1500  -      -      -        -    -        0      0
200  enet  100200     1500  -      -      -        -    -        0      0
999  enet  100999     1500  -      -      -        -    -        0      0
1000 enet  101000     1500  -      -      -        -    -        0      0
1002 fddi  101002     1500  -      -      -        -    -        0      0
1003 tr    101003     1500  -      -      -        -    -        0      0

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
1004 fdnet 101004     1500  -      -      -        ieee -        0      0
1005 trnet 101005     1500  -      -      -        ibm  -        0      0

Remote SPAN VLANs
------------------------------------------------------------------------------


Primary Secondary Type              Ports
------- --------- ----------------- ------------------------------------------

```
Порт Gi0/1 настроен как Trunk:  
```
S1#sh int trun

Port        Mode             Encapsulation  Status        Native vlan
Gi0/1       on               802.1q         trunking      1000

Port        Vlans allowed on trunk
Gi0/1       100,200,1000

Port        Vlans allowed and active in management domain
Gi0/1       100,200,1000

Port        Vlans in spanning tree forwarding state and not pruned
Gi0/1       100,200,1000
```


### Настройка и проверка DHCP сервера на маршрутизаторе R1

Настроены 2 пула на маршрутизаторе R1:
```
ip dhcp excluded-address 192.168.1.1 192.168.1.5
ip dhcp excluded-address 192.168.1.97 192.168.1.101
!
ip dhcp pool Client-1
 network 192.168.1.0 255.255.255.192
 default-router 192.168.1.1
 domain-name ccna-lab.com
 lease 2 12 30
!
ip dhcp pool R2_Client_Lan
 network 192.168.1.96 255.255.255.240
 default-router 192.168.1.97
 domain-name ccna-lab.com
 lease 2 12 30
```
Проверка настройки DHCP:
 
 ```
R1#sh ip dhcp pool
Pool Client-1 :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0
 Total addresses                : 62
 Leased addresses               : 2
 Pending event                  : none
 1 subnet is currently in the pool :
 Current index        IP address range                    Leased addresses
 192.168.1.8          192.168.1.1      - 192.168.1.62      2

Pool R2_Client_Lan :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0
 Total addresses                : 14
 Leased addresses               : 2
 Pending event                  : none
 1 subnet is currently in the pool :
 Current index        IP address range                    Leased addresses
 192.168.1.104        192.168.1.97     - 192.168.1.110     2
```
```
R1#sh ip dhcp binding
Bindings from all pools not associated with VRF:
IP address          Client-ID/              Lease expiration        Type
                    Hardware address/
                    User name
192.168.1.6         0100.5079.6668.01       Dec 10 2023 06:12 AM    Automatic
192.168.1.7         0150.0000.0700.00       Dec 11 2023 02:54 AM    Automatic
192.168.1.102       0100.5079.6668.06       Dec 10 2023 06:17 AM    Automatic
192.168.1.103       0150.0000.0800.00       Dec 11 2023 03:40 AM    Automatic

```
```
R1#sh ip dhcp server statistics
Memory usage         59142
Address pools        2
Database agents      0
Automatic bindings   4
Manual bindings      0
Expired bindings     0
Malformed messages   0
Secure arp entries   0

Message              Received
BOOTREQUEST          0
DHCPDISCOVER         6
DHCPREQUEST          4
DHCPDECLINE          0
DHCPRELEASE          0
DHCPINFORM           0

Message              Sent
BOOTREPLY            0
DHCPOFFER            4
DHCPACK              4
DHCPNAK              0

```

Проверка на клиенте:
```
VPCS> dhcp -r
DORA IP 192.168.1.6/26 GW 192.168.1.1

VPCS> ping 192.168.1.97

84 bytes from 192.168.1.97 icmp_seq=1 ttl=254 time=5.507 ms
84 bytes from 192.168.1.97 icmp_seq=2 ttl=254 time=4.387 ms
84 bytes from 192.168.1.97 icmp_seq=3 ttl=254 time=3.250 ms
84 bytes from 192.168.1.97 icmp_seq=4 ttl=254 time=3.306 ms
84 bytes from 192.168.1.97 icmp_seq=5 ttl=254 time=3.478 ms
```

### Настройка и проверка DHCP Relay на маршрутизаторе R2 

 На интерфейсе Gi0/1 маршрутизатора R2 настроен relay:
 ```
!
interface GigabitEthernet0/1
 ip address 192.168.1.97 255.255.255.240
 ip helper-address 10.0.0.1
 duplex auto
 speed auto
 media-type rj45
```

Проверка получения алреса со стороны клиента:
```
VPCS> dhcp -r
DORA IP 192.168.1.102/28 GW 192.168.1.97

VPCS> ping 192.168.1.1

84 bytes from 192.168.1.1 icmp_seq=1 ttl=254 time=3.255 ms
84 bytes from 192.168.1.1 icmp_seq=2 ttl=254 time=5.509 ms
84 bytes from 192.168.1.1 icmp_seq=3 ttl=254 time=3.843 ms
84 bytes from 192.168.1.1 icmp_seq=4 ttl=254 time=3.062 ms
84 bytes from 192.168.1.1 icmp_seq=5 ttl=254 time=3.579 ms

VPCS>
```
