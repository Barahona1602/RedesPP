# Redes punto a punto
## Integrantes Grupo 7
Nombre | Carnet |
|----------|----------|
| Pablo Josué Barahona Luncey | 202109715 |
| Ryan José Rodrigo Sigüenza Huertas | 202100101 |
| Joshua David Osorio Tally |202110773 |

## Tabla de subredes 
| Nombre VLAN | Numero Vlan | Salto | Network | Mask| Primera Asignable | Ultima Asignable | Broadcast |
| ----------- | ----------- | ------ | ------- | ------ | --------- | ------- | --------- | 
| VENT | 1 |  /25(128) | 192.168.47.0 |255.255.255.128/25 | 192.168.47.1 | 192.168.47.126 | 192.168.47.127 |
| INF | 2 | /26 (64) | 192.168.47.128 |255.255.255.192/26 | 192.168.47.129 | 192.168.47.190 | 192.168.47.191 | 
| RRHH | 3 | /27(32) | 192.168.47.192 | 255.255.255.224/27 | 192.168.47.193 | 192.168.47.222 | 192.168.47.223 | 
| CONT | 4 | /28(16) | 192.168.47.224 |  255.255.255.240/28 | 192.168.47.225 | 192.168.47.238 | 1922.168.47.239 | 


## Topologia 1
### Diseño de topología 

### Comandos
#### R1
  ```sh
    conf t	
    int fa0/0	
    ip address 10.7.2.2 255.255.255.252	
    no shutdown
    exit
    int fa1/0	
    ip address 10.7.1.1 255.255.255.248	
    glbp 7 ip 10.7.1.5
    glbp 7 priority 150 
    glbp 7 preempt
    no shutdown
    router rip
    version 2
    network 10.7.1.1
    network 10.7.2.2
    end
```
#### R2
```sh
    conf t	
    int fa1/0	
    ip address 10.7.3.2 255.255.255.252	
    no shutdown
    exit
    int f0/0	
    ip address 10.7.1.2 255.255.255.248	
    glbp 7 ip	10.7.1.5
    glbp 7 priority 100
    no shutdown
    router rip
    version 2
    network 10.7.1.2
    network 10.7.3.2
    end
```
#### R3	
 ```sh
    conf t
    int fa1/0	
    ip address 10.7.4.2 255.255.255.252	
    no shutdown
    exit
    int f0/0	
    ip address 10.7.1.3 255.255.255.248	
    standby 7 ip 10.7.1.6
    standby 7 priority 150
    standby 7 preempt
    no shutdown
    exit
    router ospf 1
    network 10.7.4.0 0.0.255.255 area 7
    network 10.7.1.0 0.0.255.255 area 7
    end
 ```
#### R4	
 ```sh
    conf t
    int fa1/0	
    ip address 10.7.5.2 255.255.255.252	
    no shutdown
    exit
    int f0/0	
    ip address 10.7.1.4 255.255.255.248
    standby 7 ip	10.7.1.6
    standby 7 priority 100
    no shutdown
    exit
    router ospf 1
    network 10.7.0.0 0.0.255.255 area 7
    network 10.7.1.0 0.0.255.255 area 7
    end
 ```

### Topologia No.2
### Diseño de topología 

### Comandos
#### ESW1 
 ```sh
    conf t
    int f1/4
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/0
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/1
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/2
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/3
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    exit
    vtp domain redes1gp7
    vtp mode server 
    vtp version 2
    vtp password redes1gp7
    end
    conf t
    vlan 10
    name RHUMANOS
    vlan 20
    name CONTABILIDAD
    vlan 30
    name VENTAS
    vlan 40 
    name INFORMATICA
    END
    conf t
    int vlan10
    ip address 192.168.47.193 255.255.255.224
    no shutdown
    int vlan20
    ip address 192.168.47.225 255.255.255.240	
    no shutdown
    int vlan30
    ip address 192.168.47.1 255.255.255.128
    no shutdown
    int vlan40
    ip address 192.168.47.129 255.255.255.192
    no shutdown
    exit 
    interface port-channel 1
    channel-group 1 mode active
    interface port-channel 2
    channel-group 2 mode active
    interface fa1/0
    speed 1000
    duplex full
    channel-group 1 mode on
    interface fa 1/1
    speed 1000
    duplex full
    channel-group 1 mode on
    interface GigabitEthernet 1/2
    speed 1000
    duplex full
    channel-group 2 mode on
    interface GigabitEthernet 1/3
    speed 1000
    duplex full
    channel-group 2 mode on
    exit
    exit
 ```
#### ESW2 
 ```sh
    conf t
    int f1/4
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/0
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/1
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/2
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/3
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/5
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/6
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/7
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/8
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    exit
    exit
    conf t
    vtp domain redes1gp7
    vtp mode client
    vtp version 2
    vtp password redes1gp7
    end
 ```
#### ESW3 
 ```sh
    conf t
    int f1/0
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/1
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/2
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/3
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/4
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/5
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/6
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/7
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/8
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    exit
    exit
    conf t
    vtp domain redes1gp7
    vtp mode client
    vtp version 2
    vtp password redes1gp7
    end
 ```
#### ESW4 
```sh
    conf t
    int f1/0
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/1
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/2
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/3
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/4
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/5
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    exit
    exit
    conf t
    vtp domain redes1gp7
    vtp mode client
    vtp version 2
    vtp password redes1gp7
    end
```
PC1
```sh
ip 192.168.47.195/26  192.168.47.193
```
PC2
```sh
ip 192.168.47.135/27 192.168.47.129
```
PC3
```sh
ip 192.168.47.10/25 192.168.47.1
```
PC4
```sh
ip 192.168.47.230/25 192.168.47.225
```
PC5
```sh
ip 192.168.47.11/25 192.168.47.1
```
PC6
```sh
ip 192.168.47.136/27 192.168.47.129
```

### Topologia 3
### Diseño de topología 

### Comandos
#### R6
 ```sh
    conf t	
    int f1/0
    ip address 10.7.5.1 255.255.255.252
    no shutdown 
    exit	
    int f0/0
    ip address 10.7.4.1 255.255.255.252
    no shutdown
    exit
    int f3/0
    no shutdown
    int f3/0.30	
    encapsulation dot1q 3
    ip address 192.168.57.126 255.255.255.128
    no shutdown
    int f3/0.40	
    encapsulation dot1q 4
    ip address 192.168.57.190 255.255.255.192	
    no shutdown
    int f3/0.10	
    encapsulation dot1q 1
    ip address 192.168.57.222 255.255.255.224
    no shutdown
    int f3/0.20
    encapsulation dot1q 2	
    ip address 192.168.57.238 255.255.255.240
    no shutdown
    exit
    router ospf 1
    network 192.168.57.0 0.0.0.255 area 7
    network 10.7.4.0 0.0.255.255 area 7
    network 10.7.5.0 0.0.255.255 area 7
    end
    wr
 ```

#### ESW5
 ```sh
    conf t
    int f1/0
    switchport mode trunk
    switchport trunk allowed vlan 1-2,1002-1005,10,20,30,40
    no shutdown
    int f1/1
    switchport mode access 
    switchport access vlan 10
    no shutdown
    int f1/2
    switchport mode access 
    switchport access vlan 40
    no shutdown
    int f1/3
    switchport mode access 
    switchport access vlan 30
    no shutdown
    int f1/4
    switchport mode access 
    switchport access vlan 20
    no shutdown
 ```





 