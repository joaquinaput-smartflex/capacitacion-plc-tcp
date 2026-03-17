# Capacitación TCP/IP – PLC Delta DVP12SE11R

**VALBOL – Válvulas Worcester de Argentina S.A.**

Curso de formación en comunicación TCP/IP para personal técnico, orientado al PLC Delta DVP12SE11R con ISPSoft.

---

## 📋 Nomenclatura de archivos

- **(TC)** = Tema Central — presentación principal del instructor
- **(GA)** = Guía de Apoyo — material complementario para alumnos o instructor

Los archivos se abren en orden secuencial: `00`, `01a`, `01b`, `01c`, `02a`, `02b`, etc.

---

## 📂 Inventario completo de archivos

### 00 – Generales
| Archivo | Tipo | Descripción |
|---------|------|-------------|
| `00_Presentacion_General(TC).pptx` | TC | Presentación general del curso completo (28 slides) |
| `00_Planificacion_16hs(GA).docx` | GA | Carga horaria, distribución en jornadas, material necesario |
| `00_Guia_Practica_General(GA).docx` | GA | Guía práctica general con ejercicios paso a paso |

### 01 – Sección 1: Redes + Repaso + Conexión (2.5 hs)
| Archivo | Tipo | Descripción |
|---------|------|-------------|
| `01a_Redes_Fisico_a_Logico(TC).pptx` | TC | Intro redes: medios físicos, modelo TCP/IP, 4 capas (15 slides, 30 min) |
| `01b_Redes_IP_MAC_DHCP_Switch_Router(TC).pptx` | TC | IP vs MAC, DHCP, ping/tracert, switch vs router (28 slides, 45 min) |
| `01c_Modulo1_Repaso_Conexion(TC).pptx` | TC | Repaso PLC, software, conexión Ethernet, prácticas (30 slides, 1:30 hs) |

### 02 – Sección 2: Modbus TCP (3.0 hs)
| Archivo | Tipo | Descripción |
|---------|------|-------------|
| `02a_Modulo2_Modbus_TCP(TC).pptx` | TC | Historia, teoría, mapeo, funciones, 6 ejercicios (27 slides) |
| `02b_Modulo2_Diagramas_Ladder(GA).pptx` | GA | Diagramas Ladder visuales para cada ejercicio (18 slides) |
| `02c_Modulo2_Guia_Practica_Alumnos(GA).docx` | GA | Guía para alumnos: ejercicios con Ladder, tablas de verificación |
| `02d_Modulo2_Cheatsheet_Modbus(GA).pdf` | GA | Referencia rápida 1 página: fórmula, registros, funciones |

### 03 – Sección 3: Socket TCP (1:30 hs)
| Archivo | Tipo | Descripción |
|---------|------|-------------|
| `03a_Modulo3_Socket_TCP(TC).pptx` | TC | Concepto socket, SOCKSND/SOCKRCV, 5 ejercicios (25 slides) |

### 04 – Sección 4: Webserver (1:00 hs)
| Archivo | Tipo | Descripción |
|---------|------|-------------|
| `04a_Modulo4_Webserver(TC).pptx` | TC | Webserver embebido, acceso, prácticas PC+celular (14 slides) |

### 05 – Sección 5: Email SMTP (1:30 hs)
| Archivo | Tipo | Descripción |
|---------|------|-------------|
| `05a_Modulo5_Email_SMTP(TC).pptx` | TC | SMTP, Python, pymodbus, 5 ejercicios graduales (20 slides) |

### 06 – Sección 6: PLC a PLC + Integrador (1:30 hs)
| Archivo | Tipo | Descripción |
|---------|------|-------------|
| `06a_Modulo6_PLC_a_PLC_Integrador(TC).pptx` | TC | Data Exchange, ejercicio integrador, evaluación final (19 slides) |

---

## 🗺️ Roadmap

Ver **[ROADMAP.md](ROADMAP.md)** para el estado detallado de cada sección y próximos pasos.

### Progreso actual:
```
Sección 1: Repaso + Conexión     ████████████████████ 100% ✅
Sección 2: Modbus TCP            ████████████████████ 100% ✅
Sección 3: Socket TCP            ████████████████████ 100% ✅ PPT listo
Sección 4: Webserver             ████████████████████ 100% ✅ PPT listo
Sección 5: Email SMTP            ████████████████████ 100% ✅ PPT listo
Sección 6: PLC a PLC + Cierre    ████████████████████ 100% ✅ PPT listo
```

---

## 🔧 Equipamiento necesario

- PLC Delta DVP12SE11R con fuente 24VDC
- Cable Ethernet RJ45 + Switch o router WiFi
- PC con ISPSoft + COMMGR + DCISoft (gratuitos, descarga de Delta)
- ModbusClientX o Hercules (para test de comunicación)
- Python 3.x con pymodbus (para módulo de email)

---

## 👥 Perfil del alumno

- Curso básico de PLC aprobado (Ladder, temporizadores, contadores)
- No se requiere experiencia en comunicaciones industriales
- No se requiere experiencia en Python

---

## 📄 Licencia

Material de uso interno VALBOL. Distribuido como recurso educativo.
