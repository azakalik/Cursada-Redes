1) ¿Cuál es el rango de direcciones de una red Clase C?
   1-126
   
   
2) ¿Qué protocolo es utilizado para averiguar la dirección de hardware de un dispositivo local?
   ARP

   
3) ¿Qué clase provee un máximo de 254 host por red?
   Clase C


4) ¿Cuáles de los siguientes ítems utilizan ICMP?
   
	Los siguientes ítems utilizan ICMP:
	b. Ping d. Traceroute
	
	ICMP (Protocolo de Control de Mensajes de Internet) se utiliza principalmente para enviar mensajes de control y notificación de errores dentro de la red. El comando "ping" se utiliza para enviar paquetes de solicitud de eco ICMP a una dirección IP específica para verificar la conectividad de red. Traceroute también utiliza ICMP para rastrear la ruta de los paquetes a través de la red mediante el envío de paquetes de eco ICMP con incrementos en el campo de tiempo de vida (TTL) para obtener respuestas de los routers en la ruta.
	
	Los otros dos ítems, Telnet y ARP, no utilizan ICMP:
	
	a. Telnet es un protocolo de red utilizado para acceder y controlar dispositivos remotos a través de una red. Telnet utiliza un protocolo de terminal virtual y no está asociado con ICMP. c. ARP (Protocolo de Resolución de Direcciones) se utiliza para mapear direcciones IP a direcciones MAC en una red local. ARP opera en la capa de enlace de datos y no depende de ICMP para su funcionamiento.

5) ¿Cuáles de las siguientes frases son verdaderas?
   a. La dirección de broadcast de MAC son todos ceros. FALSO
   b. Las direcciones MAC están definidas en la capa de red. FALSO
   c. Las direcciones MAC son utilizadas por los switch para tomar decisiones de forwarding y filtering. FALSO
   d. Las direcciones MAC son un esquema jerárquico de direcciones. FALSO


   a. La dirección de broadcast de MAC son todos ceros.
   b. Las direcciones MAC están definidas en la capa de red.
   c. Las direcciones MAC son utilizadas por los switch para tomar decisiones de forwarding y filtering.
   d. Las direcciones MAC son un esquema jerárquico de direcciones.

	La frase que es verdadera es:
	c. Las direcciones MAC son utilizadas por los switch para tomar decisiones de forwarding y filtering.
	
	Explicación:
	a. La dirección de broadcast de MAC no son todos ceros, sino que está compuesta por todos los bits a 1.
	b. Las direcciones MAC no están definidas en la capa de red, sino en la capa de enlace de datos (capa 2) del modelo OSI.
	d. Las direcciones MAC no son un esquema jerárquico de direcciones, sino que son identificadores únicos asignados a dispositivos de red a nivel local.


6) El gateway de un host está representado por :
   a. La entrada 0.0.0.0 de la tabla de ruteo.
   b. La entrada 127.0.0.1 de la tabla de ruteo.
   c. La entrada 224.0.0.0 de la tabla de ruteo.
   d. La entrada 255.255.255.255 de la tabla de ruteo.

	La respuesta correcta es a.


7) ¿Si Ud. Necesita 12 sub redes a partir de una Clase C, qué máscara de subred debería usar?
   a. 255.255.255.252 -> 252 = 1111 1100
   b. 255.255.255.248 -> 248 = 1111 1000
   c. 255.255.255.240 -> 240 = 1111 0000
   d. 255.255.255.242 -> 242 = 1111 0010

	Si la red es de clase C, entonces tiene 8 bits para repartir entre subredes y hosts de cada subred. Salvo la d. que no es valida, cualquiera de las opciones permite armar 12 subredes. Sin embargo, la mejor opcion es la c. ya que nos permite que cada subred tenga el mayor numero de hosts.


8) ¿Cuál es la dirección de broadcast de 172.16.8.159/255.255.255.192?
   a. 172.16.255.255
   b. 172.16.8.127
   c. 172.16.8.191
   d. 172.16.8.255

	192 -> 1100 0000 -> la mascara de red es /26 -> los ultimos 6 bit deben estar en 1
	159 -> 1001 1111 -> si ponemos los ultimos 6 bits en 1 -> 1011 1111 -> 191
	Nos queda que la direccion broadcast es `172.16.8.191`, que es la opcion c.


9) Si Ud. necesita una red Clase B dividida en 510 sub redes. ¿Qué máscara de red debería usar?
   a. 255.255.255.252
   b. 255.255.255.128
   c. 255.255.255.0
   d. 255.255.255.192

	Una red Clase B tiene los primeros 16 bits fijos, entonces tengo 16 bits para repartir entre cantidad de subredes y cantidad de hosts por subred.
	Para llegar a 510, la potencia de 2 mas cercana es 512 = 2^9, entonces necesito los primeros 9 bits de esos 16.
	En conclusion, la mascara me tendria que dar 16+9 = 25 bits en 1 y el resto en 0, lo cual se traduce a que los ultimos 8 bits me quedan asi 1000 0000.
	Finalmente, la mascara queda `255.255.255.128`, que es la opcion b.


10) Marque las opciones verdaderas. Para determinar el enrutamiento de un paquete, el protocolo RIP:
	a. Analiza el estado del link al próximo router.
	b. Analiza el congestionamiento de los links.
	c. Analiza la cantidad de saltos al destino.
	d. Analiza el camino más corto.

	Las opciones verdaderas son:
	
	c. Analiza la cantidad de saltos al destino.
	
	Explicación: El Protocolo de Información de Enrutamiento (RIP) utiliza la métrica de "saltos" para determinar las rutas más óptimas. Cuenta los "saltos" (número de routers atravesados) para llegar a un destino y elige la ruta con la menor cantidad de saltos como la ruta preferida.
	
	Las otras opciones no son características del protocolo RIP:
	
	a. El RIP no analiza el estado del enlace al próximo router. Es un protocolo de vector de distancia que no mantiene un estado de enlace. b. El RIP no analiza el congestionamiento de los enlaces. No tiene en cuenta la congestión en sus cálculos de enrutamiento. d. El RIP no analiza el camino más corto en términos de distancia física o tiempo de viaje, sino que se basa en la cantidad de saltos para determinar la ruta preferida.


11) Ud. se encuentra trabajando en su casa con Arnet como ISP y quiere acceder a www.miempresa.com.ar con su navegador. Marque en orden (con números desde 1 en adelante) los ítems que participan en la resolución del nombre:
    a. Servidor de DNS de dominio miempresa.com.ar
    b. Servidor de DNS de dominio www.miempresa.com.ar
    c. NIC.AR
    d. Servidor de DNS de Arnet
    e. Root server
    f. Navegador
    g. Archivo Hosts

	COMPLETAR


12) Dado un canal de transmisión de datos coaxil con una atenuación a la frecuencia de operación de 0,9 dB/100 m y donde la sensibilidad del receptor es -15 dBm. Calcular la potencia mínima que deberá tener el transmisor si la longitud del coaxil es de 1200 m. Indicar cuál es la potencia mínima del transmisor en miliwatts.

	Datos:
	- Atenuacion: 0.9 dB/100 m
	- Sensibilidad del receptor: -15 dBm
	- Longitud del coaxil: 1200 m
	
	Solucion:
	- Asumo que la potencia del transmisor es de X dBm
	- Planteo la ecuacion $X - 1200 x (0.9 / 100) > -15 dBm$
	- Despejando, me queda que $X > (12 x 0.9) - 15$
	- Llegamos a que $X > -4.2 dBm$
	- Para convertir dBm a mW, utlizamos la formula:
	$$
	P[mW] = 1 [mW] \cdot 10^{(P[dBm]/10)}
	$$
	- Finalmente, nos queda $X = 0.38 [mW]$


13) En base al ejercicio anterior indicar la ganancia de un amplificador que se debería colocar a mitad de camino entre el transmisor y el receptor si se desea cambiar a este último por otro de sensibilidad 10 veces menor.

	Datos:
	- Hay un transmisor de $-4.2 [dBm] = 0.38 [mW]$.
	- Hay un receptor con una sensibilidad de $-15[dBm] = 0.0316[mW]$.
	- Si el receptor tiene una sensibilidad 10 veces menor, nos queda en $0.316[mW] = -5[dBm]$.
	- En el medio (600m) hay un amplificador de ganancia $X [dBm]$.
	
	Solucion:
	- Suponiendo que la ganancia es G, podemos plantear la ecuacion:
	  $-4.2 + G - 1200 \cdot (0.9 / 100) = -5$
	- Despejando, nos queda $G = 10[dBm]$


14) Calcular la potencia de salida de una línea de transmisión de 100 m donde la atenuación del cable coaxil es de 5 dB/100 m y la potencia del transmisor que excita a la línea es de 0 dBm, se pierde en conectores y empalmes 2dB.

	- Asumimos que hay 2 empalmes (uno en el transmisor y uno en la salida)
	- Planteamos la ecuacion $T - 2E - A = S$ con:
		- T (transmisor)
		- E (empalmes)
		- A (atenuacion)
		- S (salida)
	- Reemplazando: $S = 0 - 2 \cdot 2 - 5 = -9 [dBm]$


15) Dado un enlace de fibra óptica entre un emisor y un receptor con los siguientes parámetros:
	- Atenuación de la FO (F) = 1 dB/km
	- Atenuación del conector (C) = 0,6 dB
	- Potencia de transmisión (P) = 3 dBm
	- Sensibilidad del receptor (S) = - 10 dBm

	Calcular la distancia máxima (X) entre receptor y transmisor suponiendo un factor de diseño FD = 10 dB (margen de diseño M), empleándose un conector en el transmisor y otro en el receptor. Repetir el cálculo para una FO cuya atenuación es de 0,2 dB/km.

	Para ambos casos planteamos la ecuacion $P - 2C - F \cdot X = S + M$ (asumimos que hay 2 conectores)
	
	Caso 1:
	$3 - 1.2 - 1 \cdot X = -10 + 10$  -> $X = 1.8 [km]$
	
	Caso 2:
	$3 - 1.2 - 0.2 \cdot X = -10 + 10$ -> $X = 9[km]$


16) Si se tiene un enlace de 1000 m entre un transmisor que entrega una potencia de 100w y un receptor con una sensibilidad de 1w y se pretende utilizar las siguientes líneas de transmisión, indicar cuándo se deberá utilizar amplificadores.
    Considerar en ambos casos dos conectores de 0,5 dB.
    a. Usando coaxil fino RG 58 con At = 5 dB/100 m
    b. Usando coaxil grueso RG 218 con At = 0,8 dB/100 m

	A tener en cuenta:
	- Transmisor: $T = 100[W] = 20[dB]$
	- Sensibilidad: $S = 1[W] = 0[dB]$
	- Perdida por conectores: $C = 1[dB]$
	- Atenuacion total: $A$
	- Potencia que recibe el receptor: $R = T - C - A$
	
	Caso A:
	- Atenuacion total RG58: $A = 1000 \cdot (5 / 100) = 50[dB]$
	- Potencia que recibe el receptor $R = 20 - 1 - 50 = -31[dB]$
	- En este caso, se debe usar un amplificador de ganancia $G = 31[dB]$, ya que en otro caso nos queda $R < S$
	
	Caso B:
	- Atenuacion total RG218: $A = 1000 \cdot (0.8 / 100) = 8[dB]$
	- Potencia que recibe el receptor $R = 20 - 1 - 8 = 11[dB]$
	- En este caso, no es necesario usar un amplificador, ya que nos queda que $R > S$
	
	Notar para ambos casos que solo es necesario usar amplificador cuando el receptor recibe menos dB que el valor de su sensibilidad.
