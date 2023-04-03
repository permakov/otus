## Базовая настройка коммутатора
### 	Задачи
#### Часть 1. Проверка конфигурации коммутатора по умолчанию
#### Часть 2. Создание сети и настройка основных параметров устройства
•	Настройте базовые параметры коммутатора.  
•	Настройте IP-адрес для ПК.
#### Часть 3. Проверка сетевых подключений
•	Отобразите конфигурацию устройства.  
•	Протестируйте сквозное соединение, отправив эхо-запрос.  
•	Протестируйте возможности удаленного управления с помощью Telnet.  
###	Необходимые ресурсы
•	1 коммутатор (Cisco 2960 с ПО Cisco IOS версии 15.2(2) с образом lanbasek9 или аналогичная модель)  
•	1 ПК (под управлением Windows с программой эмуляции терминала, например, Tera Term)  
•	1 консольный кабель для настройки устройства на базе Cisco IOS через консольный порт.  
•	1 кабель Ethernet, как показано в топологии.  


## Часть 1. Создание сети и проверка настроек коммутатора по умолчанию
### Шаг 1. Создал сеть согласно топологии.
•	Подсоединил консольный кабель, как показано в топологии.  
•	Установил консольное подключение к коммутатору с компьютера PC-A с помощью по Terminal  

### Схема стенда
![alt-текст](https://github.com/permakov/otus/blob/main/lab1/Laba1.jpg "Схема стенда")

### Вопросы
#### Почему нужно использовать консольное подключение для первоначальной настройки коммутатора?  
• Подключение по консольному кабелю необходимо так как еще не настроен виртуальный интерфейс на коммутаторе, и это единственный сейчас доступный метод подключения к нему.
#### Почему нельзя подключиться к коммутатору через Telnet или SSH?  
• К коммутатору нельзя подключиться по Telnet или SSH так как не настроен виртуальный интерфейс на коммутаторе, и соответственно у коммутатора нет IP адреса.

### Шаг 2. Проверьте настройки коммутатора по умолчанию.   

• Подключился по консольному кабелю.  
• Перешёл в привилегированный режим.  
• Проверил running config, коммутатор не настроен.    
• Сколько интерфейсов FastEthernet имеется на коммутаторе 2960?  
24  
•Сколько интерфейсов Gigabit Ethernet имеется на коммутаторе 2960?  
2  
• Каков диапазон значений, отображаемых в vty-линиях?  
0 – 15  
• Проверил startup config, файл отсутствует.  
• появляется это сообщение?  
Потому, что коммутатор не настроен, и файл конфигурации не был еще сохранен.  

• Изучил характеристики SVI для VLAN 1 (show interface vlan1)   
• Назначен ли IP-адрес сети VLAN 1?  
нет, не назначен   
interface Vlan1  
no ip address  

• Какой mac-адрес имеет SVI?  
Hardware is CPU Interface, address is 000c.cf06.774c (bia 000c.cf06.774c)  

• Данный интерфейс включен?  
Нет, выключен  
Vlan1 is administratively down, line protocol is down  

• Изучил IP-свойства интерфейса SVI сети VLAN 1.  
• какие выходные данные вы видите?  
Switch#show ip interface vlan1  
Vlan1 is administratively down, line protocol is down  
Internet protocol processing disabled  

• Подсоединил кабель Ethernet компьютера PC-A к порту 6 на коммутаторе и изучил IP-свойства интерфейса SVI сети VLAN 1  
Switch#show ip interface vlan1  
Vlan1 is administratively down, line protocol is down  
Internet protocol processing disabled  

• Под управлением какой версии ОС Cisco IOS работает коммутатор?
Version SW	Image  
------ ----- --	 ---------- ----------  
15.0(2)SE4 	C2960-LANBASEK9-M  

• Как называется файл образа системы?  
C2960-LANBASEK9-M  

• Какой базовый MAC-адрес назначен коммутатору?  
Base ethernet MAC Address : 00:0C:CF:06:77:4C  

• Изучил свойства по умолчанию интерфейса FastEthernet, который используется компьютером PC-A.  
• Интерфейс включен или выключен?  
Интерфейс включен  

• Что нужно сделать, чтобы включить интерфейс?  
Нет  

• Какой MAC-адрес у интерфейса? 
Hardware is Lance, address is 00d0.9779.d106 (bia 00d0.9779.d106)  

• Какие настройки скорости и дуплекса заданы в интерфейсе?  
Full-duplex, 100Mb/s  

• Изучил параметры сети VLAN по умолчанию на коммутаторе.  
• Какое имя присвоено сети VLAN 1 по умолчанию?  
default  

• Какие порты расположены в сети VLAN 1?  
Все
• Активна ли сеть VLAN 1?  
Да, активна  

• К какому типу сетей VLAN принадлежит VLAN по умолчанию?  
Enet  

• Изучил флеш-память.  
• Какое имя присвоено образу Cisco IOS?  
2960-lanbasek9-mz.150-2.SE4.bin  

#### Часть 3. Настройка базовых параметров сетевых устройств

•В режиме глобальной конфигурации настроил следующие базовые параметры конфигурации:  
•Запретил трансляцию имён DNS в маршрутизаторе  
Switch(config)#no ip domain-lookup  

• Присвоил имя коммутатору  
hostname S1  

• Установил пароль на привилегированный режим  
service password-encryption  
enable secret class  

• Настроил баннер  
banner motd #  
Unauthorized access is strictly prohibited. #  

• Назначил IP-адрес интерфейсу SVI на коммутаторе
S1(config)#interface vlan 1  
S1(config-if)#ip address 172.16.1.10 255.255.255.0  
S1(config-if)#no shutdown  

• Ограничил доступ через консольный порт с помощью пароля  
S1(config)#line console 0  
S1(config-line)#logging synchronous  
S1(config-line)#password class  
S1(config-line)#login  

• Настроил каналы виртуального соединения для удаленного управления (vty)  
S1(config)#line vty 0 15  
S1(config-line)#password class  
S1(config-line)#login  

• Настроил IP-адрес на компьютере PC-A

• [Конфигурация коммутатора](https://github.com/permakov/otus/blob/main/lab1/Config_S1.txt)

При просмотре методичке обратил внимание на то, что в конфигурации представленной в пункте а, первого шага третьей части на портах коммутатора настроен VLAN99, о котором речи выше не было. Добавил данный VLAN в свой конфиг и назначил его на порты
S1#configure t  
S1(config)#vlan 99  
S1(config-vlan)#exit   
S1(config)#interface range f0/1-24,g0/1-2  
S1(config-if-range)#switchport access vlan 99  
S1(config-if-range)#exit  

• Проверил параметры VLAN1  
• Какова полоса пропускания этого интерфейса?  
BW 100000 Kbit  
• В каком состоянии находится VLAN 99?  
Vlan99 is up  
• В каком состоянии находится канальный протокол?  
line protocol is up  

• Протестировал сквозное соединение, отправив эхо-запрос  
C:\> ping 172.16.1.10  

Pinging 172.16.1.10 with 32 bytes of data:  

Request timed out.  
Reply from 172.16.1.10: bytes=32 time<1ms TTL=255  
Reply from 172.16.1.10: bytes=32 time<1ms TTL=255  
Reply from 172.16.1.10: bytes=32 time<1ms TTL=255  

Ping statistics for 172.16.1.10:  
Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),  
Approximate round trip times in milli-seconds:  
Minimum = 0ms, Maximum = 0ms, Average = 0ms  

• Проверил удаленное управление коммутатором S1
• Сохранил конфигурацию коммутатора:  
S1#copy running-config startup-config  
Destination filename [startup-config]?  
Building configuration...  
[OK] Сохранил конфиг  

• Зачем необходимо настраивать пароль VTY для коммутатора?  
Для предотвращения не санкционированного доступа  
• Что нужно сделать, чтобы пароли не отправлялись в незашифрованном виде?  
Задать настройку service password-encryption



### Инициализация и перезагрузка коммутатора

• Проверил содержимое памяти  
S1#show flash  
Directory of flash:/  

1 -rw- 4670455 <no date> 2960-lanbasek9-mz.150-2.SE4.bin  
3 -rw- 1993 <no date> config.text  
2 -rw- 616 <no date> vlan.dat  

64016384 bytes total (59343320 bytes free)  

• Удалил файл vlan.dat  

S1#delete vlan.dat  
Delete filename [vlan.dat]?  
Delete flash:/vlan.dat? [confirm]y  

• Проверил содержимое памяти
  
S1#show flash   
Directory of flash:/  

1 -rw- 4670455 <no date> 2960-lanbasek9-mz.150-2.SE4.bin  
3 -rw- 1993 <no date> config.text  

64016384 bytes total (59343936 bytes free)  
  
• Удалил конфигурацию уоммутатора
  
S1#erase startup-config   
Erasing the nvram filesystem will remove all configuration files! Continue? [confirm]  
[OK]  
Erase of nvram: complete  
%SYS-7-NV_BLOCK_INIT: Initialized the geometry of nvram  
S1#



