Recomendacion de una pagina: networkacademy.io 
# Acerca del modem de casa
El equipo que nos dan tiene:
- modem
- router
- access point
- switch
- opcionales
	- firewall/antivirus
	- vpn
	- servidor NAS

## Modem
- Me conecta al ISP
- Convierte la señal de mi red local (digital) para el cable (analogico)

## Router
- Maneja el trafico entre la LAN y la WAN (internet)
- Hace NAT
- Puede tener embebido el servidor DHCP

## Access Point
- Provee conectividad inalambrica

# Packet Tracer
- Es una herramienta de Cisco
- Hay que registrarse con el mail del ITBA, porque tenemos el paquete de estudiantes
- En la pagina www.netacad.com/portal/learning tenemos algunos cursos basicos
## Notas
- Casi siempre usamos el cable que es negro liso
- El modelo de los routers u otros equipos casi nunca importa, podemos poner el PT y listo
- Cuando ponemos un router, viene con los puertos apagados. Hay que prenderlos manualmente despues de enchufarle un cablecito
- Todo lo que hagamos en la interfaz visual, se puede ver en una pseudo-linea de comandos. Tal vez en el parcial nos pueden decir que les pasemos los comandos (nos dicen que no usemos la interfaz visual)
- Esto de los comandos es porque originalmente los routers y switches no tenian una interfaz visual, habia que conectarse por ssh y tirar comandos especificos de cisco
- Estos dispositivos de red tambien traen la opcion de desactivar las conexiones remotas (cuando los requerimientos de seguridad son muy fuertes).
- Le tengo que asignar la IP manualmente a todo, PCs y hasta routers (donde se hace desde la seccion de INTERFACE)
- Para conectar entre routers uso el cable crossover (negro con rayitas)
- Cuando trabajemos con routers, recordar poner como default gateway a ese router en todas las computadoras que corresponda

# VLANs
- Es una forma de segmentar una red fisica en distintas redes logicas
- Funciona en capa 2, a nivel switch
- Esto sirve para achicar dominios de colision y problemas de seguridad
- Puedo eliminar y agregar VLANs segun sea necesario para adaptarme a cambios en la organizacion (sin tener que estar tocando cables). Esto me da buena escalabilidad
- Trunk mode: para poder etiquetar sobre el mismo puerto del switch distintas VLAN, y pasar multiples VLAN por el mismo enlace
- Para algunos casos conviene usar subredes y para otros casos conviene usar VLAN