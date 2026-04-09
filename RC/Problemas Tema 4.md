# Soluciones - Tema 4: La Capa de Red

## Redes de Computadores — Grado en Ingeniería Informática

---

## Problema 1

### Enunciado

Un router ha recibido por una de sus interfaces una IP_PDU sin opciones de 2400 bytes que debe reenviar por otra interfaz de salida cuya MTU es de 1000 bytes. ¿Cuántas IP_PDUs reenviará por dicha interfaz?

### Solución

**Datos:**

- Tamaño de la IP_PDU original: 2400 bytes
- Sin opciones → cabecera IP (IP_PCI) = 20 bytes
- Datos (IP_UD) de la IP_PDU original: 2400 − 20 = 2380 bytes
- MTU de la interfaz de salida: 1000 bytes

**Proceso de fragmentación:**

Como la IP_PDU (2400 bytes) es mayor que la MTU de la interfaz de salida (1000 bytes), es necesario fragmentar. Cada fragmento es una nueva IP_PDU que debe caber en la MTU, y cada una lleva su propia cabecera IP de 20 bytes. Por tanto, el espacio disponible para datos en cada fragmento es:

- Espacio para datos por fragmento = 1000 − 20 = **980 bytes**

**Cálculo del número de fragmentos:**

- Fragmento 1: 980 bytes de datos → quedan 2380 − 980 = 1400 bytes
- Fragmento 2: 980 bytes de datos → quedan 1400 − 980 = 420 bytes
- Fragmento 3: 420 bytes de datos (último fragmento)

**Respuesta: El router reenviará 3 IP_PDUs (fragmentos) por dicha interfaz.**

| Fragmento | Cabecera IP | Datos | Tamaño total |
|---|---|---|---|
| 1 | 20 bytes | 980 bytes | 1000 bytes |
| 2 | 20 bytes | 980 bytes | 1000 bytes |
| 3 | 20 bytes | 420 bytes | 440 bytes |

**Verificación:** 980 + 980 + 420 = 2380 bytes de datos = IP_UD original ✓

El reensamblado de los tres fragmentos se realizará en el nivel de red del destino final.

---

## Problema 2

### Enunciado

Suponga que a una empresa le asignan el bloque CIDR 200.1.0.0/24, determine de manera razonada cuántas subredes podría crear dentro de la empresa, qué prefijo de red, dirección broadcast y rango de direcciones IP asignable tendría cada una, si el número de sistemas finales a conectar en cada subred es de 20. ¿Cambiaría su respuesta si fuesen 30 sistemas finales?

### Solución

**Bloque de partida:** 200.1.0.0/24 → 256 direcciones totales, 254 asignables.

#### Caso 1: 20 sistemas finales por subred

Necesitamos al menos 20 + 1 (interfaz del router) = 21 direcciones asignables por subred, más la dirección de red y broadcast → al menos 23 direcciones totales.

La potencia de 2 más cercana ≥ 23 es **32 (2⁵)**, lo que corresponde a un prefijo **/27** (32 − 5 = 27).

Cada subred /27 tiene 32 direcciones (30 asignables).

Tomamos prestados n = 27 − 24 = **3 bits** del identificador de host → se crean **2³ = 8 subredes**.

| Subred | Prefijo de red | Rango asignable | Broadcast |
|---|---|---|---|
| 0 | 200.1.0.0/27 | 200.1.0.1 – 200.1.0.30 | 200.1.0.31 |
| 1 | 200.1.0.32/27 | 200.1.0.33 – 200.1.0.62 | 200.1.0.63 |
| 2 | 200.1.0.64/27 | 200.1.0.65 – 200.1.0.94 | 200.1.0.95 |
| 3 | 200.1.0.96/27 | 200.1.0.97 – 200.1.0.126 | 200.1.0.127 |
| 4 | 200.1.0.128/27 | 200.1.0.129 – 200.1.0.158 | 200.1.0.159 |
| 5 | 200.1.0.160/27 | 200.1.0.161 – 200.1.0.190 | 200.1.0.191 |
| 6 | 200.1.0.192/27 | 200.1.0.193 – 200.1.0.222 | 200.1.0.223 |
| 7 | 200.1.0.224/27 | 200.1.0.225 – 200.1.0.254 | 200.1.0.255 |

#### Caso 2: 30 sistemas finales por subred

Necesitamos al menos 30 + 1 = 31 direcciones asignables → al menos 33 direcciones totales.

La potencia de 2 más cercana ≥ 33 es **64 (2⁶)**, lo que corresponde a un prefijo **/26** (32 − 6 = 26).

Cada subred /26 tiene 64 direcciones (62 asignables).

Tomamos prestados n = 26 − 24 = **2 bits** → se crean **2² = 4 subredes**.

| Subred | Prefijo de red | Rango asignable | Broadcast |
|---|---|---|---|
| 0 | 200.1.0.0/26 | 200.1.0.1 – 200.1.0.62 | 200.1.0.63 |
| 1 | 200.1.0.64/26 | 200.1.0.65 – 200.1.0.126 | 200.1.0.127 |
| 2 | 200.1.0.128/26 | 200.1.0.129 – 200.1.0.190 | 200.1.0.191 |
| 3 | 200.1.0.192/26 | 200.1.0.193 – 200.1.0.254 | 200.1.0.255 |

**Sí cambia la respuesta:** con 30 sistemas finales se crean 4 subredes (en lugar de 8) con un prefijo /26 (en lugar de /27).

---

## Problema 3

### Enunciado

Red de una empresa con bloque CIDR 200.1.2.0/24, conectada a Internet a través de E2 de R1. Rext no pertenece a la empresa. Red entre R1 y Rext: 150.214.141.24/27.

### Solución

#### a) ¿Cuántos dominios de broadcast hay?

Los routers separan dominios de broadcast. Los switches y hubs no.

Cada interfaz de R1 pertenece a un dominio de broadcast distinto:

1. **Dominio 1 (E1 de R1):** SW3 — hosts A, C, Servidor DNS, Servidor Web
2. **Dominio 2 (E0 de R1):** SW1 — SW2 — hosts B, D, E, F
3. **Dominio 3 (E2 de R1):** enlace R1 ↔ Rext (red 150.214.141.24/27)

**Hay 3 dominios de broadcast.**

#### b) Entrada en la TE de Rext para reenviar tráfico a la empresa

Rext necesita una entrada para alcanzar toda la red de la empresa. El próximo salto es la dirección IP de R1 en la red compartida.

| Red | Próximo salto | Interfaz |
|---|---|---|
| 200.1.2.0/24 | 150.214.141.25 | (interfaz hacia R1) |

(Asumiendo que R1 tiene IP 150.214.141.25/27 en E2)

#### c) Asignación de prefijos de red

Bloque disponible: **200.1.2.0/24** (256 direcciones). Necesidades:

- Subred PC D (SW2/SW1): 90 PCs + interfaces de router ≈ 93 dispositivos → **/25** (128 dir, 126 asignables)
- Subred PC A (SW3): 30 PCs + servidores + router ≈ 33 dispositivos → **/26** (64 dir, 62 asignables)
- Red R1↔Rext: ya direccionada con 150.214.141.24/27 (no consume del bloque)

| Subred | Prefijo | Rango asignable | Broadcast | Máscara |
|---|---|---|---|---|
| SW1/SW2 (90 PCs) | 200.1.2.0/25 | 200.1.2.1 – 200.1.2.126 | 200.1.2.127 | 255.255.255.128 |
| SW3 (30 PCs) | 200.1.2.128/26 | 200.1.2.129 – 200.1.2.190 | 200.1.2.191 | 255.255.255.192 |
| **Libre** | 200.1.2.192/26 | — | — | — |

**Espacio libre:** 200.1.2.192/26 (64 direcciones para futuras ampliaciones).

#### d) Configuración IPv4

**Interfaces de R1:**

| Interfaz | Dirección IP | Máscara |
|---|---|---|
| E0 (SW1/SW2) | 200.1.2.1 | 255.255.255.128 |
| E1 (SW3) | 200.1.2.129 | 255.255.255.192 |
| E2 (Rext) | 150.214.141.25 | 255.255.255.224 |

**PC A (subred SW3):**

| Parámetro | Valor |
|---|---|
| Dirección IP | 200.1.2.130 |
| Máscara | 255.255.255.192 |
| Puerta de enlace | 200.1.2.129 |

**PC D (subred SW1/SW2):**

| Parámetro | Valor |
|---|---|
| Dirección IP | 200.1.2.2 |
| Máscara | 255.255.255.128 |
| Puerta de enlace | 200.1.2.1 |

**Tabla de enrutamiento de R1:**

| Red | Próximo salto | Interfaz |
|---|---|---|
| 200.1.2.0/25 | — | E0 |
| 200.1.2.128/26 | — | E1 |
| 150.214.141.24/27 | — | E2 |
| 0.0.0.0/0 | 150.214.141.26 | E2 |

---

## Problema 4

### Enunciado

Empresa RedesDeComputadores S.A. con bloque CIDR 101.110.119.128/27. Routers R1, R2, R3 (empresa) y Rext (proveedor). Red R1↔Rext: 120.60.30.0/30. Se quiere dejar espacio para 6 PCs más en Hub1.

### Solución

#### 1) Esquema de direccionamiento

**Bloque:** 101.110.119.128/27 → 32 direcciones (101.110.119.128 – 101.110.119.159).

Identificación de subredes (Hub1 comparte dominio de broadcast con SW1):

- **Subred C (SW1 + Hub1):** R1-Fa10, R2-Fa1, R3-Fa1, PC D, PC E + 6 PCs futuros = 11 dispositivos → **/28** (16 dir, 14 asignables)
- **Subred A (SW3):** R3-Fa0, Servidor DNS, PC A = 3 dispositivos → **/29** (8 dir, 6 asignables)
- **Subred B (SW2):** R2-Fa0, PC B, PC C = 3 dispositivos → **/29** (8 dir, 6 asignables)

Total: 16 + 8 + 8 = 32 ✓ (usa todo el bloque, no queda espacio para futuras subredes)

| Subred | Prefijo | Rango asignable | Broadcast | Máscara |
|---|---|---|---|---|
| C (SW1+Hub1) | 101.110.119.128/28 | .129 – .142 | .143 | 255.255.255.240 |
| A (SW3) | 101.110.119.144/29 | .145 – .150 | .151 | 255.255.255.248 |
| B (SW2) | 101.110.119.152/29 | .153 – .158 | .159 | 255.255.255.248 |

(Todas las direcciones con prefijo 101.110.119)

#### 2) ¿Es necesario NAT?

**No.** El bloque 101.110.119.128/27 contiene direcciones **públicas** (no pertenece a los rangos privados RFC 1918: 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16). Los datagramas con estas direcciones origen pueden circular por Internet sin traducción.

#### 3) Direcciones IP de interfaces y TE de R1

**Interfaces de los routers:**

| Router | Interfaz | Subred | Dirección IP | Máscara |
|---|---|---|---|---|
| R1 | Fa10 | C | 101.110.119.129/28 | 255.255.255.240 |
| R1 | Fa0 | R1↔Rext | 120.60.30.1/30 | 255.255.255.252 |
| R2 | Fa1 | C | 101.110.119.130/28 | 255.255.255.240 |
| R2 | Fa0 | B | 101.110.119.153/29 | 255.255.255.248 |
| R3 | Fa1 | C | 101.110.119.131/28 | 255.255.255.240 |
| R3 | Fa0 | A | 101.110.119.145/29 | 255.255.255.248 |
| Rext | Fa0 | R1↔Rext | 120.60.30.2/30 | 255.255.255.252 |

**Tabla de enrutamiento de R1:**

| Red | Próximo salto | Interfaz |
|---|---|---|
| 101.110.119.128/28 | — | Fa10 |
| 101.110.119.144/29 | 101.110.119.131 | Fa10 |
| 101.110.119.152/29 | 101.110.119.130 | Fa10 |
| 120.60.30.0/30 | — | Fa0 |
| 0.0.0.0/0 | 120.60.30.2 | Fa0 |

- La subred A se alcanza a través de R3 (101.110.119.131), que está en la subred C directamente conectada.
- La subred B se alcanza a través de R2 (101.110.119.130), también en la subred C.

---

## Problema 5

### Enunciado

Empresa pública con bloque CIDR único, accede a Internet por R2. Subredes: 200.1.1.0/26, 200.1.1.64/27 y 200.1.1.128/25. E0 de R2: IP=223.14.15.1, máscara=255.255.255.252.

### Solución

#### a) Máximo de sistemas finales por subred

Descontamos las direcciones de red, broadcast e interfaces de routers:

- **200.1.1.0/26:** 2⁶ − 2 = 62 asignables. Restando la interfaz de R1 → **61 sistemas finales máximo** (o 60 si R2 también tiene interfaz aquí).
- **200.1.1.64/27:** 2⁵ − 2 = 30 asignables. Restando interfaz de R1 → **29 sistemas finales máximo**.
- **200.1.1.128/25:** 2⁷ − 2 = 126 asignables. Restando interfaz de R2 → **125 sistemas finales máximo**.

#### b) ¿Es necesario NAT en R2?

**No.** Las direcciones 200.1.1.x son direcciones **públicas** (no pertenecen a los rangos privados RFC 1918). Al ser una empresa pública con un bloque CIDR asignado, no necesita NAT para acceder a Internet.

#### c) Prefijo de red en la TE de un router de Internet (RI)

Las tres subredes de la empresa se pueden agregar. Veamos:

- 200.1.1.0/26 → 200.1.1.00**000000**
- 200.1.1.64/27 → 200.1.1.01**000000**
- 200.1.1.128/25 → 200.1.1.1**0000000**

El bloque que engloba a las tres subredes es **200.1.1.0/24** (los primeros 24 bits son comunes).

En la tabla de enrutamiento de RI aparecería:

| Red | Próximo salto | Interfaz |
|---|---|---|
| 200.1.1.0/24 | (IP de R2 en el enlace) | ... |

#### d) ¿Podría la empresa direccionar una nueva subred?

Analicemos el espacio usado dentro de 200.1.1.0/24:

- 200.1.1.0/26: direcciones 0–63
- 200.1.1.64/27: direcciones 64–95
- 200.1.1.128/25: direcciones 128–255

**Espacio libre:** 200.1.1.96 – 200.1.1.127 → es un bloque contiguo de 32 direcciones → **200.1.1.96/27**.

Además hay que considerar la red entre R1 y R2, que también consume espacio (probablemente un /30 del bloque).

**Sí, se podría crear una nueva subred 200.1.1.96/27** con 30 direcciones asignables (28 sistemas finales máximo descontando interfaces de routers).

#### e) Configuración de interfaces y tablas de enrutamiento

**Red entre R1 y R2:** Necesitamos un /30 para el enlace punto a punto. Usamos parte del espacio libre: **200.1.1.96/30**.

**Interfaces de R1:**

| Interfaz | Dirección IP | Máscara |
|---|---|---|
| E1 (200.1.1.0/26) | 200.1.1.1/26 | 255.255.255.192 |
| E0 (200.1.1.64/27) | 200.1.1.65/27 | 255.255.255.224 |
| — (enlace a R2) | 200.1.1.97/30 | 255.255.255.252 |

**Interfaces de R2:**

| Interfaz | Dirección IP | Máscara |
|---|---|---|
| E0 (enlace a Rext) | 223.14.15.1/30 | 255.255.255.252 |
| E1 (enlace a R1) | 200.1.1.98/30 | 255.255.255.252 |
| E2 (200.1.1.128/25) | 200.1.1.129/25 | 255.255.255.128 |

**TE de R1:**

| Red | Próximo salto | Interfaz |
|---|---|---|
| 200.1.1.0/26 | — | E1 |
| 200.1.1.64/27 | — | E0 |
| 200.1.1.96/30 | — | (enlace a R2) |
| 0.0.0.0/0 | 200.1.1.98 | (enlace a R2) |

**TE de R2:**

| Red | Próximo salto | Interfaz |
|---|---|---|
| 200.1.1.128/25 | — | E2 |
| 200.1.1.96/30 | — | E1 |
| 223.14.15.0/30 | — | E0 |
| 200.1.1.0/26 | 200.1.1.97 | E1 |
| 200.1.1.64/27 | 200.1.1.97 | E1 |
| 0.0.0.0/0 | 223.14.15.2 | E0 |

**Sistema final de la subred 200.1.1.0/26:**

| Parámetro | Valor |
|---|---|
| Dirección IP | 200.1.1.2 |
| Máscara | 255.255.255.192 |
| Puerta de enlace | 200.1.1.1 |

**Sistema final de la subred 200.1.1.64/27:**

| Parámetro | Valor |
|---|---|
| Dirección IP | 200.1.1.66 |
| Máscara | 255.255.255.224 |
| Puerta de enlace | 200.1.1.65 |

**Sistema final de la subred 200.1.1.128/25:**

| Parámetro | Valor |
|---|---|
| Dirección IP | 200.1.1.130 |
| Máscara | 255.255.255.128 |
| Puerta de enlace | 200.1.1.129 |

---

## Problema 6

### Enunciado

Un router interconecta tres redes: red 1 con 63 sistemas finales, red 2 con 95 sistemas finales y red 3 con 16 sistemas finales. ¿Es posible direccionarlas con el bloque CIDR 223.1.17.0/24?

### Solución

**Bloque:** 223.1.17.0/24 → 256 direcciones totales.

**Necesidades** (sistemas finales + interfaz del router + red + broadcast):

- **Red 2 (95 SF):** 95 + 1 (router) = 96 asignables → necesita ≥ 98 direcciones → **/25** (128 dir, 126 asignables) ✓
- **Red 1 (63 SF):** 63 + 1 = 64 asignables → necesita ≥ 66 direcciones → **/26** (64 dir, 62 asignables) → ¡NO alcanza! 62 < 64 necesarios. Probamos **/25** (128 dir, 126 asignables).

Pero si red 2 necesita /25 (128 dir) y red 1 necesita /25 (128 dir), ya consumimos 256 direcciones y no queda nada para red 3.

**Replanteemos** contando solo sistemas finales + router:

- **Red 1:** 63 + 1 = 64 asignables → /26 da 62 asignables. **No alcanza** (64 > 62).

El problema es que con 63 sistemas finales más la interfaz del router (64 dispositivos), un /26 (62 asignables) no es suficiente. Necesitaríamos un /25 (126 asignables).

**Veamos si es posible:**

- Red 2: /25 → 128 direcciones
- Red 1: /25 → 128 direcciones
- Total: 256 → agota el bloque sin dejar espacio para red 3.

**No es posible direccionar las tres redes con el bloque 223.1.17.0/24.**

Si se considerara que las interfaces del router no cuentan como sistemas finales adicionales (interpretación alternativa donde los 63 SF ya incluyen todo), entonces:

- Red 2: 95 + 2 = 97 → /25 (128 dir)
- Red 1: 63 + 2 = 65 → /25 (128 dir)
- Total: 256, sin espacio para red 3. **Sigue sin ser posible.**

Incluso si pudiéramos encajar red 1 en /26:
- Red 2: /25 → 128 dir
- Red 1: /26 → 64 dir
- Red 3: 16 + 1 = 17 → /27 (32 dir, 30 asignables)
- Quedan: 256 − 128 − 64 = 64. Un /27 cabe → 32 dir.
- Total: 128 + 64 + 32 = 224 ≤ 256 ✓

**Bajo esta interpretación** (63 SF ya incluyen la interfaz del router, es decir, se conectan 63 dispositivos y el router es uno de ellos o solo hay 63 SF sin contar al router pero un /26 con 62 asignables es suficiente si interpretamos que el enunciado solo cuenta SF excluyendo routers):

Si la interfaz del router es adicional a los 63: necesitamos 64 asignables → /26 solo da 62 → **NO cabe**.

Si los 63 SF NO incluyen la interfaz del router pero asumimos que la interfaz del router no necesita una IP de ese rango: entonces 63 ≤ 62 sigue sin ser cierto → **NO cabe** en /26.

**Conclusión:** Con la interpretación estricta de que hay que identificar 63 sistemas finales + la interfaz del router = 64 direcciones asignables, un /26 (62 asignables) no es suficiente para red 1, y se necesitaría un /25. En ese caso, se necesitan dos /25 (para red 1 y red 2), lo que consume todo el bloque. **No es posible.**

**Nota alternativa:** Si se asume que los 63 SF incluyen la interfaz del router (es decir, se necesitan solo 63 asignables), tampoco cabe en un /26 (62 asignables < 63). **No es posible** en ningún caso con esta lectura.

**Sin embargo**, si el enunciado se interpreta como que hay exactamente 63 hosts (sin contar la interfaz del router) y un /26 con 62 posiciones asignables no basta, podríamos intentar con:

- Red 2 (95 SF): necesita 97 dir totales → /25 (128)
- Quedan 128 direcciones
- Red 1 (63 SF): necesita 65 dir → /25 (128)... consume todo.

**Respuesta final: No es posible direccionar las tres redes con 223.1.17.0/24** dado que la red 1 (63 SF) y la red 2 (95 SF) necesitan ambas un /25 cada una, agotando el bloque sin dejar espacio para la red 3.

---

## Problema 7

### Enunciado

Un router conecta dos redes (150.0.0.0 y 192.0.0.0) con direccionamiento con clase.

### Solución

#### a) Rango de direcciones, broadcast, TE y configuración

**Red 150.0.0.0:** Primer byte 150 → 128–191 → **Clase B**
- ID de Red: 150.0.0.0
- Máscara: 255.255.0.0
- Rango asignable: 150.0.0.1 – 150.0.255.254
- Broadcast: 150.0.255.255
- Nº hosts máximo: 2¹⁶ − 2 = 65534

**Red 192.0.0.0:** Primer byte 192 → 192–223 → **Clase C**
- ID de Red: 192.0.0.0
- Máscara: 255.255.255.0
- Rango asignable: 192.0.0.1 – 192.0.0.254
- Broadcast: 192.0.0.255
- Nº hosts máximo: 2⁸ − 2 = 254

**Configuración ejemplo:**

| Dispositivo | Interfaz | Dirección IP | Máscara | Puerta de enlace |
|---|---|---|---|---|
| Router | E0 | 150.0.0.1 | 255.255.0.0 | — |
| Router | E1 | 192.0.0.1 | 255.255.255.0 | — |
| Host A (red 150) | — | 150.0.0.2 | 255.255.0.0 | 150.0.0.1 |
| Host B (red 192) | — | 192.0.0.2 | 255.255.255.0 | 192.0.0.1 |

**TE del Router:**

| Red | Próximo salto | Interfaz |
|---|---|---|
| 150.0.0.0 | — | E0 |
| 192.0.0.0 | — | E1 |

**TE del Host A:**

| Red | Próximo salto | Interfaz |
|---|---|---|
| 150.0.0.0 | — | E |
| 0.0.0.0 | 150.0.0.1 | E |

**TE del Host B:**

| Red | Próximo salto | Interfaz |
|---|---|---|
| 192.0.0.0 | — | E |
| 0.0.0.0 | 192.0.0.1 | E |

#### b) IP_PDU con destino 192.0.0.255 desde red 150.0.0.0

La dirección 192.0.0.255 es la **dirección de broadcast dirigido** de la red 192.0.0.0 (clase C, broadcast = todos los bits de host a 1).

El nivel de red del host origen entrega la IP_PDU al router (por su ruta por defecto). El router busca en su TE y encuentra que 192.0.0.0 es directamente conectada por E1. Al ser broadcast dirigido, la IP_PDU se envía por E1 y la reciben **todos los dispositivos (hosts y router) de la red 192.0.0.0**.

#### c) IP_PDU con destino 255.255.255.255 desde red 192.0.0.0

La dirección 255.255.255.255 es la dirección de **broadcast limitado**. Esta dirección identifica a todos los dispositivos de la red a la que pertenece el origen.

Los datagramas con destino 255.255.255.255 **no son reenviados por los routers**. Por tanto, la IP_PDU solo será recibida por **todos los dispositivos de la red 192.0.0.0** (la red del emisor). No llegará a la red 150.0.0.0.

---

## Problema 8

### Enunciado

Empresa pública con acceso a Internet por R2. Tiene Red-1 (12 PCs), Red-2 (12 PCs), Red-3 (12 PCs) y Red (50 PCs).

### Solución

#### a) Direccionamiento con clase

Hay 4 subredes: tres con 12 PCs y una con 50 PCs.

Con direccionamiento con clase:
- Red de 50 PCs: necesita más de 50+2 = 52 direcciones → Clase C (254 asignables) es suficiente.
- Redes de 12 PCs: cada una necesita 14 direcciones → Clase C también es suficiente.

Se necesitarían **4 redes de Clase C** (la clase con menor desperdicio para estos tamaños, ya que una clase B daría 65534 asignables, enorme desperdicio).

**Respuesta:** Habría que asignar **4 redes de Clase C**. Aun así, hay desperdicio: cada Clase C tiene 254 asignables pero las redes pequeñas solo usan ~12.

#### b) ¿Es posible con 200.1.1.0/25?

Bloque: 200.1.1.0/25 → 128 direcciones (200.1.1.0 – 200.1.1.127).

Necesidades:
- Red (50 PCs): 50 + 1 (router) + 2 = 53 → /26 (64 dir, 62 asignables) ✓
- Red-1 (12 PCs): 12 + 1 + 2 = 15 → /28 (16 dir, 14 asignables) ✓
- Red-2 (12 PCs): igual → /28 (16 dir) ✓
- Red-3 (12 PCs): igual → /28 (16 dir) ✓
- Red entre R1 y R2: /30 (4 dir) ✓

Total: 64 + 16 + 16 + 16 + 4 = 116 ≤ 128 ✓

**Sí es posible.** Asignación:

| Subred | Prefijo | Rango asignable | Broadcast | Máscara |
|---|---|---|---|---|
| Red (50 PCs) | 200.1.1.0/26 | .1 – .62 | .63 | 255.255.255.192 |
| Red-1 (12 PCs) | 200.1.1.64/28 | .65 – .78 | .79 | 255.255.255.240 |
| Red-2 (12 PCs) | 200.1.1.80/28 | .81 – .94 | .95 | 255.255.255.240 |
| Red-3 (12 PCs) | 200.1.1.96/28 | .97 – .110 | .111 | 255.255.255.240 |
| R1↔R2 | 200.1.1.112/30 | .113 – .114 | .115 | 255.255.255.252 |
| **Libre** | 200.1.1.116 – .127 | — | — | — |

Espacio libre: 12 direcciones (200.1.1.116 – 200.1.1.127).

#### c) ¿Se puede añadir una subred de 13 PCs a R1?

13 PCs + 1 router = 14 asignables → /28 (16 dir, 14 asignables) ✓

Espacio libre disponible: 12 direcciones → un /28 necesita 16 direcciones. **No cabe.**

Pero un /29 (8 dir, 6 asignables) no es suficiente para 14 dispositivos. Y no hay 16 direcciones contiguas libres.

**No es posible** conectar una nueva subred de 13 PCs a R1 con el espacio restante.

Sin embargo, si reorganizamos: las 3 redes de 12 PCs podrían caber en /28 (ya asignadas así). El espacio 200.1.1.116 – 200.1.1.127 solo tiene 12 direcciones. La subred más grande que cabe ahí es /28 empezando en .112, pero .112–.115 ya están usados para el enlace R1↔R2.

**Respuesta: No es posible** añadir una subred de 13 PCs con el espacio disponible.

**TE de R2 (con mínimo de entradas, usando agregación):**

Dado que todas las subredes de la empresa están detrás de R1 (excepto la red de 50 PCs que está directamente conectada a R2), y que se puede agregar:

| Red | Próximo salto | Interfaz |
|---|---|---|
| 200.1.1.0/26 | — | (interfaz hacia Red 50 PCs) |
| 200.1.1.112/30 | — | (enlace a R1) |
| 200.1.1.64/26 | 200.1.1.113 | (enlace a R1) |
| 223.14.15.0/30 | — | E0 |
| 0.0.0.0/0 | 223.14.15.2 | E0 |

Nota: 200.1.1.64/26 agrega las tres redes de 12 PCs (200.1.1.64–127), reduciendo entradas.

#### d) ¿Cambiaría si la subred se conecta a R2?

Si la nueva subred se conectara a una interfaz libre de R2, seguiría siendo necesario un /28 (16 direcciones) y solo hay 12 disponibles. **La respuesta no cambia: sigue sin ser posible.**

La única diferencia sería que la TE de R2 no necesitaría una entrada con próximo salto para esa subred (sería directamente conectada), pero el problema de espacio de direcciones persiste.

---

## Problema 9

### Enunciado

Red de empresa con acceso a Internet por Router 1. Routers 1, 2, 3. Red entre Router 1 y RTX: 150.214.141.0/24. Bloque empresa: 199.99.1.0/24. Red A: 125 PCs. Red B: 61 PCs.

### Solución

#### a) Dominios de broadcast

Mirando la topología (Router 1 con E0 y E1; Router 2 con E0 y E1; Router 3 con E0 y E1):

1. Red entre RTX y Router 1 (150.214.141.0/24)
2. Red entre Router 1 (E1), Router 2 (E0) y Router 3 (E0) — enlace troncal
3. Red A (Router 2 – E1)
4. Red B (Router 3 – E1)

**Hay 4 dominios de broadcast en la empresa** (excluyendo el enlace a Internet, 3 si solo contamos las internas + el troncal).

Si contamos todos: **4 dominios de broadcast.**

#### b) Asignación de direcciones

Bloque: **199.99.1.0/24** → 256 direcciones.

Necesidades:
- **Red A (125 PCs):** 125 + 1 (router) = 126 asignables → /25 (128 dir, 126 asignables) ✓ exacto
- **Red B (61 PCs):** 61 + 1 = 62 asignables → /26 (64 dir, 62 asignables) ✓ exacto
- **Enlace troncal (R1, R2, R3):** 3 dispositivos → /29 (8 dir, 6 asignables) ✓

Asignación (primero la más grande para dejar espacio contiguo):

| Subred | Prefijo | Rango asignable | Broadcast | Máscara |
|---|---|---|---|---|
| Red A (125 PCs) | 199.99.1.0/25 | .1 – .126 | .127 | 255.255.255.128 |
| Red B (61 PCs) | 199.99.1.128/26 | .129 – .190 | .191 | 255.255.255.192 |
| Enlace troncal | 199.99.1.192/29 | .193 – .198 | .199 | 255.255.255.248 |
| **Libre** | 199.99.1.200 – .255 | — | — | — |

Espacio libre: 56 direcciones para futuras ampliaciones.

#### c) Dirección IP y máscara de interfaces de routers

| Router | Interfaz | Subred | Dirección IP | Máscara |
|---|---|---|---|---|
| Router 1 | E0 | 150.214.141.0/24 | 150.214.141.1/24 | 255.255.255.0 |
| Router 1 | E1 | Troncal | 199.99.1.193/29 | 255.255.255.248 |
| Router 2 | E0 | Troncal | 199.99.1.194/29 | 255.255.255.248 |
| Router 2 | E1 | Red A | 199.99.1.1/25 | 255.255.255.128 |
| Router 3 | E0 | Troncal | 199.99.1.195/29 | 255.255.255.248 |
| Router 3 | E1 | Red B | 199.99.1.129/26 | 255.255.255.192 |

#### d) Espacio libre para futuras redes

Rango libre: 199.99.1.200 – 199.99.1.255 = 56 direcciones.

Subredes posibles maximizando hosts:

- **199.99.1.200/29** → 6 asignables (200–207)
- **199.99.1.208/28** → 14 asignables (208–223)
- **199.99.1.224/27** → 30 asignables (224–255)

O bien un bloque mayor si alineamos:
- **199.99.1.224/27** (32 dir) + **199.99.1.208/28** (16 dir) + **199.99.1.200/29** (8 dir) = 56 dir.

Para maximizar hosts en una sola subred: **199.99.1.224/27** con 30 asignables.

#### e) Tablas de enrutamiento

**TE Router 1:**

| Red | Próximo salto | Interfaz |
|---|---|---|
| 150.214.141.0/24 | — | E0 |
| 199.99.1.192/29 | — | E1 |
| 199.99.1.0/25 | 199.99.1.194 | E1 |
| 199.99.1.128/26 | 199.99.1.195 | E1 |
| 0.0.0.0/0 | 150.214.141.X (IP de RTX) | E0 |

Nota: Se podría agregar las redes de la empresa como 199.99.1.0/24, pero no tiene sentido ya que R1 necesita distinguir entre el troncal, red A y red B para elegir el próximo salto correcto. Aun así, podríamos simplificar usando la ruta por defecto solo para Internet.

**TE Router 2:**

| Red | Próximo salto | Interfaz |
|---|---|---|
| 199.99.1.192/29 | — | E0 |
| 199.99.1.0/25 | — | E1 |
| 0.0.0.0/0 | 199.99.1.193 | E0 |

La ruta por defecto envía todo (Internet + red B + cualquier otra red) a Router 1.

**TE Router 3:**

| Red | Próximo salto | Interfaz |
|---|---|---|
| 199.99.1.192/29 | — | E0 |
| 199.99.1.128/26 | — | E1 |
| 0.0.0.0/0 | 199.99.1.193 | E0 |

#### f) ¿Necesita RTX una entrada para la red de la empresa?

**Sí.** RTX necesita saber cómo alcanzar las redes de la empresa. Al estar todas las subredes de la empresa dentro de 199.99.1.0/24, basta con una entrada agregada:

| Red | Próximo salto | Interfaz |
|---|---|---|
| 199.99.1.0/24 | 150.214.141.1 | (interfaz hacia Router 1) |

El próximo salto sería la dirección IP de Router 1 en la red compartida (150.214.141.1).

---

## Problema 10

### Enunciado

Red con acceso a Internet, direccionamiento sin clase. Redes Alpha y Bravo conectadas por R0 y R1 a R2, que conecta a Rext. Configuraciones parciales dadas.

### Solución

#### a) Dirección IP y máscara de E1 y E2 de R2

De las configuraciones dadas:
- R0 (E1): 150.214.128.2/30 → red 150.214.128.0/30 (rango .0–.3)
- R1 (E1): 150.214.128.5/30 → red 150.214.128.4/30 (rango .4–.7)
- R2 (E0): 150.214.128.9/30 → red 150.214.128.8/30 (rango .8–.11)

R2 conecta con R0 y R1. R2 debe tener interfaces en las mismas redes /30 que R0 y R1:

- **R2 (E1):** conecta con R0 → red 150.214.128.0/30. R0 tiene .2, así que R2 tiene **150.214.128.1/30** (máscara 255.255.255.252).
- **R2 (E2):** conecta con R1 → red 150.214.128.4/30. R1 tiene .5, así que R2 tiene **150.214.128.6/30** (máscara 255.255.255.252).

R2 (E0) = 150.214.128.9/30 conecta con Rext. Rext (E1) = 190.100.100.2/30 → esta es la red hacia Internet, en red 190.100.100.0/30. Pero R2-E0 es .9/30 → red 150.214.128.8/30. Entonces Rext debe tener otra interfaz en esa red:

- **Rext (E0):** 150.214.128.10/30 (en la red 150.214.128.8/30 compartida con R2)

#### b) ¿Son correctas las siguientes direcciones?

**Red Alpha:** R0 (E0) = 150.214.0.1/23 → red 150.214.0.0/23 (rango .0.0 – .1.255)

**Red Bravo:** R1 (E0) = 150.214.2.1/23 → red 150.214.2.0/23 (rango .2.0 – .3.255)

**i) 150.214.0.0 con máscara 255.255.254.0 en Alpha:**
150.214.0.0 es la **dirección de red** de 150.214.0.0/23. No se puede asignar a un PC. **Incorrecta.**

**ii) 150.214.0.255 con máscara 255.255.254.0 en Alpha:**
Red Alpha = 150.214.0.0/23, broadcast = 150.214.1.255. La dirección 150.214.0.255 está dentro del rango asignable (150.214.0.1 – 150.214.1.254). **Correcta** (es una dirección válida dentro de la subred).

**iii) 150.214.2.5 con máscara 255.255.252.0 en Bravo:**
Máscara 255.255.252.0 = /22. Con esa máscara, la dirección 150.214.2.5 pertenecería a la red 150.214.0.0/22 (rango .0.0 – .3.255). Pero la red Bravo es 150.214.2.0/23, no /22. La **máscara es incorrecta** (no coincide con la configurada en la red Bravo). El PC tendría una configuración inconsistente con el resto de la red.

**iv) 150.214.1.2 con máscara 255.255.254.0 en Bravo:**
Con máscara /23, 150.214.1.2 pertenece a la red 150.214.0.0/23 (red Alpha, no Bravo). **Incorrecta** — esta IP pertenece a la red Alpha, no a Bravo (150.214.2.0/23).

#### c) Tablas de enrutamiento de R2 y Rext

**TE R2:**

| Red | Próximo salto | Interfaz |
|---|---|---|
| 150.214.128.0/30 | — | E1 |
| 150.214.128.4/30 | — | E2 |
| 150.214.128.8/30 | — | E0 |
| 150.214.0.0/23 | 150.214.128.2 | E1 |
| 150.214.2.0/23 | 150.214.128.5 | E2 |
| 0.0.0.0/0 | 150.214.128.10 | E0 |

Se podría agregar Alpha y Bravo como 150.214.0.0/22 si ambas se alcanzaran por la misma interfaz, pero no es el caso.

**TE Rext:**

| Red | Próximo salto | Interfaz |
|---|---|---|
| 150.214.128.8/30 | — | E0 |
| 150.214.0.0/22 | 150.214.128.9 | E0 |
| 190.100.100.0/30 | — | E1 |
| 0.0.0.0/0 | 190.100.100.1 | E1 |

Nota: Rext puede agregar las redes Alpha (150.214.0.0/23), Bravo (150.214.2.0/23) y los enlaces internos como **150.214.0.0/22** (o incluso un prefijo más general), ya que todo ese tráfico va hacia R2.

#### d) Sustituir R0, R1, R2 por un switch

Si se sustituyen los tres routers internos por un switch, Alpha y Bravo quedan en el **mismo dominio de broadcast** conectado directamente a Rext.

**Problema:** Alpha usa 150.214.0.0/23 y Bravo usa 150.214.2.0/23. Al estar ahora en el mismo dominio de broadcast, necesitan pertenecer a la **misma red lógica**.

Como no se pueden modificar las IPs de los nodos de Alpha y Bravo, la solución es:

- **Cambiar la máscara** de todos los nodos de Alpha y Bravo a **/22** (255.255.252.0), que engloba ambas redes (150.214.0.0/22 cubre 150.214.0.0 – 150.214.3.255).
- **Configurar la puerta de enlace** de todos los nodos apuntando a la interfaz de Rext en esa red.
- **Configurar Rext** con una interfaz en la red 150.214.0.0/22 (por ejemplo, 150.214.0.1/22 o 150.214.3.254/22).

De esta forma, todos los PCs de Alpha y Bravo están en la misma red lógica 150.214.0.0/22 y pueden comunicarse entre sí directamente y acceder a Internet a través de Rext.

---

## Problema 11

### Enunciado

Router conecta dominio de broadcast 1 (MTU 1500) y dominio 2 (MTU 1000). PcA (puerto 49789, cliente) en dominio 1. PcB (puerto 51345, servidor) en dominio 1 (apartado a) o dominio 2 (apartado b). UDP envía A_PDU de 1472 bytes.

### Solución

#### Datos previos

- A_PDU = 1472 bytes (datos de aplicación)
- UDP añade 8 bytes de cabecera → UDP_PDU = 1472 + 8 = **1480 bytes**
- IP añade 20 bytes (sin opciones) → IP_PDU = 1480 + 20 = **1500 bytes**

#### a) PcB en dominio de broadcast 1 (misma red que PcA)

La IP_PDU tiene 1500 bytes y la MTU del dominio 1 es 1500 bytes.

**1500 ≤ 1500** → No hay fragmentación. La IP_PDU cabe en una única trama.

- **IP en PcB recibe: 1 IP_PDU** (de 1500 bytes)
- **UDP en PcB recibe: 1 UDP_PDU** (de 1480 bytes)

La IP_PDU no pasa por el router (PcA y PcB están en el mismo dominio de broadcast), por lo que la MTU del dominio 2 es irrelevante.

#### b) PcB en dominio de broadcast 2

Ahora PcA está en dominio 1 y PcB en dominio 2. La IP_PDU debe atravesar el router.

**Tramo PcA → Router (dominio 1, MTU 1500):**

IP_PDU = 1500 bytes ≤ MTU 1500 → sin fragmentación. Llega al router íntegra.

**Tramo Router → PcB (dominio 2, MTU 1000):**

La IP_PDU de 1500 bytes > MTU 1000 → **hay que fragmentar.**

Datos de la IP_PDU original: IP_UD = 1500 − 20 = 1480 bytes.

Cada fragmento tiene cabecera IP de 20 bytes → espacio para datos = 1000 − 20 = **980 bytes**.

- **Fragmento 1:** 980 bytes de datos → total 1000 bytes. Quedan 1480 − 980 = 500 bytes.
- **Fragmento 2:** 500 bytes de datos → total 520 bytes (último fragmento).

Verificación: 980 + 500 = 1480 ✓

**IP en PcB recibe: 2 IP_PDUs** (fragmentos).

Tras el reensamblado en el nivel de red de PcB, se reconstruye la IP_PDU original y se extrae la UDP_PDU completa.

**UDP en PcB recibe: 1 UDP_PDU** (de 1480 bytes).

**Sí cambia la respuesta:** IP recibe 2 IP_PDUs en lugar de 1 (debido a la fragmentación en el router). Sin embargo, UDP sigue recibiendo 1 sola UDP_PDU, ya que el reensamblado ocurre en el nivel de red antes de entregar al nivel de transporte.

---

*Documento generado como material de estudio para Redes de Computadores — Tema 4: La Capa de Red.*