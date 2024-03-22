# Notas
- Se puede usar TAB para el autocompletado en todas las CLI
## Modos
![](Pasted%20image%2020240319151208.png)
## Comandos en CLI del switch
- Version del switch: `show version`
- Opciones del comando show: `show ?`
- Ingresar en modo privilege: `enable`
- Ver configuraciones de las interfaces: `show running-config`
- Establecer hostname del switch
  ```
	enable
	configure terminal
	hostname SW_Piso2
	```

# Ejercicio 3
- Para el ejercicio se uso el switch 2960, que es de tipo 3 (por lo que tiene VLAN)
- Primero, el comando `enable` nos pasa de **user mode** a **privileged mode**.
- Luego, el comando `configure terminal` nos pasa de **privilege mode** a **configure mode**.
- Despues, el comando `hostname {name}` le asigna el nombre `{name}` al switch.
- Corremos `exit` para volver a **privileged mode**, y ejecutamos `show running-config` para ver las configuraciones actuales.
- Corriendo `show ip interface` en **user mode** o **privileged mode**, podemos ver la **IP** actual del switch.
- En el modo configuracion, ejecutamos `interface Vlan1` para entrar a la configuracion de **VLAN1**, y con `ip address 10.1.1.1 255.255.0.0` le asignamos una IP para esa VLAN.
- Ejecutando nuevamente `show running-config` vemos: ![](Pasted%20image%2020240319153811.png)
- Finalmente, habilitamos la VLAN con el comando `no shutdown`.
- NOTA: no pareceria que se pueda asignarle una IP a alguna VLAN del switch desde GUI, parece que es solo por CLI.
# Ejercicio 4
Aprendemos que `telnet` viene desactivado por default en el switch, hay que activarlo manualmente
# Ejercicio 6
Aprendemos que ingresando `?` en la terminal, podemos ver los comandos posibles del modo en el que estemos
Ademas, el comando `description` nos sirve para darle una descripcion a un puerto de un switch, como se ve abajo
```
Switch(config)#interface FastEthernet0/1

Switch(config-if)#description File_Server

Switch(config-if)#exit

Switch(config)#exit

Switch#show running-config
...

interface FastEthernet0/1

description File_Server
```
# Ejercicio 8
El comando `write erase` nos deja borrar la NVRAM de un switch, que es donde se guardan todas las configuraciones.
El comando `reload` nos permite reiniciar un switch.