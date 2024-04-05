# Ejercicio 1
El comando `show vlan` nos da informacion con respecto a las VLAN existentes
# Ejercicio 2
Ejecutamos los siguientes comandos para crear la VLAN 2,  y hacer que la interfaz Fast Ethernet 1 tome esa VLAN
```
Switch(config)#vlan 2

Switch(config-vlan)#name presidencia

Switch(config-vlan)#exit

Switch(config)#interface Fa0/1

Switch(config-if)#switchport access vlan 2

Switch(config-if)#exit

Switch(config)#exit
```
NOTA: hay un MONTON de info que se puede ver solo haciendo hover por el switch (como el estado de cada interfaz y de cada VLAN)
![](Pasted%20image%2020240326151941.png)
- ¿Qué sucede si intenta hacer ping desde el Host 0 hacia el Host 1?
  Si intento esto, no va a funcionar, dado que estan en distintas VLAN

- ¿Qué sucede si intenta hacer ping desde el Host 0 hacia el switch? Teniendo el switch la IP 10.1.1.1
  Pareceria segun CPT que esto no funciona

- Cambiar a la vlan2 la interfaz Fa1/1 (hacia PC1), ¿Qué dos hosts pueden hacer ping entre ellos ahora
  Ahora la PC0 y PC1 pueden hacer ping entre si, dado que estan en la misma VLAN
# Ejercicio 3
Si todos los dispositivos estan en la misma VLAN, todos pueden hacer ping entre si
# Ejercicio 4
Vamos a hacer una red en donde PC0 y PC2 pertenezcan a VLAN1, PC1 y PC3 van a pertenecer a VLAN2.
Para esto sabemos que:
- PC0 esta conectado a la interfaz del switch Fa0/1
- PC1 a Fa0/2
- PC2 a Fa0/3
- PC3 a Fa0/4
Ejecutamos:
```
config t
interface Fa0/1
switchport access vlan 2
...
```
# Ejercicio 5
**¿Cómo puedo hacer una VLAN de 27 Hosts utilizando switches 2960?**
Cada switch 2960 soporta hasta 24 hosts. Para hacer una VLAN con 27 hosts, solo haria falta hacer stacking de estos switches.

NOTA: dentro del modo `config`, los comandos `interface vlan 1` y `vlan 1` NO son equivalentes. El primero nos permite asignarle una IP a la vlan, mientras que el segundo nos permite cambiar otros ajustes como ponerle un nombre.
Hacemos el siguiente diagrama de red:
![](Pasted%20image%2020240326155249.png)
## Parte A
**Utilizando la configuración por default de los switches. ¿Existe conectividad entre todos los host? (luego de establecer una IP y levantar las interfaces**
No existe conectividad entre PC3 y el resto de las PCs, dado que pertenecen a distintas VLAN
## Parte B
**Crear en el switch 0 una VLAN (de número 2 y de nombre “compras”) a la que pertenezcan los Host PC0 y PC1. Usar la subred 10.2.0.0/16 y establecer las IPs: Switch 0 10.2.1.1, PC0 10.2.1.2, PC1 10.2.1.3**
En el switch 0, ejecuto los comandos
```
conf t
vlan 1
name compras
exit
interface vlan 1
ip address 10.2.1.1 255.255.0.0
exit
interface Fa0/1
switchport access vlan 2
exit
interface Fa0/2
switchport access vlan 2
exit
exit
show running-config
```
## Parte C
**¿Con qué Host tiene visibilidad el Host PC3? ¿Por qué?**
El host PC3 no se puede comunicar con ningun otro host, dado que el puerto que comunica los switches (GigabitEthernet0/1) esta en **access mode** en vez de **trunk mode**.
## Partes D, E
**Configure en el Switch 0 para que el puerto Gig0/1 sea del tipo trunk, de la siguiente manera: Ingrese a modo configuración de la interface Gig0/1 y ejecute el comando `switchport mode trunk`**

El comando switchport mode trunk en un switch Cisco configura el puerto del switch en modo Trunk. Los puertos Trunk se utilizan para conectar switches entre sí y pueden transportar múltiples VLANs entre los switches. Los puertos Trunk deben recibir tramas etiquetadas con la asignación de VLAN de cada trama.

En otras palabras, un puerto configurado en modo Trunk puede llevar el tráfico de varias VLANs a través de un solo enlace físico. Esto es útil en situaciones donde tienes múltiples switches y necesitas llevar el tráfico de varias VLANs a través de ellos.

Es importante tener en cuenta que los puertos Trunk utilizan etiquetas (o “tags”) para identificar a qué VLAN pertenece cada trama de datos. Este proceso se conoce como "encapsulación 802.1Q".
## Parte F

# Ejercicio 6
preguntar este y el 5. yo creo que PC3 arranca en VLAN2 en el punto a, pero britu piensa que PC3 arranca en VLAN1
# Ejercicio 7
**Utilizando 2 switches 2960, se desea armar un LAN con 3 VLAN. Los Host pueden estar conectados a cualquiera de los 2 switches. Realice un ejemplo que cumpla con las condiciones anteriores.**
Creo las VLAN con el comando `vlan {numero}`, les asigno un nombre con `name {nombre}`
Le asigno a cada puerto la vlan que quiero con el comando `switchport access vlan {numero}` dentro de cada interfaz.
# Ejercicio 8
Existen 5 tipos de password en un equipo CISCO: El de CONSOLA, AUXILIAR, VTY (TELNET), ENABLE PASSWORD y ENABLE SECRET. Los últimos dos se pueden setear para impedir el acceso a los modos privilegiados de configuración y los primeros tres se utilizan para permitir el acceso a la configuración del equipo mediante la Consola, el puerto auxiliar o Telnet.
## Configurar acceso por Telnet
Lo primero que hacemos es crear una red con un switch 2960 (IP 192.168.0.1/24) y un host (IP 192.168.0.2/24)
Luego, configuramos la clave **itba** para acceso por telnet, ejecutando los siguientes comandos:
```
Switch#configure terminal

Enter configuration commands, one per line. End with CNTL/Z.

Switch(config)#line ?

<0-16> First Line number

console Primary terminal line

vty Virtual terminal

Switch(config)#line vty 0 15

Switch(config-line)#login

% Login disabled on line 1, until 'password' is set

...

% Login disabled on line 16, until 'password' is set

Switch(config-line)#password itba

Switch(config-line)#exit
```
Ejecutamos los siguientes comandos desde la terminal de nuestro host:
```
C:\>telnet 192.168.0.1

Trying 192.168.0.1 ...Open
User Access Verification
Password:

Switch>
```
Podemos ver que luego de ingresar la clave **itba**, ya accedimos a nuestro switch. Sin embargo, cuando intentamos ingresar al modo privilegiado, no nos deja y aparece el siguiente error:
```
Switch>enable

% No password set.
```
# Ejercicio 9
Tras ejecutar en el switch
```
Switch#conf t

Enter configuration commands, one per line. End with CNTL/Z.

Switch(config)#enable secret mipass
```
Podemos acceder desde el host de la siguiente forma
```
C:\>telnet 192.168.0.1

Trying 192.168.0.1 ...Open

User Access Verification

Password:

Switch>enable

Password:

Switch#
```
Ingresando primero la clave **itba** y luego **mipass**
# Ejercicio 10
