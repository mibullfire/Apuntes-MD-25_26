# Problemas Tema 1

## Ejercicio 1: Encapsulamiento y Fragmentación

**Suponga que, en el modelo OSI, un protocolo de nivel de enlace de datostiene limitado el tamaño máximo de la E_SDU a 1000 bytes, y que el protocolo de nivel de red no limita eltamaño máximo de las R_SDUs a su nivel superior. Si las T_PDUs tienen siempre un tamaño de 2000 bytes y la R_PCI ocupa 100 bytes, ¿Cuántas R_PDUs enviará el nivel de red? ¿Qué contendrá cada R_PDU ? (Nota: Las R_PDUs que se envíen deben ser del tamaño máximo posible).**

![Dibujo #1]()

El tamaño máximo que puede tener la $E\_SDU$ es de $1000$ bytes.

Tenemos que transportar $2000$ bytes de datos provenientes de las $T\_PDU$.

- Primera $R\_PDU$: $100$ bytes de cabecera + $900$ bytes de datos. Nos quedan $1100$ bytes datos por enviar.
- Segunda $R\_PDU$: $100$ bytes de cabecera + $900$ bytes de datos. Nos quedan $200$ bytes datos por enviar.
- Primera $R\_PDU$: $100$ bytes de cabecera + $200$ bytes de datos. Nos quedan $0$ datos por enviar.

Por lo que necesitamos 3 $R\_PDU$.

## Ejercicio 2: Segmentación

![Figura Ejercicio 2](./img/Figura%20Ejercicio%202.png)

**Considerad quese envía un mensaje cuya longitud es8x106 bits desde el origen hasta el destino mostrados en la figura anterior. Suponed que cada enlace mostrado es de 2 Mbps Ignorad los retardos de propagación, de cola y de procesamiento.**

**a) Suponed que el mensaje se transmite desde el origen al destino sin segmentarlo. ¿Cuánto tarda el mensaje en desplazarse desde origen hasta el primer router? Teniendo en cuenta que cada router usa el método de store and forward, ¿cuál es el tiempo total que invierte el mensaje para ir desde el equipo origen al destino?**

Siendo $R=2Mbps$ y $L=8 \cdot 10^6$:

$$d= \frac{L}{R}=4 s$$

Tarda un total de $12s$ en llegar al destino ($d_T$).

**b) Suponedqueelmensaje se segmenta en 4000 paquetesy quelalongitud decada uno es 2000 bits. ¿Cuánto tarda el primer paquete en desplazarse desde el origen hasta el primer router? Cuando se está enviando el primer paquete del primer router al segundo, el host de origen envía un segundo paquete al router, ¿en qué instantede tiempo habrárecibidoel primer router el segundopaquetecompleto?**

$$d= \frac{2000}{2 \cdot 10^6}=1ms$$

Siendo $d_{TOTAL}=3ms+1ms \cdot 3999 = 4002 ms \ 4s$

**c) ¿Cuánto se tarda en transmitir el mensaje completo desde el host origen al destino cuando se emplea la segmentación de mensajes? Comparad este resultado conla respuesta del primer apartadoy comentadlo.**
**d) ¿Cuáles creéis que son los inconvenientes de la segmentación de mensajes?**

## Ejercicio 3: TDM

**¿Cuánto se tarda en enviar un fichero de 640000 bits (640 Kb) desde un sistema final A al B sobre una red de conmutaciónde circuitos?**

- **velocidades de todos los enlaces: $1.536$ Mbps.**
- **cada enlace usa TDM con 24 particiones/marco.**
- **500 ms para establecer el circuito de terminal a terminal.**

![Figura Ejercicio 3](./img/Figura%20Ejercicio%203.png)

Tenemos que calcular la $R_{eff} dividiendo entre el número de particiones del marco:

$$R_{eff} = \frac{1.536 \cdot 10^6}{24} = 64Kbps$$

Con esta nueva velocidad calculamos el tiempo total, teniendo en cuenta el retraso de establecimiento, que hay que sumarlo:

$$d= \frac{L}{R_{eff}} = \frac{640K}{64K}+0,5s=10,5s$$

*Extra*: Suponer ahora que tenemos conmutación de paquetes, y tenemos ahora todo el ancho de banda:

$$d_{paq}=\frac{640K}{1,536 \cdot 10^6}=420ms$$

Como tenemos 4 enlaces: $d{paq}=420ms \cdot 4 = 1680ms$.

## Ejercicio 4: Retardo de Extremo a Extremo

**Considerar un paquete de longitud L transmitido por un sistema terminal A que pasa a través de tres enlaces hasta alcanzar el sistema terminal destino. Los tres enlaces se conectan a través de dos router en una red de conmutación de paquetes. Considerando que di, si y Ri denotan la longitud, la velocidad de propagación y la velocidad de transmisión del enlace i, para i=1,2,3, que los routers retrasan cada paquete dproc, y asumiendo que no hay retardos decolas, en términos de di, si y Ri (i=1,2,3) y L, ¿cuál el retardo total de extremo a extremo para el paquete?**

La formula que vamos a usar es la siguiente:

$$d_{nodal}=d_{proc}+d_{cola}+d_{trans}+d_{prop}$$

$$d_{nodal}=x+0+\frac{L}{R}+\frac{d}{s}$$

Nuestro esquema seria:

    A - R_1 - R_2 - B

Los enlaces son los "cables" que unen a cada uno.

Tenemos que calcular el total:
$$d_T=d_{E1}+d_{E2}+d_{E3}$$

$$d_{E1}=d_{proc_{E1}}+\frac{L}{R_1}+\frac{d_1}{s_1}$$

De las formulas anteriores obtenemos:
$$d_T=2\cdot d_{proc}+ \sum^3_{i=1} \left[ \frac{L}{R_i}+\frac{d_i}{s_i}\right]$$
Tenemos en cuenta que hay que procesar la información en dos procesadores, en este caso no estamos contando con que el equipo B tenga que procesar la información. Esto último ha de indicarse en el examen.

**Suponer ahora que la longitud del paquete es $1500$ bytes, la velocidad de propagación en los enlaces es $2.5 \times 10^8$ m/s, la velocidad de transmisión es $2$ Mbps, el retardo de procesamiento del router es $3$ ms, la longitud del primer enlace es $5000$ km, la del segundo $4000$ km y la del tercero $1000$ km. Para estos valores, ¿cuál es el retardo de extremo a extremo?**

$$d_T=2\cdot 3 \times 10^{-3}+ \left[ \frac{12000}{2 \times 10^6}+\frac{5 \times 10^6}{2.5 \times 10^8}\right] + \left[ \frac{12000}{2 \times 10^6}+\frac{4 \times 10^6}{2.5 \times 10^8}\right] + \left[ \frac{12000}{2 \times 10^6}+\frac{1 \times 10^6}{2.5 \times 10^8}\right]$$

$$d_T=0.006+ 0.026 + 0.022 + 0.01 = 0.064 \text{ s} = 64 \text{ ms}$$

## Ejercicio 5: Retardo de Cola

**Un router en una red conmutación de paquetes recibe un paquete y determina el enlace de salida al cual el paquete debe reenviarse. Cuando el paquete llega, otro paquete se está transmitiendo por ese enlace de salida (la mitad se ha transmitido) y tres más están esperando para ser transmitidos. Los paquetes se transmiten por orden de llegada.**

**Suponiendo que todos los paquetes tiene una longitud de 1500 bytes y que la velocidad del enlace es de 2Mbps. ¿Cuál es el retardo de cola del paquete?**

Tenemos un paquete que va por la mitad, dicho paquete tardará:

$$d=\frac{\frac{1500 \times 8}{2}}{2\times10^6}=3ms$$

Y el resto de paquetes:

$$d_{prop}=3\times\frac{12000}{2\times10^6}=3\times6ms=18ms$$

Siendo el total: $d_{cola}=21ms$.

**De forma general, ¿cuál es el retardo de cola cuando todos los paquetes tienen longitud L, la velocidad de transmisión es R, x bits del paquete que se está transmitiendo se han transmitido, y n paquetes están ya en la cola?**

Para el paquete que se está transmietiendo:
$$d=\frac{L-x}{R}$$

Para el resto:
$$d=n\times\frac{L}{R}$$

Siendo el total:
$$d_{cola}=\frac{(L-x)+nL}{R}$$

*Nota*: teniendo en cuenta de que $L$ y $x$ representan cantidad de `bits`.
