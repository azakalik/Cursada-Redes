# Ejercicio 1
## Bloque CIDR para DC
Consigna:
- Clase B -> mascara /16
- 8 subredes -> tomo 8 bits para subredes (aunque con 3 alcanzaria)
Defino la red 172.16.0.0/16, con las siguientes subredes:
- 172.16.0.0/24
- 172.16.1.0/24
- 172.16.2.0/24
- ...
- 172.16.7.0/24
## Por que dividir en subredes
Segmentando la red, se permite tener una subred de administracion de la red separada de la subred por la que le llegan pedidos al server.
## Bloque CIDR para CORP
Defino la red 192.168.0.0/16
# Ejercicio 3
Usted ingresa a trabajar a una empresa de servicios de nube multinacional, es el encargado de diseñar la conexión entre la ciudad de las Toninas ARG hasta Río de Janeiro BRA mediante cable submarino.
Se sabe que:
- La distancia lineal entre las ciudades de manera lineal es de 1934 km, y se tomará 2000km para los cálculos.
- Se dispone de tecnología de fibra óptica monomodo de atenuación de 0,5 db/km La sensibilidad de los repetidores es de 10dBm
Se pide:
- Calcule la cantidad de repetidores a colocar
- Dibuje un gráfico donde se vea la ubicación de los repetidores en los 2000km

Resolvemos:
- 0.5 dBm por km, entonces la atenuacion total es de 0.5 * 2000 = 1000 dBm.
- Se necesitaran 100 repetidores, colocados cada 20km cada uno