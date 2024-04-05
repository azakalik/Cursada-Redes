## Access vs trunk port
Con modo access, yo configuro la boca del switch para que trabaje en una VLAN especifica. En este modo, cuando recibo un paquete sin etiquetar lo taggeo y destagueo automaticamente.

Con modo trunk, yo puedo manejar multiples VLAN en la misma boca del switch (por ejemplo, si quiero conectar un swich a un switch central). En el modo trunk, yo elijo una SERIE de VLANs que estan habilitadas (por ejemplo las VLAN 1,2,3,4).
Por ejemplo, si yo tengo un host que es un servidor, tal vez quiero que pertenezca a multiples VLAN, y le asigno un puerto trunk.
Si no especifico una serie de VLANs, pueden pasar todas (aunque hay algunos switches que no).

Las VLAN etiquetadas por los hosts no son comunes, porque el host no tiene por que saber en que VLAN esta (salvo tal vez en servidores).

## Acerca de OSPF

![](Pasted%20image%2020240404182658.png)
- Todos los routers que pertenecen a area 1, van a recibir solo novedades por broadcast de las redes que estan en area 1.
- El resultado final es que todos los routers saben como llegar a todos los routers