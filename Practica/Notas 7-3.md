# Cableado estructurado

Gráfico de un cableado estructurado de la empresa
	![[Pasted image 20240307201020.png]]
	Los números 1 y 2 son cableado horizontal, los números 3 y 4 son cableado vertical
## Los 4 tramos de una conexion empresarial
### Cableado Horizontal
#### Tramo 1
- Este tramo va del puerto RJ45 de una PC hasta una ficha embutida en la pared, llamada roseta
- El tramo se compone de un cable corto, que se suele romper y se puede intercambiar facilmente
#### Tramo 2
- Este tramo va de las rosetas al patch panel.
- Los cables viajan primero al patch panel de una manera ordenada (generalmente por un cable canal por el techo, desde la roseta embutida a la pared hasta el rack).
- Al patch panel (o patchera) le llegan cables UTP sin ficha, que se conectan con una herramienta de impacto.
- Estos son los cables mas largos y mas duraderos (porque no se andan enchufando y desenchufando), pero tambien los mas dificiles de cambiar
#### Tramo 3
- Es el tramo que va de la patchera al switch de piso (con cables cortos)
- La patchera no tiene logica, es pasiva
- Los cables chiquitos que van de cada puerto de la patchera a cada puerto del switch se suelen romper mas dado que se cambian mas seguido, por eso esta bueno tener la patchera para conectar entre las rosetas y el switch de piso
- Foto de patchera (negro) y switch (gris):
	![[Pasted image 20240307201501.png]]

#### Tramo 4
- Es el tramo que va de cada switch de piso al core
- El core funciona como un router que conecta las redes de cada piso con las redes de los servidores e internet. Dado que todo el routing se hace desde el core, tenemos un unico centro de control
- El core no es un monolito, tiene mucha redundancia para poder hacer mantenimiento y por si se rompe algo. Son muy caros (pueden salir miles de dolares).
- Cuando salgo del switch y voy al al core, salgo con dos cables (por si se rompe uno), y generalmente voy con un cable mejor del que uso para conectar a la patchera (se usan cables de 10GB/s)

## Cosas a tener en cuenta para armar la red
- Cuantas rosetas y cables voy a necesitar para conectar cada una de las computadoras
- Cuantos patch panel necesito
- Cuantos metros de cable para ir a los patch panel
- Cuantos y que tipo de cable (rj45 o fibra) para ir al core. La fibra es mas frágil pero me permite cubrir mayores distancias.