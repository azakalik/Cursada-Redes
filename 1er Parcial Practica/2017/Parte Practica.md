![](Pasted%20image%2020240423150718.png)
# Ejercicio 1
## 1
La notebook de hogar generara una interfaz virtual, con una IP que este dentro de la red laboral. Por ejemplo: 10.1.1.3.
## 2
Asi se ve antes la tabla de ruteo

| red           | mascara         | Interfaz | gateway       |
| ------------- | --------------- | -------- | ------------- |
| 0.0.0.0       | 0.0.0.0         | wlan0    | 192.168.1.254 |
| 192.168.1.0   | 255.255.255.0   | wlan0    | -             |
| 192.168.1.10  | 255.255.255.255 | wlan0    | -             |
| 192.168.1.254 | 255.255.255.255 | wlan0    | -             |

## 3
Asi se ve depsues

| red          | mascara         | ip    | gateway       |
| ------------ | --------------- | ----- | ------------- |
| 0.0.0.0      | 0.0.0.0         | wlan0 | 192.168.1.254 |
| 192.168.1.0  | 255.255.255.0   | wlan0 | -             |
| 192.168.1.10 | 255.255.255.255 | wlan0 | -             |
| 200.1.2.3    | 255.255.255.255 | wlan0 | 192.168.1.254 |
| 10.1.1.0     | 255.255.255.0   | virt0 | -             |
| 10.1.1.3     | 255.255.255.255 | virt0 | -             |

## 4 y 5
Tabla ruteo PC Laboral antes y despues

| red        | mascara         | ip       | gateway            |
| ---------- | --------------- | -------- | ------------------ |
| 0.0.0.0    | 0.0.0.0         |          | 10.1.1.254         |
| 10.1.1.0   | 255.255.255.0   | 10.1.1.1 | (interfaz virtual) |
| 10.1.1.254 | 255.255.255.255 | 10.1.1.1 | (interfaz virtual) |
| 10.1.1.1   | 255.255.255.255 | 10.1.1.1 | (interfaz virtual) |
# Ejercicio 3
![](Pasted%20image%2020240424183259.png)
![](Pasted%20image%2020240423151550.png)

| ID  | Descripcion                  | IP origen    | Puerto origen | IP Destino   | Puerto destino | Protocolo |
| --- | ---------------------------- | ------------ | ------------- | ------------ | -------------- | --------- |
| 1   | A se conecta con servidor    | 10.1.1.1     | 2828          | 200.1.2.3    | 7777           | TCP       |
| 2   | FWA le hace SNAT             | 188.99.88.77 | 2828          | 200.1.2.3    | 7777           | TCP       |
| 3   | B se conecta con el servidor | 10.1.1.1     | 3232          | 200.1.2.3    | 7777           | TCP       |
| 4   | FWB le hace SNAT             | 190.1.1.1    | 3232          | 200.1.2.3    | 7777           | TCP       |
| 5   | Servidor responde a FWA      | 200.1.2.3    | 7777          | 188.99.88.77 | 2828           | TCP       |
| 6   | FWA le hace DNAT             | 200.1.2.3    | 2828          | 10.1.1.1     | 2828           | TCP       |
| 7   | Servidor responde a FWB      | 200.1.2.3    | 7777          | 190.1.1.1    | 3232           | TCP       |
| 8   | FWB le hace DNAT             | 200.1.2.3    | 3232          | 10.1.1.1     | 3232           | TCP       |
| 9   | A se comunica con B          | 188.99.88.77 | 2829          | 190.1.1.1    | 3233           | UDP       |
| 10  | B se comunica con A          | 190.1.1.1    | 3233          | 188.99.88.77 | 2829           | UDP       |
## Pasos a seguir
1. A se conecta con el servidor. Envia un paquete TCP desde su IP privada (10.1.1.1) y el puerto que elige (2828 en este caso) hacia la IP y puerto destino.
   La idea es que A y B se conectaran al servidor en simultaneo para tomar decisiones respecto de como efectuaran el UDP hole punching.
2. Antes de que el paquete llegue al servidor, pasa por el firewall de A, quien reemplaza la IP privada (10.1.1.1) y pone una IP publica (188.99.88.77). Como estamos hablando de IP y PORT restricted, entonces intentara abrir la conexion con el mismo numero de puerto (que asumimos que no estaba en uso), entonces el paquete sale con puerto 2828.
   En este paso, FWA hace un "hole" para esa combinacion IP-puerto, por lo que el servidor podra contestarnos alli (siempre que el servidor mantenga su mismo IP-puerto).
3. Parecido a 1. B intenta conectar al servidor con IP 10.1.1.1 y puerto 3232
4. Parecido a 2. FWB le hace SNAT, manteniendo el puerto 3232 e intercambiando la IP privada con la IP 190.1.1.1. Se abre un "hole" en FWB, que permite que el IP-puerto del servidor nos conteste al puerto 3232 del FWB.
5. El servidor le responde el TCP SYN a FWA.
6. FWA recibe el paquete. Este puede pasar porque ya existia un hole en el FWA que nos lo permite. Le hace DNAT al servidor, permitiendo que A lo reciba.
7. Similar a 5
8. Similar a 6
9. A y B se habian puesto de acuerdo utilizando el servidor como intermediario para pactar como iban a hacer. Suponemos que ambos pactan que A utilizara el puerto 2829 y B el puerto 3233
   A envia un paquete UDP a B. FWB no lo deja pasar porque es IP y Port restricted, pero FWA abre un hole en el puerto 2829 para escuchar la respuesta de B.
10. B envia un paquete UDP a A. Como en el caso anterior se habia abierto un hole para habilitar que B lo contacte con su IP y puerto (3233), entonces lo deja pasar.
    En este punto la conexion ya esta establecida entre A y B!