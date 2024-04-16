# Instalacion Asterisk
- Trate de hacer la guia con docker en wsl, pero es un quilombo abrir los puertos, entocnes me instale una VM de ubuntu server y la puse en modo bridge
- Corremos `systemctl status asterisk`. Al igual que en la practica, podemos ver que tiene errores
![](Pasted%20image%2020240413110615.png)
- Vamos a solucionar el error ejecutando `sudo vim /etc/asterisk/{file}`, con `{file}` siendo `cel.conf` y `cdr.conf`, y intercambiando la linea
```
radiuscfg => /usr/radiuclient.conf
```
por 
```
radiuscfg => /etc/radcli/radiuclient.conf
```
- Finalmente corremos `systemctl restart asterisk`
- El comando `systemctl enable asterisk` nos sirve para habilitar que dicho servicio se inicie solo cada vez que booteamos el sistema (en mi caso un docker)
- El comando `apt-cache search asterisk` nos deja ver todos los plugins que se pueden descargar en asterisk
- Nosotros vamos a bajar los siguientes:
```sh
apt-get install asterisk-prompt-es asterisk-core-sounds-es asterisk-core-sounds-es-gsm asterisk-core-sounds-es-wav asterisk-core-sounds-es-g722
```
- Comprobamos que se bajaron los archivos con `dpkg -l aterisk*`
- Los archivos de configuracion que vamos a usar se encuentran en `/etc/asterisk`, y se llaman `sip.conf` y `extensions.conf`.
- Los archivos de sonido se encuentran en `/usr/share/asterisk/sounds`
- Bajar el programa `Zoiper` para el celu y para la compu, la mayoria de los programas en la windows store no sirven para nada

# Uso Asterisk
- Ejecutamos `asterisk -rvvvv`, se nos abre el CLI de asterisk
- Ejecutando `netstat -nlp`, podemos ver que asterisk abrio un puerto (el puerto default es 5060):
```
root@df35ab25d07f:/# netstat -nlp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.1:5038          0.0.0.0:*               LISTEN      -
udp        0      0 0.0.0.0:4569            0.0.0.0:*                           -
udp        0      0 0.0.0.0:5060            0.0.0.0:*                           -
```
- `sip show users` nos muestra que todavia no tenemos usuarios
- `sip show peers` nos muestra que todavia no hay nadie conectado
# Ejercicio 1
- En el archivo `sip.conf`, ponemos lo siguiente
```
[usuario](!)
type=friend
host=dynamic
context=itba

[ext10](usuario)
username=User1
secret=user1

[ext20](usuario)
username=User2
secret=user2

[ext30](usuario)
username=User3
secret=user3
type=user  
```
- En el archivo `extensions.conf`, ponemos lo siguiente
```
[itba]
exten => 10,1,Dial(SIP/ext10)
exten => 20,1,Dial(SIP/ext20)
exten => 30,1,Dial(SIP/ext30) 
```
- Ejecutamos `systemctl restart asterisk` para recargar los archivos
- Ejecutamos `asterisk -rvvvv`
- Desde `zoiper desktop`, ponemos el usuario `ext10` y la clave `user1`. Podemos ver que el login se loguea en `asterisk CLI`.
- En `zoiper android` nos registramos con el usuario `ext20` y la clave `user2`. Podemos corroborar que las llamadas ya funcionan
# Ejercicio 2
- En wireshark, ponemos el filtrado por `sip || rtp`
- Podemos ver estos paquetes cuando nos registramos con el usuario `ext20`
![](Pasted%20image%2020240413134126.png)
- Podemos ver estos paquetes cuando hacemos una llamada del `ext10` al `ext20` y cortamos desde `ext10`
![](Pasted%20image%2020240413134322.png)
- Los logs del `asterisk CLI` nos muestran algo como esto
![](Pasted%20image%2020240413134454.png)
