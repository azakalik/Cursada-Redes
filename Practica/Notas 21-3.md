# Acerca del dB
- Si el ej dice dB, asumimos que son dBm (decibeles-miliwatts). Si dice dBw es con watts.
- Explicacion de dBm: los decibeles son una **comparacion** de potencia. En el caso del dBm, esta unidad es una operacion matematica que denota una comparacion con 1 mW. De la misma forma, el dBW es una comparacion con 1W.
- Formulas: $Pot(mW) = 10^{(dBm/10)}$,  $Pot(W) = 10^{(dBW/10)}$

# Beto
## Problemas con RIP
- Limite de 15 saltos entre routers
- Las actualizaciones de rutas las tira en broadcast para todos lados.
- OSPF resuelve ambos problemas. Primero que no tiene limite de saltos, y segundo que se pueden definir areas para las cuales se mandan las actualizaciones de rutas. En vez de haber broadcast, cada router le avisa a su vecino acerca de las actualizaciones de rutas. Sin embargo esto genera un tiempo de convergencia para que se terminen de propagar estas actualizaciones por todos los routers de la red.
## Static routing
- Es cuando agregas manualmente algo a la tabla de ruteo, y le decis: a esta red anda por esta interfaz
- Se suele hacer bastante pero no escala bien, no puedo ir a agregar una fila a todos los routes table de todos mis routers cada vez que agrego una red
## RIP
```
Router(config)#router rip

Router(config-router)#network 192.168.0.0

Router(config-router)#network 10.0.0.0
```
Luego de ejecutar esos comandos en un router, el mismo se encarga automaticamente de encontrar donde esta cada red haciendo broadcast para todos lados. Lo que le estan diciendo esos comandos es que esas redes existen, y que las encuentre.
Queda algo asi en la config del router:
![](Pasted%20image%2020240321193946.png)
## Ruteo dinamico
- Cada router declara cuales son las redes que tiene conectadas (OSPF)
- Ejecuto los comandos
```
Router(config)#route ospf 1

Router(config-router)#network 192.168.0.0 0.0.0.255 area 1

Router(config-router)#network 10.0.0.0 0.0.0.255 area 1

Router(config-router)#exit
```
- Ospf pide que le pongamos un numero de ID, poner siempre 1
- El area es el comando que uno tiene para limitar los dominios de broadcast
- `route ospf 1` "activa ospf"
- `network ...` dice yo como router conozco esta red
- Si pongo areas distintas, esas redes no se pueden conectar entre si. Mis configuraciones solo se propagan para los area 1.
- OSPF pide que usemos wildcard en vez de mascaras. Para convertir tenemos que restarle 255 a las mascaras. La idea del wildcard es que representa cuantos hosts podes tener por subred.
- Ejecutando `show ip route`, vemos que el router aprende algunas redes de tipo O (OSPF)
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

  

10.0.0.0/8 is variably subnetted, 3 subnets, 2 masks

C 10.0.0.0/24 is directly connected, GigabitEthernet0/1

L 10.0.0.1/32 is directly connected, GigabitEthernet0/1

O 10.0.1.0/24 [110/2] via 10.0.0.2, 00:01:46, GigabitEthernet0/1

192.168.0.0/24 is variably subnetted, 2 subnets, 2 masks

C 192.168.0.0/24 is directly connected, GigabitEthernet0/0

L 192.168.0.254/32 is directly connected, GigabitEthernet0/0

O 192.168.1.0/24 [110/3] via 10.0.0.2, 00:00:11, GigabitEthernet0/1
```
- El comando `show ip protocols` nos muestra que estamos usando OSPF
```
Router#show ip protocols

Routing Protocol is "ospf 1"

Outgoing update filter list for all interfaces is not set

Incoming update filter list for all interfaces is not set

Router ID 192.168.0.254

Number of areas in this router is 1. 1 normal 0 stub 0 nssa

Maximum path: 4

Routing for Networks:

192.168.0.0 0.0.0.255 area 1

10.0.0.0 0.0.0.255 area 1

Routing Information Sources:

Gateway Distance Last Update

10.0.1.2 110 00:01:33

192.168.0.254 110 00:03:21

192.168.1.254 110 00:01:17

Distance: (default is 110)
```
- La distancia por default es 110. Para cambiarlas, hay que hacerlo a mano.
- El comando `show ip ospf neighbor` nos muestra los vecinos de nuestro router
```
Router#show ip ospf neighbor

Neighbor ID Pri State Dead Time Address Interface

10.0.1.2 1 FULL/BDR 00:00:38 10.0.0.2 GigabitEthernet0/1
```
- El OSPF me deja que todos los routers de un SA sepan llegar a todo el resto de los routers del mismo SA. Este es el objetivo del admin: lograr que todos mis routers se conozcan entre si de la manera mas eficiente posible.
- Ejemplo de ejecucion de un traceroute
```
C:\>tracert 192.168.1.2

Tracing route to 192.168.1.2 over a maximum of 30 hops:

1 0 ms 0 ms 0 ms 192.168.0.254

2 0 ms 0 ms 0 ms 10.0.0.2

3 0 ms 0 ms 0 ms 10.0.1.1

4 0 ms 0 ms 0 ms 192.168.1.2

Trace complete.
```
![](Pasted%20image%2020240321202833.png)
