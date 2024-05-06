# Ataque sobre la red wifi
- Tenia kali instalado en una Raspberry PI
- El ataque se llama Aircrack, y no funciona para redes 5GHz ni para WPA3 (solo WPA1 o WPA2)
- El va a atacar su propia red kevin2.4
- Adentro de aircrack-ng (suite opensource) tenemos
	- airmon-ng
	- aireplay-ng
	- airodump-ng
	- airdecap-ng
- Lo que hace aircrack es inyectar paquetes, expulsar gente de la red y recuperar claves WEP y WPA1/2
# Brute forcing con rockyou
- Abrir Kali
- Abrir la terminal
- `ifconfig` -> veo cual es mi interfaz. Veo que tengo dos conexiones
	- Una wifi (`wlan0`)
	- Una cableada (`eth0`)
- Para hacer el ataque tenemos que estar conectados a wifi, entonces vamos a trabajar sobre la interfaz `wlan0`
- Vamos a ejecutar `airmon-ng start wlan0`. Este comando nos va a habilitar para monitorear la red
- Si ejecutamos `ifconfig` de vuelta vemos que la interfaz `wlan0` se intercambio por la interfaz `wlan0mon`. Lo que hace es una especie de proxy que pasa todos los paquetes por esa interfaz, poniendo la interfaz en modo monitor
- Ejecutamos `airodump-ng wlan0mon`. Esto va a escanear y capturar todos los paquetes de la red, y me va a mostrar un monton de informacion (como BSSID, PWR, Beacons, CH)
	- BSSID: basic service set identifier -> direccion MAC del AP de wifi
	- ESSID: -> el nombre de la red wifi
	- CH: canal de frecuencia por el que opera la red wifi
	- PWR: habla de la intensidad de la señal
	- Beacon: mientras mas alto sea el numero, mas estabilidad/confiabilidad tiene nuestra red
![](Pasted%20image%2020240502184007.png)
- Ejecutamos `airodump-ng -c 11 --bssid {MAC} -w {FILENAME} wlan0mon`, forzando a que use el canal y bssid que yo quiero. Escribira el resultado al archivo FILENAME
![](Pasted%20image%2020240502184116.png)
- Cada uno de estos representa un dispositivo conectado a esa red, en donde uno es el default gateway
- Cuando conecto un dispositivo a la red, aparece arriba un WPA HANDSHAKE, que coincide con la MAC address del route
![](Pasted%20image%2020240502184515.png)
- El handshake es un proceso de autenticacion que ocurre cuando un dispositivo se conecta a una red WPA/WPA2 (no funciona para WPA3)
	- Primero se hace una solicitud de asociacion (association request)
	- Luego se hace un association response, en donde me devuelve un token con una clave
	- Luego yo envio un mensaje de autenticacion (EAPOL), y el router me confirma la conexion
![](Pasted%20image%2020240502184402.png)
- El objetivo del atacante es obtener la llave que aparece en el mensaje 3
- Siempre que tratemos de acceder al AP, tengo conexiones mas fuertes que otras (por ejemplo, en un edificio me llegan un monton de redes). El overlapping de las redes wifi es un problema, porque el celular va a intentar siempre conectarme a la red wifi con mas señal. Las redes wifi implementan `intelligent roaming`. El AP esta constantemente monitoreando la potencia de la señal recibida de los dispositivos conectados. Si el AP detecta que el dispositivo esta en una ubicacion que tiene señal baja (y puede conectarse a una red mas fuente), mi AP me envia un `deauthentication frame`, expulsandome de la red.
- Ejecuto el comando `aireplay-ng -0 9 -c {MAC_DISPOSITIVO_A_DESCONECTAR} -a {MAC_AP} wlan0mon` para expulsar al dispositivo. Esto es facil de hacer porque no hace falta autenticacion para mandar este tipo de mensajes.
- En este punto, el celular va a intentar reconectarse a la red, y yo voy a poder obtener el WPA handshake. Las dos formas de obtener el WPA handshake es haciendo esto o esperar a que se conecte un dispositivo nuevo.
- Si hacemos `ls`, podemos ver que se generaron un monton de archivos .cap
- `rockyou` es una recopilacion de las claves mas vulnerables con la que nos hayamos encontrado
- Ejecutamos `aircrack-ng -b {WPA_HANDSHAKE} -w ./rockyou.txt ./{NAME}.cap`. El .cap son los paquetes que encontre de hacer el sniffing previamente.
- En este punto, va a intentar hacer fuerza bruta hasta que encuentre la clave
![](Pasted%20image%2020240502190327.png)
- Recapitulacion
	- Poner en modo monitoreo nuestra interfaz wifi
	- Hicimos un sniffing por todas las redes que tenemos cerca
	- Elegimos una red, identificamos los clientes conectados y el BSSID
	- Forzamos la desconexion de la MAC de un cliente para la MAC de una red
	- El dispositivo se reconecta a la red
	- El envio de paquetes en la reconexion, me lo guarde en el archivo `.cap`
	- Repetimos esto hasta que la herramienta devuelva el WPA handshake
	- Me hago pasar por ese dispositivo, y le mando millones de claves que tenia guardadas en un archivo hasta que encuentre la correcta
	- El WPA Handshake lo necesito para empezar a probar contraseñas
	- La idea es que yo no le pego al AP preguntando por las passwords, sino que empiezo a probar claves hasta tener el mismo hash que el del WPA Handshake
- Link con info util:
![](Pasted%20image%2020240502191410.png)

# Man In The Middle
- Yo tengo una conexion entre un usuario y una aplicacion web
- Un atacante que esta haciendo MITM se pone en el medio (haciendose pasar por la aplicacion web), exponiendo un servidor interno con una interfaz web parecida, para robar contraseñas
- Vamos a utilizar con ettercap:
	- ARP Spoofing: es una tabla que mapea direcciones logicas (IP) a direcciones fisicas (MAC). El atacante quiere hacerme pasar por otra MAC, haciendole creer al atacado que una direccion IP es una MAC que no corresponde
	- DNS Poisoning: infectar la tabla DNS que tiene cacheada en la computadora la persona atacada (en `/etc/hosts`), haciendole pasar un dominio conocido por una IP falsa (en este caso la IP del atacante)
## Pasos
- El esta en una LAN con dos dispositivos
- Abre `social engineering toolkit`
- Vamos a elegir la opcion 1, que se llama `Spear-Phishing Attack Vectors`
- Ahora elegimos La opcion 2 `site-cloner` para clonar campus
- Le vamos a decir que la direccion IP sea la del atacante, y le pasamos la URL de campus
- Desde la computadora del atacado, hacemos login en campus
- En la terminal del atacante, podemos ver que obtuvimos una contraseña
- La compu del atacado es redirigido a la pagina de campus real
- Entramos al archivo `etter.dns`. Aca podemos mapear los dominios que querramos a nuestra IP (como google y el de campus)
- Abro la app de Ettercap. Voy a listar los dispositivos conectados a la red (por IP)
![](Pasted%20image%2020240502192752.png)
- Pongo el router como el target 1 (en targets -> select target)
- Pongo el dispositivo del atacado como target 2
- Accedo a la opcion de MITM
- Habilito para hacer ARP poisoning
- Ahora empiezo a hacer sniffing