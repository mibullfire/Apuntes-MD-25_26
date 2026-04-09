# PI: Tema 5

## Capacidad del Disco

Número de pistas por sector:

$$\text{nº pistas} \cdot \text{nº sectores/pista} \cdot 512\text{ Bytes}$$

Capacidad total del disco:

$$C_{\text{Disco}} = \text{nº caras} \cdot \text{nº pistas} \cdot \text{nº sectores} \cdot 512\text{ Bytes/sector}$$

Nota: por cadad disco se cuente la cara de arriba y de abajo. Osea, un disco tiene dos caras.

La capacidad del disco se mide en verdad en: número de cilindros (que coincide con el número de pistas), el número de cabezas lectoras (que coincide con las caras) y el número de sectores. Siempre multiplicado por los $512$ Bytes.

El primer sector se encarga del direccionamiento del resto de sectores. Por lo que si tenemos $64$ sectores. El rango de sectores usables, en vez de empezar en cero, empieza en $1$: $1$-$63$.

## Ejercicios

**Ejercicio 1: Un determinado dispositivo tiene los siguientes parámetros:**

- **$2$ caras**
- **$80$ pistas/cara**
- **$20$ sectores/pista**

**Pregunta 1: Calculo de la capacidad.**

$$\text{Capacidad} = 2 \text{ caras} \cdot 80 \text{ pistas/cara}\cdot 20 \text{ sectores/pista} \cdot 512 \text{ Bytes/sector} = 1.56 \text{MB}$$

**Pregunta 2: ¿Puedo almacenar 400 fichecros de 100 Bytes cada uno?**

`Importante:` $\text{Nº Sectores Totales} = 2 \cdot 80 \cdot 20 = 3200 \text{ sectores}$

Si por ejemplo quiero almacenar un archivo de $1024$ Bytes, tendría que almacenarlo en dos sectores de $512$.
Si fueran $1048$ Bytes sería: 512 | 512 | 24. Y el sector de $24$ Bytes ya no puede ser usado nunca más por otro fichero.
`Puedo usar varios sectores para un mismo fichero, pero no puedo usar un sector para diferentes ficheros.`

Para este ejercicio, no supero la capacidad teórica, pero no puedo tener más de 3200 ficheros, por lo que no podría tener los 4000 ficheros que plantea el enunciado.


**Ejercicio 2: Un determinado sistema de almacenamiento tiene los siguientes parámetros:**

- **Cilindros: $100$.**
- **Head: $50$.**
- **S = $4$.**

||C|H|S|LBA|
|-------|-------|-------|-------|-------|
|Primer Sector|0|0|1|0|
|Último Sector|99|49|3|19999|

`Nota`: el último LBA se calcula con $100\cdot50\cdot4-1$. Número total de sectores, menos 1.
