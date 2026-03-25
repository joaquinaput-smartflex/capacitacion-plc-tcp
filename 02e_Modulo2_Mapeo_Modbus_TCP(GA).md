# Mapeo Modbus TCP — Programa PLC Delta DVP12SE11R

## Referencia para uso con Modbus Poll / SCADA / HMI

Este documento detalla todas las señales del programa PLC (`ips texto.txt`) accesibles via Modbus TCP, organizadas por función.

---

## Formulas de Conversion Delta DVP → Modbus

| Dispositivo | Tipo | Function Code Lectura | Function Code Escritura | Base (hex) | Base (dec) | Formula |
|---|---|---|---|---|---|---|
| **X** (entradas) | Bit | 02 Read Discrete Inputs | — (solo lectura) | 0x0400 | 1024 | `1024 + N` |
| **Y** (salidas) | Bit | 01 Read Coils | 05 Write Single Coil | 0x0500 | 1280 | `1280 + N` |
| **T** (timer contacto) | Bit | 01 Read Coils | — | 0x0600 | 1536 | `1536 + N` |
| **M** (reles internos) | Bit | 01 Read Coils | 05 Write Single Coil | 0x0800 | 2048 | `2048 + N` |
| **C** (counter contacto) | Bit | 01 Read Coils | — | 0x0E00 | 3584 | `3584 + N` |
| **T** (timer valor actual) | 16-bit | 03 Read Holding Registers | — | 0x0600 | 1536 | `1536 + N` |
| **C** (counter valor actual) | 16-bit | 03 Read Holding Registers | — | 0x0E00 | 3584 | `3584 + N` |
| **D** (registros datos) | 16-bit | 03 Read Holding Registers | 06 Write Single Register | 0x1000 | 4096 | `4096 + N` |

> **Ejemplo:** Para leer M21 → Modbus address = 2048 + 21 = **2069**, FC 01.
> **Ejemplo:** Para leer D100 → Modbus address = 4096 + 100 = **4196**, FC 03.

---

## 1. Interfaz SCADA/HMI (D100–D112) — LECTURA PRINCIPAL

Estos 13 registros contiguos concentran toda la informacion del sistema. Se leen con un solo request Modbus.

**Modbus Poll: FC 03, Address 4196, Quantity 13**

| Registro PLC | Modbus Addr (dec) | Modbus Addr (hex) | Descripcion | Valores |
|---|---|---|---|---|
| D100 | 4196 | 0x1064 | Valor comunicacion general | Entero del modulo comm |
| D101 | 4197 | 0x1065 | Sensor 1 — valor actual | Valor raw del sensor |
| D102 | 4198 | 0x1066 | Sensor 2 — valor actual | Valor raw del sensor |
| D103 | 4199 | 0x1067 | Sensor 3 — valor actual | Valor raw del sensor |
| D104 | 4200 | 0x1068 | Alarma BAJA (2-de-3 sensores) | 0=Normal, 1=Alarma |
| D105 | 4201 | 0x1069 | Alarma ALTA (2-de-3 sensores) | 0=Normal, 1=Alarma |
| D106 | 4202 | 0x106A | Alarma CAMBIO BRUSCO | 0=Normal, 1=Alarma |
| D107 | 4203 | 0x106B | Estado salida canal 1 (temperatura) | 0=Idle, 1=Activo, 2=Manual+, 3=Manual- |
| D108 | 4204 | 0x106C | Estado salida canal 2 (temperatura) | 0=Idle, 1=Activo, 2=Manual+, 3=Manual- |
| D109 | 4205 | 0x106D | Estado salida canal 3 (corriente) | 0=Idle, 1=Activo, 2=Manual+, 3=Manual- |
| D110 | 4206 | 0x106E | Estado salida canal 4 (corriente) | 0=Idle, 1=Activo, 2=Manual+, 3=Manual- |
| D111 | 4207 | 0x106F | Estado rele Y1 | 0=OFF, 1=ON |
| D112 | 4208 | 0x1070 | Estado rele Y2 | 0=OFF, 1=ON |

**Origen de cada registro en el programa:**
```
MOV D7001 D100    ← registro de comunicacion
MOV D7011 D101    ← sensor 1 raw
MOV D7021 D102    ← sensor 2 raw
MOV D7031 D103    ← sensor 3 raw
M2 → D104         ← flag alarma baja (votacion 2-de-3)
M3 → D105         ← flag alarma alta (votacion 2-de-3)
M4 → D106         ← flag cambio brusco (diferencial > umbral)
Y1 → D111         ← estado fisico salida Y1
Y2 → D112         ← estado fisico salida Y2
```

---

## 2. Parametros Configurables (D0–D5, D7100–D7101) — LECTURA/ESCRITURA

Estos registros se pueden modificar remotamente para ajustar el comportamiento del sistema.

**Modbus Poll: FC 03, Address 4096, Quantity 6** (D0–D5)

| Registro PLC | Modbus Addr | Descripcion | Uso | Rango tipico |
|---|---|---|---|---|
| D0 | 4096 | Codigo/password 1 | Ingresado por botones (C30-C32) | 0–999 |
| D1 | 4097 | Codigo/password 2 | Ingresado por botones (C40-C42) | 0–999 |
| D2 | 4098 | Umbral cambio brusco | Diferencial maximo permitido entre muestras | Segun sensor |
| D3 | 4099 | Periodo de muestreo | Base de tiempo para timers T50/T52 (x100ms) | 1–600 |
| D4 | 4100 | Limite inferior comparacion | Umbral bajo para D7001 | Segun sensor |
| D5 | 4101 | Limite superior comparacion | Umbral alto para D7001 | Segun sensor |

**Modbus Poll: FC 03, Address 11196, Quantity 2** (D7100–D7101)

| Registro PLC | Modbus Addr | Descripcion | Uso |
|---|---|---|---|
| D7100 | 11196 | Limite MAXIMO sensor (alarma alta) | Si sensor >= D7100 → alarma individual |
| D7101 | 11197 | Limite MINIMO sensor (alarma baja) | Si sensor <= D7101 → alarma individual |

> **Escritura remota:** Usar FC 06 (Write Single Register) o FC 16 (Write Multiple) para cambiar parametros sin reprogramar el PLC.

---

## 3. Entradas Fisicas (X)

**Modbus Poll: FC 02, Address 1024, Quantity 5**

| Registro PLC | Modbus Addr | Descripcion |
|---|---|---|
| X0 | 1024 | Entrada digital 0 (no usada en programa) |
| X1 | 1025 | Entrada digital 1 (habilitacion / flag) |
| X2 | 1026 | Boton digito UNIDADES |
| X3 | 1027 | Boton digito DECENAS / Accion bajar |
| X4 | 1028 | Boton digito CENTENAS / Accion subir / Confirmacion |

---

## 4. Salidas Fisicas (Y)

**Modbus Poll: FC 01, Address 1280, Quantity 3**

| Registro PLC | Modbus Addr | Descripcion |
|---|---|---|
| Y0 | 1280 | Salida rele 0 (no usada en programa) |
| Y1 | 1281 | Salida rele 1 |
| Y2 | 1282 | Salida rele 2 |

---

## 5. Alarmas y Flags de Estado (M)

### Alarmas principales
**Modbus Poll: FC 01, Address 2048, Quantity 6**

| Registro PLC | Modbus Addr | Descripcion | Condicion de activacion |
|---|---|---|---|
| M0 | 2048 | Habilitacion general sistema | ON = sistema activo |
| M2 | 2050 | ALARMA BAJA (votacion 2-de-3) | Al menos 2 sensores <= D7101 |
| M3 | 2051 | ALARMA ALTA (votacion 2-de-3) | Al menos 2 sensores >= D7100 |
| M4 | 2052 | ALARMA CAMBIO BRUSCO | Diferencial sensor > D2 (umbral) |
| M5 | 2053 | Flag sistema (reset general) | Reset de subsistemas |

### Alarmas individuales por sensor (bajo minimo)
**Modbus Poll: FC 01, Address 2159, Quantity 3**

| Registro PLC | Modbus Addr | Descripcion |
|---|---|---|
| M111 | 2159 | Sensor 1 bajo minimo (D7011 <= D7101) |
| M112 | 2160 | Sensor 2 bajo minimo (D7021 <= D7101) |
| M113 | 2161 | Sensor 3 bajo minimo (D7031 <= D7101) |

### Alarmas individuales por sensor (sobre maximo)
**Modbus Poll: FC 01, Address 2169, Quantity 3**

| Registro PLC | Modbus Addr | Descripcion |
|---|---|---|
| M121 | 2169 | Sensor 1 sobre maximo (D7011 >= D7100) |
| M122 | 2170 | Sensor 2 sobre maximo (D7021 >= D7100) |
| M123 | 2171 | Sensor 3 sobre maximo (D7031 >= D7100) |

### Control e interlock
**Modbus Poll: FC 01, Address 2076, Quantity 1** (M28)

| Registro PLC | Modbus Addr | Descripcion |
|---|---|---|
| M28 | 2076 | INTERLOCK maestro — inhibe alarma M4 |
| M100 | 2148 | Inicializacion del sistema completada |

### Acciones de salida
| Registro PLC | Modbus Addr | Descripcion |
|---|---|---|
| M131 | 2179 | Accion SUBIR — canal corriente |
| M132 | 2180 | Accion BAJAR — canal corriente |
| M137 | 2185 | Salida combinada A (subir + estados) |
| M138 | 2186 | Salida combinada B (bajar + estados) |
| M141 | 2189 | Accion SUBIR — canal temperatura |
| M142 | 2190 | Accion BAJAR — canal temperatura |
| M147 | 2195 | Salida combinada C |
| M148 | 2196 | Salida combinada D |

### Deteccion de codigos duplicados
| Registro PLC | Modbus Addr | Descripcion |
|---|---|---|
| M200 | 2248 | Codigo 1 = Codigo 2 |
| M201 | 2249 | Codigo 1 = Codigo 3 |
| M202 | 2250 | Codigo 2 = Codigo 3 |
| M203 | 2251 | BLOQUEO — al menos 2 codigos son iguales |

---

## 6. Maquina de Estados (24 pasos)

### Registro de estado (valor numerico)
**Modbus Poll: FC 03, Address 4256, Quantity 1**

| Registro PLC | Modbus Addr | Descripcion | Valores |
|---|---|---|---|
| D160 | 4256 | Estado actual de la maquina | 1–24 |

### Flags individuales por paso
**Modbus Poll: FC 01, Address 2218, Quantity 24**

| Registro PLC | Modbus Addr | Paso | Registro PLC | Modbus Addr | Paso |
|---|---|---|---|---|---|
| M170 | 2218 | 1 | M182 | 2230 | 13 |
| M171 | 2219 | 2 | M183 | 2231 | 14 |
| M172 | 2220 | 3 | M184 | 2232 | 15 |
| M173 | 2221 | 4 | M185 | 2233 | 16 |
| M174 | 2222 | 5 | M186 | 2234 | 17 |
| M175 | 2223 | 6 | M187 | 2235 | 18 |
| M176 | 2224 | 7 | M188 | 2236 | 19 |
| M177 | 2225 | 8 | M189 | 2237 | 20 |
| M178 | 2226 | 9 | M190 | 2238 | 21 |
| M179 | 2227 | 10 | M191 | 2239 | 22 |
| M180 | 2228 | 11 | M192 | 2240 | 23 |
| M181 | 2229 | 12 | — | — | 24 (C65 reset) |

---

## 7. Valores Internos de Calculo (D)

### Sensores — muestras y diferenciales
**Modbus Poll: FC 03, Address 11107, Quantity 28**

| Registro PLC | Modbus Addr | Descripcion |
|---|---|---|
| **Sensor 1** | | |
| D7011 | 11107 | Valor actual (raw) |
| D7012 | 11108 | Muestra T1 — muestreo corto |
| D7013 | 11109 | Muestra T2 — muestreo largo |
| D7016 | 11112 | Diferencial absoluto (T2 - T1) |
| **Sensor 2** | | |
| D7021 | 11117 | Valor actual (raw) |
| D7022 | 11118 | Muestra T1 — muestreo corto |
| D7023 | 11119 | Muestra T2 — muestreo largo |
| D7024 | 11120 | Diferencial absoluto (T2 - T1) |
| **Sensor 3** | | |
| D7031 | 11127 | Valor actual (raw) |
| D7032 | 11128 | Muestra T1 — muestreo corto |
| D7033 | 11129 | Muestra T2 — muestreo largo |
| D7034 | 11130 | Diferencial absoluto (T2 - T1) |

### Otros valores internos
| Registro PLC | Modbus Addr | Descripcion |
|---|---|---|
| D7001 | 11097 | Valor comunicacion (origen de D100) |
| D7104 | 11200 | Copia de D4 (limite inferior) |
| D7105 | 11201 | Copia de D5 (limite superior) |
| D7110 | 11206 | Periodo muestreo x2 (D3 × K2) |

---

## 8. Contadores — Valores Actuales

**Modbus Poll: FC 03 (holding register)**

### Codigos de acceso (3 niveles)
| Registro PLC | Modbus Addr | Descripcion |
|---|---|---|
| C70 | 3654 | Codigo nivel 1 — digito centenas |
| C71 | 3655 | Codigo nivel 1 — digito decenas |
| C72 | 3656 | Codigo nivel 1 — digito unidades |
| C80 | 3664 | Codigo nivel 2 — digito centenas |
| C81 | 3665 | Codigo nivel 2 — digito decenas |
| C82 | 3666 | Codigo nivel 2 — digito unidades |
| C90 | 3674 | Codigo nivel 3 — digito centenas |
| C91 | 3675 | Codigo nivel 3 — digito decenas |
| C92 | 3676 | Codigo nivel 3 — digito unidades |

### Ingreso de parametros
| Registro PLC | Modbus Addr | Descripcion |
|---|---|---|
| C20 | 3604 | Ingreso D5 — digito centenas |
| C21 | 3605 | Ingreso D5 — digito decenas |
| C22 | 3606 | Ingreso D5 — digito unidades |
| C10 | 3594 | Ingreso D4 — digito centenas |
| C11 | 3595 | Ingreso D4 — digito decenas |
| C12 | 3596 | Ingreso D4 — digito unidades |

### Contadores de retencion
| Registro PLC | Modbus Addr | Descripcion |
|---|---|---|
| C5 | 3589 | Contador pulsos canal corriente (subir) |
| C6 | 3590 | Contador pulsos canal corriente (bajar) |
| C15 | 3599 | Contador pulsos canal temperatura (subir) |
| C16 | 3600 | Contador pulsos canal temperatura (bajar) |
| C65 | 3649 | Contador maquina de estados (0–24) |

---

## Resumen — Configuracion Rapida en Modbus Poll

### Ventanas recomendadas

| # | Function Code | Address | Quantity | Que se monitorea |
|---|---|---|---|---|
| 1 | **03** Read Holding Registers | **4196** | **13** | **Interfaz SCADA completa** (D100–D112) |
| 2 | **03** Read Holding Registers | **4096** | **6** | Parametros configurables (D0–D5) |
| 3 | **01** Read Coils | **2050** | **4** | Alarmas principales (M2, M3, M4, M5) |
| 4 | **02** Read Discrete Inputs | **1024** | **5** | Entradas fisicas (X0–X4) |
| 5 | **01** Read Coils | **1280** | **3** | Salidas fisicas (Y0–Y2) |
| 6 | **03** Read Holding Registers | **4256** | **1** | Estado maquina (D160) |
| 7 | **01** Read Coils | **2218** | **24** | Flags maquina de estados (M170–M192) |
| 8 | **03** Read Holding Registers | **11107** | **24** | Sensores raw + diferenciales |
| 9 | **01** Read Coils | **2159** | **3** | Alarmas individuales bajo (M111–M113) |
| 10 | **01** Read Coils | **2169** | **3** | Alarmas individuales alto (M121–M123) |

### Conexion en Modbus Poll

```
Connection → Connect:
  ├── Connection Type: TCP/IP
  ├── Remote Server IP: [IP del PLC]  (ej: 192.168.1.1)
  ├── Port: 502
  └── Unit ID / Slave: 1

Setup → Read/Write Definition (F8):
  ├── Slave ID: 1
  ├── Function: [segun tabla arriba]
  ├── Address: [segun tabla arriba]
  ├── Quantity: [segun tabla arriba]
  └── Scan Rate: 1000 ms
```

---

## Diagrama de Flujo de Datos

```
  SENSORES (D7011/D7021/D7031)
       │
       ▼
  ┌─────────────────────┐
  │  MUESTREO TEMPORAL  │  T50/T52 (corto)  T53/T54 (largo)
  │  Muestra T1 → D70x2 │
  │  Muestra T2 → D70x3 │
  └─────────┬───────────┘
            │
            ▼
  ┌─────────────────────┐
  │  DIFERENCIAL (ABS)  │  D7016 = |D7013 - D7012|
  │  Compara vs D2      │  D7024 = |D7023 - D7022|
  │  (umbral)           │  D7034 = |D7033 - D7032|
  └─────────┬───────────┘
            │
            ▼
  ┌─────────────────────┐     ┌──────────────────┐
  │  VOTACION 2-DE-3    │     │  ALARMA CAMBIO   │
  │  Sensores vs        │     │  BRUSCO           │
  │  D7100 (max)        │     │  Diferencial > D2 │
  │  D7101 (min)        │     │  → M4 (D106)      │
  │  → M2 baja (D104)   │     └──────────────────┘
  │  → M3 alta (D105)   │
  └─────────┬───────────┘
            │
            ▼
  ┌─────────────────────┐
  │  INTERFAZ MODBUS    │  D100–D112
  │  (13 registros)     │  Address: 4196
  │  FC 03              │  Quantity: 13
  └─────────────────────┘
```

---

*Documento generado para el curso de Capacitacion TCP/IP — VALBOL*
*PLC: Delta DVP12SE11R | Programa: ips texto.txt*
