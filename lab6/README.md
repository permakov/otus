
## Внедрение маршрутизации между виртуальными локальными сетями

### Тополония сети
![](https://github.com/permakov/otus/blob/main/lab6/Topology.jpg)

### Таблица адресации

Устройство |	Интерфейс |	IP-адрес |	Маска подсети |	Шлюз по умолчанию  
---------- | ---------- | -------- | -------------- | -----------------
R1 |	e0/0.10 |	192.168.10.1 |	255.255.255.0 |	—  
R1 |	e0/0.20 |	192.168.20.1 |	255.255.255.0 |	—  
R1 |	e0/0.30 |	192.168.30.1 |	255.255.255.0 |	—  
R1 |	e0/0.1000 |	- |	- |	—  
S1 |	VLAN 10 |	192.168.10.11 |	255.255.255.0 |	192.168.10.1  
S2 |	VLAN 10 |	192.168.10.12 |	255.255.255.0 |	192.168.10.1  
PC-A |	NIC |	192.168.20.3 |	255.255.255.0 |	192.168.20.1  
PC-B |	NIC |	192.168.30.3 |	255.255.255.0 |	192.168.30.1  

### Таблица VLAN

VLAN |	Имя |	Назначенный интерфейс   
---------- | ---------- | --------
10  | MGMT | S1: VLAN10; S2: VLAN10    
20 | Sales |S1: e0/2
30 | Operations |S2: e0/2
999 | Parking_Lot | S1: e0/3 S2: e0/1, e0/3
1000 |  - | -  



### Создание сети и настройка основных параметров устройства  
Настроил базовые параметры для маршрутизатора    

![](https://github.com/permakov/otus/blob/main/lab6/R1_conf.jpg)    

Настройил базовые параметры каждого коммутатора  

![](https://github.com/permakov/otus/blob/main/lab6/S1_conf.jpg)  
![](https://github.com/permakov/otus/blob/main/lab6/S2_conf.jpg)  

Настроил узлы ПК  

###  Создание сетей VLAN и назначение портов коммутатора

![](https://github.com/permakov/otus/blob/main/lab6/S1_VLAN_2.jpg)
![](https://github.com/permakov/otus/blob/main/lab6/S2_VLAN_2.jpg)  


### Конфигурация магистрального канала стандарта 802.1Q между коммутаторами  

Вручную настройте магистральный между коммутаторами S1 и S2.  
Вручную настройте магистральный между коммутатором S1 и маршрутизатором R1  
  
![](https://github.com/permakov/otus/blob/main/lab6/S2_trunk.jpg)  
![](https://github.com/permakov/otus/blob/main/lab6/S2_trunk_2.jpg)  
![](https://github.com/permakov/otus/blob/main/lab6/S1_trunk.jpg)  
![](https://github.com/permakov/otus/blob/main/lab6/S1_trunk_to_R1.jpg)  

###  Настройка маршрутизации между сетями VLAN  
Настроил подинтерфейсы для каждой VLAN, как указано в таблице IP-адресации. Все подинтерфейсы используют инкапсуляцию 802.1Q  
![](https://github.com/permakov/otus/blob/main/lab6/R1_subint.jpg)  

### Проверка работы маршрутизации между VLAN  
![](https://github.com/permakov/otus/blob/main/lab6/PING_1.jpg)  
![](https://github.com/permakov/otus/blob/main/lab6/PING_2.jpg)  
