# Ejercicio 1
**Cual es la diferencia entre stacking de switches y STP**.
Se llama stacking al proceso de conectar N switches para que se comporten como uno solo. De esta manera, se puede tener una red con 40 hosts, utilizando dos switches de 24 hosts cada uno. Para hacer stacking, se utiliza un puerto especial que es propietary (tienen que ser todos de la misma marca).
STP, o spanning tree protocol, es un algoritmo por el cual se detectan loops al interconectar switches mediante stacking. Para este proceso, se elige uno de los switches como el principal (o el root) y se va armando una topologia de arbol, que termine con todos los switches conectados entre si. Las conexiones que no terminan apareciendo en el arbol, se quedan como conexiones de backup, que se prenden solo en caso de que falle otra de las conexiones.
Para elegir que conexiones se utilizaran activamente y cuales quedaran de backup, se tienen en cuenta una diversidad de factores como la velocidad de los cables y puertos de cada switch.

# Ejercicio 2
**Que provee un protocolo de WAN que no provea uno de LAN?**
Los protocolos de WAN nos permiten hacer que un host que esta en una red se comunique con otro host que esta en otra red, como si ambos estuvieran en la misma red LAN. Es decir, hace transparente el hecho de que ambos hosts pueden estar a muchos kilometros de distancia.
Lo que provee un protocolo WAN, es la encapsulacion de paquetes IP o paquetes Ethernet para que ellos puedan viajar largas distancias, y se desencapsulen al llegar al otro host.
Un protocolo de LAN, como ethernet, no provee esto. En el caso de Ethernet, la maxima distancia recomendada para un cable es de 100m, porque de lo contrario habra problemas en la deteccion de colisiones.

# Ejercicio 3
