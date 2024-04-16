# Objetivos de clase
El objetivo de la clase es conectar dos computadoras a traves de una red LAN, y mediante una aplicacion de softphone, hacer una llamada entre dispositivos. En la red configuraremos un servidor asterisk.
Pasos
1. Un usuario realiza una llamada a traves de un dispositivo conectado a Asterisk
2. Asterisk recibe la llamada y la identifica mediante el canal utilizado
3. Asterik consulta la configuracion del sistema para determinar como enrutar la llamada
4. La llamada se enruta al destino seleccionado, que puede ser otro usuario, un buzon de voz, un IVR, etc.
5. La llamada se mantiene hasta que uno de los usuarios cuelga

# Algunas notas
PSTN: public switch telephone network. Es la red estandar de telefono que utiliza cobre tradicional (que ahora tmb usa fibra optica)
![](Pasted%20image%2020240411182734.png)

Todos los siguientes son protocolos telefonicos que hacen lo mismo pero de maneras distintas
- IAX: protocolo propietario de asterisk
- RTP / RTCP: otro protocolo para transmitir audio/video en tiempo real
- SRTP: secure RTP
- TTMF: sistema de señalizacion e interaccion con el IVR (interactive voice response)
- IVR: el robot que cuando llamamos a una empresa dice: "aprete 1 para ir a tal menu"
- H323: para entornos empresariales. Tiene un poco mas de seguridad/robustez que SIP. Menos compatibilidad que SIP
- SIP: el protocolo de telefono mas estandar

# Configuracion
- Dentro de la VM ubuntu, configuro la red en modo bridge para que pueda tener una IP propia dentro de la red de su casa
- Instalamos asterisk con `apt install asterisk`, en la clase instala la version 18.10
- `apt-cache search asterisk` nos muestra las librerias adicionales que podemos descargar para asterisk. Viene con un monton de audios de ejemplo, como `asterisk-core-sounds-en`.
- Instalamos los paquetes de clase con 
```
apt install asterisk-core-sounds-es asterisk-core-sounds-es-g722 asterisk-core-sounds-es-gsm asterisk-core-sounds-es-wav asterisk-prompt-es
```
- Vemos los paquetes que tengo instalados con `dpkg -l asterisk*`
- Hago `cd /usr/share/asterisk/sounds/`, en donde voy a tener todos los sonidos que instalamos
- Con `service asterisk status` vemos que el servicio esta corriendo. Vamos a ver qu ehay dos errores (rc_read_config). Es porque por defecto asterisk trata de acceder al archivo radiusclient.conf, que esta en otro directorio.
- Vamos a entrar a dos archivos de configuracion, y los vamos a arreglar para solucionar el error. Primero ejecutamos los siguientes comandos para hacer copias de seguridad
  ```
  cp /etc/asterisk/cel.conf /etc/asterisk/cel.conf.copia
  cp /etc/asterisk/cdr.conf /etc/asterisk/cdr.conf.copia
  ```
  - Ahora que tenemos las copias de seguridad, modificamos `cel.conf`. Vamos a descomentar la linea de la foto y ponemos la ruta que corresponde (`/etc/radiusclient`). Hacemos lo mismo con `cdr.conf`. **NOTA: en la practica hicimos esto, pero en Ubuntu Server tuve que cambiar el path por `/etc/radcli/radiusclient.conf` (ver mi TP7)**
  ![](Pasted%20image%2020240411184713.png)
  - Ahora ejecutamos `service asterisk restart` y vemos que solucionamos uno de los dos errores. El error restante no lo solucionamos porque no hace falta.
  - En `/etc/asterisk`, vamos a trabajar con `sip.conf` y `extensions.conf`. En el `sip.conf` vamos a configurar los usuarios y terminales ('beto' y 'kevin' serian dos entradas aca), y en `extensions.conf` ponemos como rutear cada usuario. El core de lo que vamos a trabajar va a ser `sip.conf`.
  - Si tenemos multiples dispositivos conectados con el numero de telefono 10, cuando se haga una llamada al 10 todos van a sonar.
  - Todo va por UDP
  - Nos queda como tarea averiguar que son estas cosas en el `sip.conf`
  ![](Pasted%20image%2020240411190410.png)
  - Borramos entero el archivo `sip.conf` y creamos los usuarios de la siguiente forma
  ```
  [usuario](!)
  type=friend ; llamadas entrantes y salientes
  host=dynamic ; cualquier IP en la red LAN
  context=itba ; esto lo usamos en el extensions.conf
  [ext10](usuario)
  username=kevin
  secret=itba123
  ;port=5061 (es el default, pero lo comentamos)
  [ext20](usuario)
  username=beto
  secret=itba123
  ```
- Definimos las extensiones `ext10` y `ext20`. Ambas pertenecen a la clase `usuario`, que esta definida arriba y dice que todos pueden hacer llamadas entrantes y salientes, y tambien le agregue el contexto `itba` para terminar de configurar en el `extensions.conf`
- El campo `type` puede ser `friend` (puede recibir llamadas entrantes/salientes), `user` (solo puedo recibir llamadas) o `peer` (solo puedo hacer llamadas)
- El username REAL es `ext10` o lo que sea, el campo `username` es un nombre de fantasia para que la UI quede mas linda. Si `kevin` renuncia y llega un empleado nuevo `martin` a su escritorio, simplemente se cambia ese nombre
- Ahora configuramos el `extensions.conf` de la siguiente forma
```
[itba]
exten => 10,1,Dial(SIP/ext10)
exten => 20,1,Dial(SIP/ext20)
```
- La sintaxis es: NRO_TELEFONO, PRIORIDAD, FUNCION
- La linea `[itba]` nos deja especificar el contexto
- En mi empresa, por ejemplo yo puedo tener el contexto "marketing", "ventas", "operaciones". Y estos no se pueden comunicar entre si.
- Por mas que este en distintos contextos, no puedo repetir los numeros de extension
- Ejecutamos `asterisk -rvvvv` mientras mas `v` tengamos, es mas verbose (4 es el maximo). Entramos en la CLI de asterisk
- Dentro de la CLI, ejecutamos `sip show users`, `sip show peers`. No tenemos nada porque no le dijimos a asterisk que haga un reload con los cambios nuevos que hicimos antes. `sip show friends` no existe, el comando para ver a los usuarios es peers.
- Ejecutamos `dialplan reload` para levantar los cambios
![](Pasted%20image%2020240411192446.png)
- El se bajo la aplicacion PORTSIP en el iphone, que es un softphone. La app pide configurar usuario, clave e IP del servidor SIP
- En nombre de usuario, pone `ext10`, clave `itba123` y IP de su VM.
- Una vez que toca login en la app, aparece el log en la terminal de la VM
- En otro telefono, abre la app y lo configura como `ext20`
- En el primer telefono, marca el numero `20`. Se puede ver que se produce el log en la terminal de la VM, y aparece la llamada en el segundo telefono.
- Esto puede viajar por TCP o UDP, aunque el default es UDP. En el archivo `sip.conf` esta esto ![](Pasted%20image%2020240411194304.png)
- En el `extensions.conf`, levantamos un numero adicional para que nos de un audio, nos redirija a una llamada y corte. Agregamos lo siguiente:
```
exten => 100,1,NoOp(Probando llamada con el numero 100) ; va a loguear ese texto
exten => 100,2,Playback(demo-congrats) ; reproducimos un audio de prueba
exten => 100,3,Dial(STP/ext10) ; redirigimos al usuario 10
exten => 100,4,Hangup() ; colgamos
```
- Estas funciones que usamos arriba estan en la documentacion de `asterisk`, en `docs.asterisk.org/Getting-Started/`
- Por default no esta encriptado, pero en `sip.conf` podriamos configurarlo para que asi sea. SIP es encriptado solo si va sobre TLS, y se llama SIPS
# Cosas que pueden pedir en el parcial
## Configurar un ambiente de trabajo
- Como hariamos una configuracion basica de un switch para que se conecte a internet
- Clases de IP, subredes (primer clase)
- Configurar un ambiente de trabajo en una oficina (pachera y eso)
- Como configurar la telefonia interna si tengo una oficina (configuro servidor, tirar cableado de los telefonos fijos al switch que se conectan al servidor, configurar el servidor, configurar los .conf)