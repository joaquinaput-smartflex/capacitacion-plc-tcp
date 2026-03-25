# Control de Cambios: Programa PLC v1 → v2

## Resumen

| ID | Tipo | Severidad | Descripcion corta |
|---|---|---|---|
| FIX-1 | Bug | **CRITICA** | Typo en comparacion de codigos duplicados (M200) |
| FIX-2 | Bug | **ALTA** | Triple OUT M4 — solo el ultimo sensor decidia |
| FIX-3 | Bug | BAJA | C50 K6 limita umbral D2 a max 599 |

---

## FIX-1: Typo en comparacion de codigos (M200)

### Ubicacion
- **Archivo original**: `ips texto.txt`, linea **363**
- **Archivo corregido**: `ips texto v2.txt`, seccion "DETECCION DE CODIGOS DUPLICADOS"

### Que hace este bloque
Compara los 3 digitos del **Codigo 1** (C70, C71, C72) contra los 3 digitos del **Codigo 2** (C80, C81, C82) para detectar si son iguales. Si M200 = ON, significa "Codigo 1 = Codigo 2".

### El error
Se comparaba C72 (unidades del Codigo 1) contra **C81** (decenas del Codigo 2), en vez de **C82** (unidades del Codigo 2).

### Codigo ORIGINAL (con error)

```
361  LD= C70 C80        ; centenas: Codigo 1 vs Codigo 2  ✔
362  AND= C71 C81       ; decenas:  Codigo 1 vs Codigo 2  ✔
363  AND= C72 C81       ; ← ERROR: compara unidades con DECENAS
364  OUT M200
```

### Codigo CORREGIDO

```
     LD= C70 C80        ; centenas: Codigo 1 vs Codigo 2  ✔
     AND= C71 C81       ; decenas:  Codigo 1 vs Codigo 2  ✔
     AND= C72 C82       ; unidades: Codigo 1 vs Codigo 2  ✔  ← CAMBIO
     OUT M200
```

### Cambio a realizar
```
CAMBIAR:  AND= C72 C81
POR:      AND= C72 C82
                   ^^^
                   C81 → C82
```

### Ejemplo del problema

Supongamos:
- Codigo 1 = **352** (C70=3, C71=5, C72=2)
- Codigo 2 = **325** (C80=3, C81=2, C82=5)

**Con el error (v1):**
```
C70=3 vs C80=3 → TRUE
C71=5 vs C81=2 → FALSE
C72=2 vs C81=2 → TRUE    ← Compara con decenas del Codigo 2!
Resultado: FALSE (por suerte, pero por la razon equivocada)
```

**Caso peor — falso positivo:**
- Codigo 1 = **322** (C70=3, C71=2, C72=2)
- Codigo 2 = **321** (C80=3, C81=2, C82=1)

```
C70=3 vs C80=3 → TRUE
C71=2 vs C81=2 → TRUE
C72=2 vs C81=2 → TRUE    ← Compara unidades con decenas!
Resultado: TRUE → M200=ON → M203=ON → BLOQUEO
```
Los codigos son **distintos** (322 ≠ 321) pero el sistema los detecta como **iguales**. El operario queda bloqueado sin razon.

**Con la correccion (v2):**
```
C70=3 vs C80=3 → TRUE
C71=2 vs C81=2 → TRUE
C72=2 vs C82=1 → FALSE   ← Ahora compara unidades con unidades
Resultado: FALSE → correcto, los codigos son distintos
```

### Impacto
M200 alimenta a M203 (bloqueo por codigos duplicados). Con el error, M203 puede activarse incorrectamente, **bloqueando el acceso al sistema** o permitiendo codigos duplicados que deberian estar prohibidos.

---

## FIX-2: Triple OUT M4 (alarma cambio brusco)

### Ubicacion
- **Archivo original**: `ips texto.txt`, lineas **68–85**
- **Archivo corregido**: `ips texto v2.txt`, seccion "ALARMA CAMBIO BRUSCO"

### El error
Tres bloques separados escriben al mismo coil M4. En cada scan del PLC los tres se ejecutan secuencialmente, y **el ultimo bloque sobreescribe el resultado** de los anteriores.

### Codigo ORIGINAL (con error)

```
; --- Bloque 1: Sensor 1 ---
68   LD>= D7016 D2       ; diferencial sensor 1 >= umbral?
69   LD M4               ; auto-latch
70   ANI M43             ; reset condition
71   ORB
72   ANI M28             ; interlock
73   OUT M4              ; ← escribe M4

; --- Bloque 2: Sensor 2 ---
74   LD>= D7024 D2       ; diferencial sensor 2 >= umbral?
75   LD M4
76   ANI M43
77   ORB
78   ANI M28
79   OUT M4              ; ← SOBREESCRIBE M4

; --- Bloque 3: Sensor 3 ---
80   LD>= D7034 D2       ; diferencial sensor 3 >= umbral?
81   LD M4
82   ANI M43
83   ORB
84   ANI M28
85   OUT M4              ; ← SOBREESCRIBE M4 (ESTA GANA)
```

### Codigo CORREGIDO

```
; Las 3 condiciones se combinan con OR ANTES del unico OUT M4

     LD>= D7016 D2       ; sensor 1 diferencial >= umbral?
     LD>= D7024 D2       ; sensor 2 diferencial >= umbral?
     ORB                  ; sensor 1 OR sensor 2
     LD>= D7034 D2       ; sensor 3 diferencial >= umbral?
     ORB                  ; (sensor 1 OR sensor 2) OR sensor 3
     LD M4               ; auto-latch
     ANI M43             ; reset condition
     ORB                 ; condicion nueva OR latch
     ANI M28             ; interlock
     OUT M4              ; ← UNICO OUT M4
```

### Cambio a realizar

**BORRAR** las 18 lineas originales (68-85) y **REEMPLAZAR** por estas 10 lineas:

```
LD>= D7016 D2
LD>= D7024 D2
ORB
LD>= D7034 D2
ORB
LD M4
ANI M43
ORB
ANI M28
OUT M4
```

### Ejemplo del problema

Escenario: M43=ON (reset activo), sensor 1 detecta cambio brusco, sensores 2 y 3 no.

**Con el error (v1):**
```
Bloque 1: D7016>=D2 (TRUE) OR (M4 AND NOT M43=FALSE) = TRUE, NOT M28 → M4=ON
Bloque 2: D7024<D2 (FALSE) OR (M4=ON AND NOT M43=FALSE) = FALSE → M4=OFF  ← pierde!
Bloque 3: D7034<D2 (FALSE) OR (M4=OFF AND NOT M43=FALSE) = FALSE → M4=OFF

Resultado: M4=OFF → ALARMA NO SUENA a pesar de que el sensor 1 detecto cambio brusco
```

**Con la correccion (v2):**
```
D7016>=D2 (TRUE) OR D7024<D2 (FALSE) OR D7034<D2 (FALSE) = TRUE
TRUE OR (M4 AND NOT M43) = TRUE
TRUE AND NOT M28 = TRUE → M4=ON

Resultado: M4=ON → ALARMA SUENA correctamente
```

### Impacto
Sin la correccion, la alarma de cambio brusco puede **no sonar** cuando solo 1 o 2 sensores (que no sean el ultimo) detectan un cambio. Esto es un riesgo de seguridad en un sistema de monitoreo industrial.

### Regla de oro en PLC
> **Un coil, un OUT por scan.** Si necesitas multiples condiciones para el mismo coil, combinalas con OR/ORB antes del unico OUT.

---

## FIX-3: Rango del contador C50 (umbral D2)

### Ubicacion
- **Archivo original**: `ips texto.txt`, linea **545**
- **Archivo corregido**: `ips texto v2.txt`, seccion "INGRESO DE PARAMETRO D2"

### El error
C50 cuenta el digito de centenas del parametro D2 (umbral de cambio brusco). Usa K6 como limite, mientras que todos los demas contadores de centenas usan K10 o K2.

### Codigo ORIGINAL

```
542  LD X4
543  AND M184
544  AND M0
545  CNT C50 K6          ; ← rango 0-5 (max centenas = 5)
```

### Codigo CORREGIDO

```
     LD X4
     AND M184
     AND M0
     CNT C50 K10          ; ← rango 0-9 (max centenas = 9)
                ^^^
                K6 → K10
```

### Cambio a realizar
```
CAMBIAR:  CNT C50 K6
POR:      CNT C50 K10
```

### Impacto
- **Con K6**: D2 puede valer entre 0 y 599
- **Con K10**: D2 puede valer entre 0 y 999

Si el umbral necesario es mayor a 599, el operario no podria configurarlo con la version original.

> **Nota**: Si K6 fue intencional (el umbral nunca deberia superar 599), revertir este cambio.

---

## Tabla resumen de cambios

| # | Linea original | Instruccion original | Instruccion corregida | Tipo de cambio |
|---|---|---|---|---|
| FIX-1 | 363 | `AND= C72 C81` | `AND= C72 C82` | Cambiar 1 operando |
| FIX-2 | 68–85 | 18 lineas (3 bloques OUT M4) | 10 lineas (1 bloque OR + OUT M4) | Borrar y reemplazar |
| FIX-3 | 545 | `CNT C50 K6` | `CNT C50 K10` | Cambiar 1 operando |

### Orden sugerido para aplicar los cambios

1. **Empezar por FIX-1** (linea 363) — es un solo caracter, facil y rapido
2. **Seguir con FIX-3** (linea 545) — tambien es un solo cambio
3. **Terminar con FIX-2** (lineas 68-85) — requiere borrar y reescribir un bloque

> **Tip**: Despues de cada cambio, compilar (F7) para verificar que no se introdujeron errores nuevos.

---

## Verificacion de los cambios

### Test FIX-1 (codigos duplicados)
1. Cargar codigos: Codigo 1 = **321**, Codigo 2 = **322**
2. Verificar: M200 debe ser **OFF** (codigos distintos)
3. Cargar codigos: Codigo 1 = **321**, Codigo 2 = **321**
4. Verificar: M200 debe ser **ON** (codigos iguales)

### Test FIX-2 (alarma cambio brusco)
1. Configurar D2 = 50 (umbral)
2. Forzar D7016 = 100 (sensor 1 diferencial alto)
3. Dejar D7024 = 0, D7034 = 0
4. Verificar: M4 debe ser **ON** (antes del fix, quedaba OFF)

### Test FIX-3 (rango D2)
1. Ir al paso 15 (M184)
2. Intentar ingresar D2 = 700
3. Verificar: el valor se acepta correctamente (antes del fix, max era 599)

---

*Control de cambios para Capacitacion TCP/IP — VALBOL*
*Archivos: `ips texto.txt` (v1) → `ips texto v2.txt` (v2)*
