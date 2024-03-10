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