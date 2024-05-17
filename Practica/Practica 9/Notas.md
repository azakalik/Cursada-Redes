# Distribucion de AWS
- AWS tiene regiones por todo el mundo
- El conjunto de regiones es lo que llamamos "la nube"
- Cada region es un conjunto de datacenters
- Cada region tiene como minimo 6 availability zones (AZ)
- Cada AZ puede ser uno o mas datacenters que estan relativamente cerca
- Las AZ de la misma region estan separadas por hasta 100km. Estas distancias pueden ser relativamente largas, para darle alta disponibilidad.
- Yo como usuario se en que region y AZ estoy, pero no puedo elegir ni saber en que datacenter especifico de esa AZ estoy
- Tambien existen las local zones. Son un mini datacenter en un pais, que funcionan como una especie de region con menos recursos y servicios disponibles, pero permite que algunos paises que no tienen una region ahi se aprovechen de poder hostear algunas cosas en su propio pais.
- Conectarse a los local zones trae algunas ventajas, como una baja de latencia, reduccion del trafico internacional (si es que nuestro pais no cuenta con una region), conectarse a la WAN de AWS, etc
- Ademas de los local zones, tambien hay "puntos de presencia", que son solo para hacer CDN
- AWS nos deja comprar Outposts, que son un rack de AWS y te lo instalan en tu propio datacenter. Entonces tendriamos "una nube" en nuestro datacenter. Esto medio que funcionaria como un "rack as a service", porque los de AWS le ponen un candado y se supone que nosotros no lo podemos ir tocando. Los outputs son usados por empresas que tienen que cumplir normas duras de donde guardan sus datos (como bancos).
# Diseño con buena Availability
- Si nosotros ponemos todos nuestros servicios en una AZ y esta se cae, vamos a estar en un problema
- La solucion es poner nuestros servicios de manera replicada en dos o mas AZ
- Si elegimos replicar nuestros servicios en distintas AZ, vamos a necesitar poner un load balancer que se encargue de llevar los distintos paquetes de nuestros clientes a las distintas AZ
- AWS nos obliga a que nuestros load balancers esten replicados en al menos dos AZ. La idea es que la consulta DNS de nuestro dominio (de nuestra pagina web por ejemplo) devuelva las IP de los load balancers que tenemos en cada region
- Cuando nosotros queremos replicar nuestra BD RDS en distintas AZ, tenemos que seleccionar una como main/master y las otras como standby/slave. Si es que master se llega a caer, AWS automaticamente usa su estrategia de failover para poner nuestra BD slave como master
- Tenemos que tener en cuenta que no es buena practica que tengamos un EC2 y una base de datos en la misma subnet. Esto se debe a que si un atacante logra infiltrarse en la EC2, podria obtener acceso a la RDS (sumandole un ACL). Teniendo esto en cuenta, hasta ahora tenemos 3 subredes en cada AZ:
	- Public subnet -> load balancer
	- Private App subnet -> EC2
	- Private DB subnet -> RDS
- Tengo que poner un IGW a nivel VPC
- Tengo que poner un NGW en cada AZ
- Recordar que el IGW es bidireccional (permite trafico entrante y saliente), mientras que la NGW solo permite trafico saliente
- Si una subnet tiene en su tabla de ruteo el IGW, entonces es publica. Podemos tener una tabla de ruteo por cada subred, o al menos hay que tener una para subredes publicas y otra para subredes privadas.
- Los security groups son una especie de firewall, para segurizar nuestros distintos servicios. Nos permite especificar que tipos de conexiones (IP-Puerto) aceptamos. Tambien podemos decir "permiti el trafico de el security group X a mi security group"
- Los SG tienen un ID, y puedo asignarle el mismo a distintos servicios. Por ejemplo mis dos instancias de EC2 seguro van a tener el mismo SG
- Las maquinas virtuales tienen distintas categorias (por ejemplo micro, xlarge, etc). Generalmente me conviene tener mas cantidad de maquinas menos potentes (medium por ejemplo) que una sola maquina muy potente (x-large por ejemplo). Esto nos beneficia en
	- Availability -> si se cae una maquina todavia tengo varias
	- Auto-scaling -> en los horarios de pico de demanda puedo tener menos maquinas prendidas
- En la nube podemos hacer auto-scaling horizontal (sumando y restando nodos). Pero NO se puede hacer auto-scaling vertical. Para cambiar las specs de un servicio (como agregarle vCPU) tengo que apagarla

# Grafico de clase

![](Pasted%20image%2020240509203557.png)