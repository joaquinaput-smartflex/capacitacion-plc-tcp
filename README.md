# Capacitación TCP/IP – PLC Delta DVP12SE11R

**VALBOL – Válvulas Worcester de Argentina S.A.**

Curso de formación en comunicación TCP/IP para personal técnico, orientado al PLC Delta DVP12SE11R con ISPSoft.

---

## 📋 Contenido del curso (16 horas)

### Materiales por módulo

| Módulo | Presentación | Material alumno | Duración |
|--------|-------------|-----------------|----------|
| **Intro** | `Redes_Fisico_a_Logico.pptx` | — | 30 min |
| **Módulo 1** | `Modulo1_Repaso_Conexion.pptx` | — | 1:30 hs |
| **Módulo 2** | `Modulo2_Modbus_TCP.pptx` | `Guia_Practica_Modulo2_Modbus.docx` | 3:00 hs |
| | `Modulo2_Ladder_Modbus.pptx` | `Cheatsheet_Modbus_TCP.pdf` | |
| | `Modulo2_Redes_Profundidad.pptx` | — | 30 min (Bloque A2) |

### Materiales generales

| Archivo | Descripción |
|---------|-------------|
| `Capacitacion_TCP_DVP12SE.pptx` | Presentación general del curso completo (28 slides) |
| `Guia_Practica_TCP_DVP12SE.docx` | Guía práctica general con ejercicios paso a paso |
| `Resumen_Capacitacion_TCP_16hs.docx` | Planificación de 16 hs, distribución en jornadas, material necesario |
| `ROADMAP.md` | Estado del proyecto, próximos pasos, entregables pendientes |

---

## 📂 Detalle por sección

### Sección 1: Repaso + Conexión (2.0 hs) ✅

| Archivo | Tipo | Contenido |
|---------|------|-----------|
| `Redes_Fisico_a_Logico.pptx` | PPT instructor (15 slides) | Medios físicos, modelo TCP/IP 4 capas, diagnóstico por capas |
| `Modulo1_Repaso_Conexion.pptx` | PPT instructor (30 slides) | Qué es un PLC, DVP12SE, Ladder, registros, ISPSoft/COMMGR/DCISoft, conexión Ethernet, 3 prácticas, evaluación |

### Sección 2: Modbus TCP (3.0 hs) ✅

| Archivo | Tipo | Contenido |
|---------|------|-----------|
| `Modulo2_Modbus_TCP.pptx` | PPT instructor (27 slides) | Historia 1979, Modicon, timeline, Master/Slave, modelo de datos, mapeo, funciones, trama TCP, 6 ejercicios |
| `Modulo2_Ladder_Modbus.pptx` | PPT instructor (18 slides) | Diagramas Ladder visuales para cada ejercicio (rieles, contactos, ramas OR, contactos NC) |
| `Modulo2_Redes_Profundidad.pptx` | PPT instructor (13 slides) | IP vs MAC, DHCP vs estática, ping, tracert, ipconfig, switch vs router, diagrama de red |
| | `Modulo2_Redes_Profundidad.pptx` | — | 30 min (Bloque A2) |
| `Guia_Practica_Modulo2_Modbus.docx` | Word para alumnos | 6 ejercicios con código Ladder completo, diagramas visuales, tablas de verificación |
| `Cheatsheet_Modbus_TCP.pdf` | PDF 1 página (imprimir) | Fórmula de mapeo, registros, funciones, config DVP12SE, diagnóstico, credenciales |

---

## 🎯 Temas cubiertos

- **Redes industriales**: medios físicos, capas TCP/IP, diagnóstico por capas
- **Modbus TCP**: historia, arquitectura Master/Slave, mapeo de registros, funciones H03/H06/H10
- **Socket TCP**: instrucciones SOCKSND/SOCKRCV para comunicación en formato libre
- **Webserver embebido**: monitoreo del PLC desde navegador web
- **Email SMTP**: notificaciones y alarmas por correo electrónico
- **PLC a PLC**: Data Exchange con DCISoft

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

## 🗺️ Roadmap

Ver **[ROADMAP.md](ROADMAP.md)** para el estado detallado de cada sección y próximos pasos.

### Progreso actual:
```
Sección 1: Repaso + Conexión     ████████████████████ 100% ✅
Sección 2: Modbus TCP            ████████████████████ 100% ✅
Sección 3: Socket TCP            ░░░░░░░░░░░░░░░░░░░░   0% ⬜
Sección 4: Webserver             ░░░░░░░░░░░░░░░░░░░░   0% ⬜
Sección 5: Email SMTP            ░░░░░░░░░░░░░░░░░░░░   0% ⬜
Sección 6: PLC a PLC + Cierre    ░░░░░░░░░░░░░░░░░░░░   0% ⬜
```

---

## 📄 Licencia

Material de uso interno VALBOL. Distribuido como recurso educativo.
