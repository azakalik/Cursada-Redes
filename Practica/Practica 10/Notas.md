- Arrancamos construyendo una VPC
- Hacemos el clasico dibujo de una aplicacion web con alta disponibilidad
![](Pasted%20image%2020240516181441.png)
- Una subnet publica y una privada para cada una de las dos AZ
## Creacion de VPC
![](Pasted%20image%2020240516181858.png)
- Tenancy: si queremos que se deployen nuestrar cosas en hardware dedicado
- Tenemos un numerito que nos deja elegir en cuantas AZ queremos que se cree nuestra VPC
- Para cada AZ, nos deja elegir numero de public y private subnet y sus respectivos CIDR
- Las subnets 1a, 1b, etc dependen del contexto de cada cuenta. Mi 1a no es el mismo que el 1a de otra empresa. Recordar que nosotros no podemos elegir en que AZ fisico estamos, solo podemos elegir la region (ej: us-east-1)
- Tambien nos deja hacer deployment de NAT gateways, uno solo o uno por AZ
- Es buena practica hacer que las subnets compartan la misma tabla de ruteo cuando sea posible. Siempre se tiene que tratar de crear la minima cantidad posible de objetos
- El unico que tiene relacion con el IGW es el public routing table (el de las public subnets)
## Levantamos una EC2
- Uno de los primeros servicios que existio, junto a VPC, S3 y SNS
- Aca tambien tenemos un wizard para levantar servidores
- Nos pregunta si queremos hacerlo publico o privado
- El AMI es el formato de imagenes en AWS. Son las imagenes que ellos ya tienen listas para que podamos usarla.
- La mayoria de las empresas (como OpenVPN) intenta tener su AMI. Esto es para que la gente lo pueda usar mas facil.
- La mas liviana de todas es Amazon Linux
- Podemos elegir entre usar x86 y ARM. Lo malo de ARM es que hay que recompilar todo para que funcione ahi, pero lo bueno es que es mucho mas costo-efectivo.
- Lo unico que nos pide elegir es el AMI a usar y el tamaño de la instancia (micro, medium, etc.)
- Hay familias de instancias, y cada familia tiene un proposito distinto. Las m son para prod, las t son para desarrollo, etc. No hace falta aprenderselo.
  Hay algunas optimizadas para computo (mas cpu que ram), optimizadas para memoria (mas ram que cpu)
  - En la clase levantamos una t3.micro. El 3 es el numero de generacion (generalmente actualizan procesadores, etc)
  - No existen las migraciones automaticas de generacion. Si yo tengo un cluster en t3 y sale el t4, tengo que migrar todo a mano uno por uno
  - Despues nos deja elegir un Key-Pair, que es lo que usamos para hacer login
    ![](Pasted%20image%2020240516183745.png)
- Cuando creas el Key-Pair te baja el `.pem` que es la autenticacion por certificado que vamos a usar para conectarnos por SSH
- Tambien le podemos decir que cree automaticamente el SG
- Le decimos que solo permita trafico SSH de una IP particular
- Para storage, le damos un disco gp3 de 8gb
- Si tocamos `terminate`, borramos para siempre la intancia
- Tambien puedo elegir si le asigno automaticamente una IP publica o no
- A la cantidad de IP que tenemos en nuestro bloque CIDR, hay que restarle 4 porque las usa AWS
- El usuario por default en Amazon Linux para conectarnos por ssh se llama `ec2-user`. En Ubuntu es `ubuntu`.
- Instalamos `apache`, lo levantamos, con `lsof` nos fijamos que este escuchando en el 80, creamos un `index.html` basico. Con esto ya tendriamos una pagina estatica
- Ahora abrimos el puerto 80 para inbound editando el SG
- En este punto ya podemos entrar desde un navegador
- En el mundo de cloud, ya no se referencian las cosas tanto por IP, sino que se referencia con el ID (esto es mas facil de hacer con IaaC)
## Creamos Load Balancer
- Nos deja elegir si es Internet-Facing o Internal
- Elegimos a que subnets en que AZ apunta el LB
- El load balancer va a levantar una IP en cada subnet que le asigne (yo no veo esa IP pero existe)
- Elegimos el SG que queremos. Por ahora dejamos el default.
- Ponemos un listener en el puerto 80 para HTTP. Para la default action, ponemos create target group y elegimos que sea en base a instancias
![](Pasted%20image%2020240516190110.png)
- Despues decimos donde hace el health check. Por ahora ponemos el path `/` porque ese path devuelve una pagina estatica en nuestro `nginx`
- Despues asignamos ese target group al listener
- Recordar que el SG de los EC2 solo permitia conexiones de la IP publica de beto. Ahora que agregamos un LB, tenemos que modificar esto.
- Creamos un SG para el LB. Le ponemos que permita conexiones de la IP publica de beto
- Vamos al SG de las EC2. Le vamos a permitir conexiones HTTP desde el SG de nuestro LB. Notar como es posible configurar las rules de un SG para aceptar tales conexiones desde otro SG.
- Despues averiguamos la URL del load balancer, la ponemos en el navegador, y vemos que todo funciona (si vamos reiniciando vemos que nos va mandando a nuestros distintos webservers)
## S3
- Hay carpetas, pero es una cuestion visual / para agrupar. No existe el concepto de filesystem.
- Cada objeto que guardo en un bucket S3 solo lo puedo acceder si conozco la URL del mismo
- S3 es extremadamente durable. Existe la probabilidad de que pierda un archivo cada 10mil años, por la manera de hacer replicacion que tiene. Por este motivo, no hace falta hacerle backup
- No solo se usa para tirarle objetos, se usa bastante para servir websites estaticos
- Es una manera muy simple de hostear un website estatico sin preocuparse por un web server
- Tocamos create bucket -> general purpose
- Si yo habilito ACL, entonces puedo hacer que un espacio en un bucket solo pueda ser controlado por un servicio, y ni siquiera un admin pueda entrar y modificar algo
- Tenemos tambien opcion de versioning
- Podemos elegir de que manera se hace el server side encryption. Nosotros elegimos hacerlo con llaves de S3. Si fueramos una empresa grande, podemos definir nuestra propia llave de encripcion (para usar la misma que en nuestro datacenter on-premise por ejemplo). Esto ultimo sirve como garantia de que S3 no puede ver lo que tenemos guardado.
- En el ejemplo de clase, subimos un .html y un .css. Al entrar del navegador, vemos una pagina web simple.
- En properties, podemos habilitar la opcion de website hosting. Aca nos deja elegir el index document, redirection rules (simples), etc
- Todas las politicas de seguridad funcionan como un ACL y se escriben en un JSON como este
![](Pasted%20image%2020240516192912.png)
- El ARN es el identificador unico de mi bucket

## CloudFront (CDN)
- De origin domain, podemos elegir una APIG, un LB, un S3, etc
- Nosotros ponemos nuestro S3 en el origen
- Ponemos originar access control settings. En el bucket de S3 permitimos que solo CloudFront lo vea. Entonces si cualquiera quiere llegar a nuestro bucket desde un lugar que no es CloudFront, no nos deja
- CloudFront tiene puntos de presencia en todo el planeta. Nuestro bucket S3 vive en una region especifica (ejemplo: us-east-1). La CDN nos acerca este contenido a todos los puntos de presencia que tienen en el mundo, reduciendo la latencia.
- Tenemos un monton de opciones, como que protocolos aceptamos, que tipos de paquete HTTP, etc.
- Tambien le podemos poner un WAF adelante, nosotros elegimos no hacerlo
- Volvemos al S3 y agregamos la config necesaria para que CloudFront pueda acceder al mismo
![](Pasted%20image%2020240516193651.png)
## Lambdas
- Tienen 9 años
- Es el servicio que mas rapido se adopto en la historia de AWS
- La primera vez que se pudo correr codigo sin servidores
- El billing es una mezcla del tiempo que estuvo corriendo con la cantidad de invocaciones que tuvo
- Esta diseñada para aplicaciones basadas en microservicios
- Tienen un limite de tiempo de ejecucion de 15 minutos
- Hay ciertas librerias nativas, como las de node/python/etc o las de AWS
- Los triggers nos permiten usar tecnologias de terceros (o de AWS) para llamar a una lambda.
- Nosotros vamos a usar un APIG de trigger. La mas costo-efectiva es la HTTP pero tambien podemos hacer que escuche por websockets
- Definimos las rutas HTTP (como POST /) y aca nos deja usar algun authorization
- La primera vez que llamo a una lambda "queda prendida". Entonces si la vuelvo a llamar tiene menos latencia. Despues de un rato, se vuelve a "apagar". Tambien esta la opcion de que la lamba este siempre "prendida", pero esto es muy caro.
## DynamoDB
- Base de datos NoSQL serverless
- Uno no crea los servidores, simplemente creas tablas
- Para que la lambda acceda a nuestro Dynamo, se lo tenemos que permitir desde IAM
## IAM
- Identity access manager
- Vamos a crear una policy
- Esta es la manera visual de generar el JSON de permisos que mencionamos antes
- En este caso, queremos que nos deje el permiso `PutItem` para nuestra tabla `books` de DynamoDB en `us-east-1`
- Vamos a lambda -> configurations -> permissions y le agregamos la policy que acabamos de generar
- En los graficos de AWS las politicas se ven como un casco
## Cloudwatch
- El servicio de concentracion de logs unificados
- La gran mayoria de los servicios en AWS crean un log group en CloudWatch, y ponen entradas por cada una de las ejecuciones
- Por default las lambdas tienen algunos permisos para escribir en CloudWatch. De lo contrario no podrian hacer ningun log.

## Grafico de serverless
- Esta es la arquitectura que hicimos
![](Pasted%20image%2020240516202907.png)
## Notas de final de clase
- Beto cuenta que organizaron la vacunacion en Paraguay originalmente con un servidor en PHP
- Cuando entraron todos juntos al servidor, se cayo
- Hacer un grupo de autoscaling les costaba carisimo y no escalaba tan rapido
- Entonces eligieron migrar a usar lambdas y el servicio no se cayo mas. Usaron dynamo
- Hay un numero maximo de invocaciones en simultaneo que tienen las lambdas. Despues de determinado numero, hay que hacer un pedido especial a AWS para que corra X conexiones concurrentes.
- NO USAR LAMBDA PARA
	- Procesos batch
	- Operaciones con mucho computo