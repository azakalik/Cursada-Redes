# Ejercicio 2
Luego de conectarse por **telnet** al sitio web, podemos acceder a la consola de un router.
Al ejecutar el comando `show ip bgp`, podemos ver la siguiente salida
```
route-views.wide.routeviews.org> show ip bgp
BGP table version is 0, local router ID is 202.249.2.166
Status codes: s suppressed, d damped, h history, * valid, > best, = multipath,
              i internal, r RIB-failure, S Stale, R Removed
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric LocPrf Weight Path
*  1.0.0.0/24       202.249.2.83                           0 2500 13335 i
*>                  202.249.2.169                          0 2497 13335 i
*                   202.249.2.169                          0 7500 2497 13335 i
*  1.0.4.0/22       202.249.2.83                           0 2500 7660 4635 7545 2764 38803 i
...
```
Lo que nos muerstra el comando, son todas las rutas aprendidas por `bgp`.
- La primera lineanos dice que para llegar a la red `1.0.0.0/24`, el "next-hop" es el IP `202.249.2.83` (que es el IP de un router obviamente), a la cual se puede llegar pasando primero por el AS 2500 y luego por el AS 13335.
- La segunda y tercer lineas son opciones adicionales para llegar a la red `1.0.0.0/24`
- Todos los path marcados con el simbolo `*` son validos
- Los path marcados con el simbolo `>` son el "mejor camino", de acuerdo con como se definen los costos
- La `i` detras de cada entrada, significa que la ruta se introdujo utilizando un IGP (interior gateway protocol)
# Ejercicio 3
Al menos en la version que se puede obtener por `apt` en `Ubuntu 22.04`, pareceria que `traceroute` no tiene la opcion `-A` disponible.
Utilizamos la alternativa `mrt --aslookup {dominio}`. En el caso de `www.google.com`, nos devuelve lo siguiente:
![](Pasted%20image%2020240404224613.png)
Podemos ver que pasamos por distintas IP en este orden:
- Localhost (zenbook)
- IP de router hogareño
- Una serie de IPs desconocidas -> routers internos de telecom
- Router borde del AS7303 (de telecom)
- Algunos routers dentro del AS15169 (de google)
Esto significa que nuestro ISP (telecom) esta conectado directamente a google, ya que no paso por ningun otro AS. El total de routers hasta llegar al host que queremos es de 9 (incluyendo el de hogar).
Al investigar el `AS15169` en  http://lacnic.net/cgi-bin/lacnic/whois?lg=EN, vemos que pertenece a **Google**, asi como mas informacion.
# Ejercicio 4
Ejecutando el comando `mtr -aslookup pampero.itba.edu.ar`, tenemos la siguiente salida
![](Pasted%20image%2020240404225417.png)
Podemos ver facilmente que dicho dominio pertenece al `AS10834`, el cual es propiedad de telefonica (esto lo averiguamos con ayuda de https://hackertarget.com/as-ip-lookup/)
# Ejercicio 5
Avisar en clase que el dominio route-views.oregon-ix.net no parece estar disponible