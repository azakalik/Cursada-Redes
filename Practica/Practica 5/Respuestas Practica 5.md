# Ejercicio 1
![](Pasted%20image%2020240329174755.png)
**Configure las rutas estaticas en cada Router con el comando “ip route red mascara interfaz ”Ej.: ip route 192.168.1.0 255.255.255.0 GigabitEthernet0/1**
En el router 0, corremos los siguientes comandos en el modo config:
```
Router(config)#ip route 192.168.0.0 255.255.255.0 GigabitEthernet0/1

%Default route without gateway, if not a point-to-point interface, may impact performance

Router(config)#ip route 10.0.0.0 255.255.255.252 GigabitEthernet0/0

%Default route without gateway, if not a point-to-point interface, may impact performance
```
Corremos los comandos similares en el router 1.
Vemos que las cosas quedaron bien configuradas en cada router con el siguiente comando:
```
Router#show ip route

Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP

D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area

N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2

E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP

i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area

* - candidate default, U - per-user static route, o - ODR

P - periodic downloaded static route 

Gateway of last resort is not set

10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks

C 10.0.0.0/30 is directly connected, GigabitEthernet0/0

L 10.0.0.1/32 is directly connected, GigabitEthernet0/0

192.168.0.0/24 is variably subnetted, 2 subnets, 2 masks

C 192.168.0.0/24 is directly connected, GigabitEthernet0/1

L 192.168.0.254/32 is directly connected, GigabitEthernet0/1

Router#
```
Las rutas estaticas del router 1 quedan asi:
![](Pasted%20image%2020240329182613.png)
NOTA: no olvidarse de configurar los default gateway para todos los hosts
# Ejercicio 2
Las rutas estaticas de nuestro router nuevo quedan asi:
![](Pasted%20image%2020240329182928.png)
Notar que a medida que vamos agregando routers y redes, cada vez necesitamos mas rutas estaticas cuando elegimos configurar nuestra red de esta manera. Tambien se debe agregar la ruta del server nuevo en los otros dos routers.
Una vez que tengamos conexion entre el servidor y nuestros hosts (comprobar con `ping`), podemos abrir el navegador web de un host y poner la IP del servidor, para ver la pagina web que esta hosteada ahi.
# Ejercicio 3
Ejecuto los siguientes comandos en el router 0, y comandos similares en el router 1.
```
Router(config)#router rip

Router(config-router)#network 192.168.0.0

Router(config-router)#network 10.0.0.0
```
Simplemente con ejecutar los comandos de arriba (y configurar correctamente los GW de todos los hosts), podemos ver que todos los hosts ya se pueden comunicar
# Ejercicio 4
En todos los routers, ejecutamos esta serie de comandos para configurar todas las redes disponibles en OSPF asi se esparcen por gossip.
```
Router(config)#router ospf 1

Router(config-router)#network 172.16.0.0 0.0.0.3 area 1
```
Solo con configurar OSPF y el GW de cada host, hay conexion entre computadoras.
El ejercicio quedo asi:
![](Pasted%20image%2020240330173622.png)
# Ejercicio 5
Corremos los siguientes comandos en el router de la izquierda:
```
Router(config)#router ospf 1

Router(config-router)#network 192.168.1.0 0.0.0.255 area 1

Router(config-router)#network 172.16.0.0 0.0.0.3 area 0

Router(config-router)#exit
```
En el router del medio:
```
Router(config)#router ospf 1

Router(config-router)#network 172.16.0.0 0.0.0.3 area 0

Router(config-router)#network 172.16.0.4 0.0.0.3 area 0

Router(config-router)#exit
```
En el router de la derecha:
```
Router(config)#router ospf 1

Router(config-router)#network 172.16.0.4 0.0.0.3 area 0

Router(config-router)#network 192.168.2.0 0.0.0.255 area 2

Router(config-router)#exit
```
El diagrama queda asi:
![](Pasted%20image%2020240330180935.png)
## Algunos comandos
El comando `show ip protocols` nos muestra informacion sobre el protocolo de ruteo que se esta utilizando, junto a las redes conectadas a cada area
```
Router#show ip protocols

  

Routing Protocol is "ospf 1"

Outgoing update filter list for all interfaces is not set

Incoming update filter list for all interfaces is not set

Router ID 192.168.1.254

Number of areas in this router is 2. 2 normal 0 stub 0 nssa

Maximum path: 4

Routing for Networks:

192.168.1.0 0.0.0.255 area 1

172.16.0.0 0.0.0.3 area 0

Routing Information Sources:

Gateway Distance Last Update

172.16.0.5 110 00:05:05

192.168.1.254 110 00:06:26

192.168.2.254 110 00:04:39

Distance: (default is 11
```

El comando `show ip route` nos muestra las rutas disponibles
```
Router#show ip route

Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP

D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area

N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2

E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP

i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area

* - candidate default, U - per-user static route, o - ODR

P - periodic downloaded static route

Gateway of last resort is not set

172.16.0.0/16 is variably subnetted, 3 subnets, 2 masks

C 172.16.0.0/30 is directly connected, GigabitEthernet0/0

L 172.16.0.1/32 is directly connected, GigabitEthernet0/0

O 172.16.0.4/30 [110/2] via 172.16.0.2, 00:07:41, GigabitEthernet0/0

192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks

C 192.168.1.0/24 is directly connected, GigabitEthernet0/1

L 192.168.1.254/32 is directly connected, GigabitEthernet0/1

O IA 192.168.2.0/24 [110/3] via 172.16.0.2, 00:07:10, GigabitEthernet0/0
```

El comando `show ip ospf neighbors` nos muestra las redes vecinas (**neighbor**) y por que IP acceder a ellas (**address**)
```
Router#show ip ospf neighbor
Neighbor ID Pri State Dead Time Address Interface

172.16.0.5 1 FULL/BDR 00:00:36 172.16.0.2 GigabitEthernet0/0
```

El comando `show ip ospf database` nos muestra
```
Router#show ip ospf database

OSPF Router with ID (192.168.1.254) (Process ID 1)

Router Link States (Area 0)

Link ID ADV Router Age Seq# Checksum Link count

192.168.1.254 192.168.1.254 772 0x80000002 0x00aa5a 1

172.16.0.5 172.16.0.5 691 0x80000004 0x009c20 2

192.168.2.254 192.168.2.254 665 0x80000003 0x000fe8 1

Net Link States (Area 0)

Link ID ADV Router Age Seq# Checksum

172.16.0.1 192.168.1.254 772 0x80000001 0x00bfe3

172.16.0.6 192.168.2.254 691 0x80000001 0x00513c

Summary Net Link States (Area 0)

Link ID ADV Router Age Seq# Checksum

192.168.1.0 192.168.1.254 1125 0x80000001 0x00c9c0

192.168.2.0 192.168.2.254 656 0x80000001 0x00b7d0

Router Link States (Area 1)
Link ID ADV Router Age Seq# Checksum Link count

192.168.1.254 192.168.1.254 1130 0x80000002 0x00cf45 1

Summary Net Link States (Area 1)
Link ID ADV Router Age Seq# Checksum

172.16.0.0 192.168.1.254 1120 0x80000001 0x00ee4c

172.16.0.4 192.168.1.254 736 0x80000002 0x00ce66

192.168.2.0 192.168.1.254 650 0x80000003 0x00ceb6
```