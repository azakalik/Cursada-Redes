# Ejercicio A
**Se necesita realizar un cableado estructurado para un edificio de 7 pisos y 6 oficinas por piso, distribuidas alrededor de un hueco central. El edificio tiene 20 m de frente por 50 m de largo, el hueco central es de 4 m por 5m. La montante se encuentra sobre uno de los lados de 50 metros. Se pide:**
- **Ubicar el centro de cómputos en el 5to piso.**
- **Diseñar la topología de red para que sea tolerante a una falla.**
- **Cablear todos los pisos para tener la mayor cantidad de puestos de trabajo (red y telefonía).**
- **Calcular la cantidad de:**
	- **Metros de fibra.**
	- **Metros de cable.**
	- **Metros de cable-canal.**
	- **Cantidad de racks.**
	- **Cantidad de patcheras.**
	- **Cantidad de rosetas.**
	- **Cantidad de conectores RJ-45 (macho y hembra).**
	- **Cantidad de patch cords.**
	- **Cantidad de switches.**

### Solucion
NOTA: use la pagina https://www.autodraw.com/ para el dibujo
![[Pasted image 20240312155110.png]]
Supuestos:
- Cada oficina tiene una sola roseta RJ45
- Entre pisos hay 3m
- En el piso 5 no hay oficinas
- La fibra es solo lo que va al ISP, la medimos desde el piso 5 hasta planta baja. El resto es cable coaxil.
- Cada oficina tiene un cable que sale desde la roseta que mide 1m
- Suponemos que hay un rack, switch y pachera por piso con oficina

Pasos:
- Primero dibujo uno de los pisos
- Para que sea tolerante a **una falla**, en la montante tiene que haber siempre dos cables que entren al piso y conecten con el switch de piso
- Para calcular los metros de fibra, hacemos el calculo 3 (metros entre piso) por 5 (cantidad de pisos hasta el centro de computo). Nos queda **15 metros de fibra optica**.
- Calculamos el total de patch cord que viaja por el techo
	- Desde el techo hasta cada roseta hay 3m
	- Desde la pachera hasta la oficina 1 hay 20m
	- Desde la pachera hasta la oficina 2 hay 30m
	- Desde la pachera hasta la oficina 3 hay 42.5m
	- Desde la pachera hasta la oficina 4 hay 55m
	- Desde la pachera hasta la oficina 5 hay 67.5m
	- Desde la pachera hasta la oficina 6 hay 77.5m
	- Haciendo la suma, nos da que el **total de cable** desde la pachera hasta las rosetas de cada oficina es de **320m** (tambien incluyendo el metro de cable desde la roseta hasta cada PC)
	- Los **metros de cable canal** son 320 - 6 = **314m**
- Para calcular la cantidad de rosetas, hacemos el calculo pisos con oficinas por cantidad de oficinas. Nos queda que en **el edificio hay 36 rosetas.**
- **El total de racks, switches y pacheras para los pisos con oficina es de 6**
- Para calcular la cantidad de RJ45 tenemos:
	- 2 machos por cada cable desde la roseta hasta la PC -> 2x6x6 = 72
	- 1 hembra por cada roseta -> 6x6 = 36
	- 2 machos por cada cable desde el switch a la pachera en cada piso -> 2x6x6 = 72
	- **Total de machos: 72x2 = 144**
	- **Total de hembras: 36**
	- NOTA: el cable de la pachera hasta las rosetas va soldado
- Para los cables que van desde cada piso a traves de la montante hasta el piso 5 tenemos que:
	- Piso 1 al piso 5: 12m
	- Piso 2 al piso 5: 9m
	- Piso 3 al piso 5: 6m
	- Piso 4 al piso 5: 3m
	- Piso 6 al piso 5: 3m
	- Piso 7 al piso 5: 6m
	- El total nos da: 39m, que al multiplicar por dos (para la tolerancia a un fallo) nos da un **total de cable coaxil de 78m**


