
## Rozwiązanie
-Podział sieci 188.156.226.160/27 na dwie podsieci: przeznaczoną na obsługę sal i przeznaczoną na obsługę wi-fi
-Utworzenie serwerowni:
* serwer główny - 188.156.220.161/28
* serwer - brama sieci lan - 188.156.220.162/28
* serwer WI-FI - 188.156.220.163/28


-Utworzenie na każdym piętrze routera, do którego będzie kierowany ruch z każdego laboratorium, który następnie będzie kierowany do serwera-bramy
![diagram](diagram3.jpg)

## Konfiguracja

### Ustawienie statycznego ip
 ``iface eth0 inet static``
 
### Tablica trasowania

SALE: 

| network  | destination | gateway|
| ------------- | ------------- | -------------| 
| 10.0.9.0/26 | default  |10.0.0.0|
| 10.0.13.0/26  | default  |10.0.0.0|
| 10.0.9.0/26 | default  |10.0.0.0|
| 10.0.13.0/26  | default  |10.0.0.0|
| 10.0.0.0 | default |188.15.220.162/28 |
| 188.15.220.162/28 | default | 188.15.220.161/28 |

WI FI 

| network  | destination | gateway|
| ------------- | ------------- | -------------|
| 10.1.0.0/22| default| 10.0.0.0 |
| 10.2.0.0/22| default| 10.0.0.0 |
| 10.3.0.0/22| default| 10.0.0.0 |
| |10.0.0.0 | 188.156.220.178.0/28 |

### Umożliwienie forwardowania ip dla : Serwer główny, Brama główna, Serwer wi-fi 
``plik: /etc/sysctl.d/99-sysctl.conf   net.ipv4.ip_forward=1``



### Masquerade
#### Serwer główny
``sudo iptables -t nat -A POSTROUTING -s 188.156.220.160/28 -o enp0s3 -j MASQUERADE``  
``sudo iptables -t nat -A POSTROUTING -s 188.156.220.176/28 -o enp0s3 -j MASQUERADE``  
``sudo iptables-save | sudo tee /etc/iptables.sav``  


### Routing
``ip route add default via 10.0.0.0/28``

dla każdego komputera default route to router znajdujący się na odpowiadającym piętrze
