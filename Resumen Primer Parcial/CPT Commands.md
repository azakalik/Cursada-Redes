# Switch
## Modos
- User
	- user -> privileged: `enable`
- Privileged
	- privileged -> user: `disable`
	- privileged -> configure: `configure terminal`
- Configure
	- configure -> privileged: `exit`
	- configure -> configure interface: `interface ...`
- Configure interface
	- configure interface -> configure: `exit`
## Comandos
- `show running-config` -> ver configuraciones generales
- `hostname {name}` -> dentro de config terminal para configurar el hostname
- `no shutdown` para prender una interfaz
- dar una ip dentro de una VLAN:
```
configure terminal
interface Vlan1
ip address 192.168.0.1 255.255.255.0
```