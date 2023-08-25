## Развертывание коммутируемой сети с резервными каналами  

### 	Топология
![](https://github.com/permakov/otus/blob/main/lab9/Schema.jpg)   

### 	Таблица адресации  

Устройство |	Интерфейс	| IP-адрес |	Маска подсети
---------- | ---------- | ------- |--------------
R1 |	Gi0/0 | 192.168.10.1 |	255.255.255.0
R1 |	Loopback0 | 10.10.1.1 |	255.255.255.0
S1 |	VLAN 10 | 192.168.10.201 |	255.255.255.0
S2 |	VLAN 1 |	192.168.10.202	| 255.255.255.0
PC-A |	NIC |	DHCP |	255.255.255.0
PC-B |	NIC |	DHCP|	255.255.255.0

### Настройка маршрутизатора R1

![](https://github.com/permakov/otus/blob/main/lab9/R1.jpg)  
![](https://github.com/permakov/otus/blob/main/lab9/R2.jpg)  
![](https://github.com/permakov/otus/blob/main/lab9/R1_br.jpg)  

### Настройка и проверка основных параметров коммутатора  

a.	Настроил имя хоста для коммутаторов S1 и S2.  
b.	Запретил нежелательный поиск в DNS.  
c.	Настроил описания интерфейса для портов, которые используются в S1 и S2.  
d.	Установил для шлюза по умолчанию для VLAN управления значение 192.168.10.1 на обоих коммутаторах.  

### Настройка VLAN

![](https://github.com/permakov/otus/blob/main/lab9/Trunk.jpg)  
  
![](https://github.com/permakov/otus/blob/main/lab9/Trunk_2.jpg)  


### Настройка портов доступа  

На S1 и S2 переместил неиспользуемые порты из VLAN 1 в VLAN 999 и отключил неиспользуемые порты.  

![](https://github.com/permakov/otus/blob/main/lab9/Port_status.jpg)  

### Документирование и реализация функций безопасности порта  
Настройки безопасности порта по умолчанию:  
![](https://github.com/permakov/otus/blob/main/lab9/Default_PS.jpg)  



