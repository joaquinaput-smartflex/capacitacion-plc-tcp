# 🗺️ ROADMAP – Capacitación TCP/IP PLC Delta DVP12SE11R

**Estado general del proyecto:** 🟡 En desarrollo (Sección 1 completada)

---

## 📊 Progreso por sección

```
Sección 1 ████████████████████ 100%  ✅ COMPLETADA
Sección 2 ████████████████████ 100%  ✅ COMPLETADA
Sección 3 ░░░░░░░░░░░░░░░░░░░░   0%  ⬜ Pendiente
Sección 4 ░░░░░░░░░░░░░░░░░░░░   0%  ⬜ Pendiente
Sección 5 ░░░░░░░░░░░░░░░░░░░░   0%  ⬜ Pendiente
Sección 6 ░░░░░░░░░░░░░░░░░░░░   0%  ⬜ Pendiente
```

---

## Sección 1: Repaso + Conexión (2.0 hs) ✅ COMPLETADA

| Entregable | Estado | Archivo |
|------------|--------|---------|
| PPT Intro Redes (30 min) | ✅ Listo | `Redes_Fisico_a_Logico.pptx` |
| PPT Módulo 1 (1:30 hs, 30 slides) | ✅ Listo | `Modulo1_Repaso_Conexion.pptx` |
| Bloque A: Repaso PLC | ✅ Completo | Qué es un PLC, DVP12SE, Ladder, registros, estados |
| Bloque B: Software | ✅ Completo | ISPSoft, COMMGR, DCISoft |
| Bloque C: Conexión | ✅ Completo | USB/Ethernet/RS-485, config IP, COMMGR, credenciales |
| Bloque D: Prácticas | ✅ Completo | 3 ejercicios: conectar, monitorear, cambiar IP |
| Bloque E: Evaluación | ✅ Completo | 8 preguntas + autoevaluación |

---

## Sección 2: Modbus TCP (3.0 hs) ✅ COMPLETADA

| Entregable | Estado | Archivo |
|------------|--------|---------|
| PPT Módulo 2 (3 hs, 27 slides) | ✅ Listo | `Modulo2_Modbus_TCP.pptx` |
| Bloque A: Historia Modbus | ✅ Completo | Origen 1979, Modicon, timeline, por qué es popular |
| Bloque B: Teoría | ✅ Completo | Master/Slave, modelo de datos, mapeo, funciones, trama |
| Bloque C: Ejercicios 1-6 | ✅ Completo | Graduales ⭐ a ⭐⭐⭐⭐ |
| Guía práctica detallada Modbus | ✅ Listo | `Guia_Practica_Modulo2_Modbus.docx` |
| Cheat sheet Modbus (1 página) | ✅ Listo | `Cheatsheet_Modbus_TCP.pdf` |
| Evaluación formal Sección 2 | ⬜ Pendiente | Cuestionario imprimible para los alumnos |

### Próximos pasos Sección 2:
- [x] Crear guía práctica Word con código Ladder completo para cada ejercicio
- [x] Crear cheat sheet imprimible (mapeo de registros + funciones)
- [ ] Crear evaluación formal (cuestionario teórico-práctico)

---

## Sección 3: Socket TCP – SOCKSND / SOCKRCV (3.5 hs) ⬜ PENDIENTE

| Entregable | Estado | Descripción |
|------------|--------|-------------|
| PPT Módulo 3 (~30 slides) | ⬜ Pendiente | Concepto de sockets, SOCKSND, SOCKRCV, registros especiales |
| Guía práctica Socket TCP | ⬜ Pendiente | Ejercicios paso a paso con Hercules/Python listener |
| Scripts de ejemplo Python | ⬜ Pendiente | Listener TCP para recibir datos del PLC |
| Evaluación Sección 3 | ⬜ Pendiente | Cuestionario + ejercicio práctico |

### Contenido planificado:
- [ ] Bloque A: Concepto de socket vs Modbus (comparativa visual)
- [ ] Bloque B: SOCKSND parámetro por parámetro con diagrama
- [ ] Bloque C: SOCKRCV y registros especiales (D1120, M1350, M1351)
- [ ] Ejercicio 1: Instalar Hercules y verificar que escucha
- [ ] Ejercicio 2: Primer envío SOCKSND (3 registros al PC)
- [ ] Ejercicio 3: Envío periódico con temporizador
- [ ] Ejercicio 4: Recibir datos con SOCKRCV
- [ ] Ejercicio 5: Sistema de reporte (presión + estado + timestamp)
- [ ] Bloque D: Troubleshooting de sockets (timeout, IP, puerto)

---

## Sección 4: Webserver embebido (2.0 hs) ⬜ PENDIENTE

| Entregable | Estado | Descripción |
|------------|--------|-------------|
| PPT Módulo 4 (~20 slides) | ⬜ Pendiente | Webserver del DVP-SE, acceso, navegación, integración web |
| Guía práctica Webserver | ⬜ Pendiente | Ejercicios de acceso desde PC, tablet, celular |
| Demo Node-RED (opcional) | ⬜ Pendiente | Flujo básico que lee Modbus TCP y muestra dashboard |

### Contenido planificado:
- [ ] Acceso al webserver: URL, credenciales, navegadores
- [ ] Recorrido completo por las secciones del webserver
- [ ] Ejercicio 1: Acceso multi-dispositivo (PC + celular)
- [ ] Ejercicio 2: Monitoreo en tiempo real vs ISPSoft
- [ ] Ejercicio 3: Ventajas y limitaciones vs SCADA
- [ ] Intro integración web avanzada: PLC → Gateway → Dashboard
- [ ] Demo conceptual Node-RED / Python Flask (sin práctica)

---

## Sección 5: Email / SMTP (3.0 hs) ⬜ PENDIENTE

| Entregable | Estado | Descripción |
|------------|--------|-------------|
| PPT Módulo 5 (~25 slides) | ⬜ Pendiente | SMTP, 3 métodos, Gmail app password, ejercicios graduales |
| Guía práctica Email | ⬜ Pendiente | Instalación Python, scripts paso a paso |
| Scripts Python completos | ⬜ Pendiente | Lectura Modbus + envío email con smtplib |
| Programa Ladder de alarmas | ⬜ Pendiente | Simulación alarma Line Break para disparar email |

### Contenido planificado:
- [ ] Bloque A: Concepto SMTP, por qué email en industria
- [ ] Bloque B: Método 1 – HMI DOP (DOPSoft SMTP config)
- [ ] Bloque C: Método 2 – Python (pymodbus + smtplib)
- [ ] Intro Python para no-programadores (30 min)
- [ ] Gmail: contraseña de aplicación paso a paso
- [ ] Ejercicio 1: Instalar Python + pymodbus
- [ ] Ejercicio 2: Script que lee registros e imprime
- [ ] Ejercicio 3: Lógica de alarma (si valor < umbral → alerta)
- [ ] Ejercicio 4: Envío de email real
- [ ] Ejercicio 5: Alarma Line Break completa (Ladder + Python)
- [ ] Método 3: Socket TCP directo a SMTP (conceptual)

---

## Sección 6: PLC a PLC + Integrador (2.5 hs) ⬜ PENDIENTE

| Entregable | Estado | Descripción |
|------------|--------|-------------|
| PPT Módulo 6 (~20 slides) | ⬜ Pendiente | Data Exchange, Master/Slave, ejercicio integrador |
| Guía práctica PLC a PLC | ⬜ Pendiente | Config DCISoft paso a paso |
| Ejercicio integrador | ⬜ Pendiente | Diseño de arquitectura Line Break completo |
| Evaluación final del curso | ⬜ Pendiente | Cuestionario teórico-práctico + encuesta satisfacción |
| Certificado de aprobación | ⬜ Pendiente | Template Word/PDF para entregar a los alumnos |

### Contenido planificado:
- [ ] Bloque A: Data Exchange en DCISoft (Master/Slave)
- [ ] Bloque B: Config registros lectura/escritura, Enable Condition
- [ ] Ejercicio 1: Data Exchange entre 2 PLCs (o PLC + simulador)
- [ ] Ejercicio 2: Simular con ModbusClientX como esclavo virtual
- [ ] Ejercicio integrador: diseñar arquitectura Line Break completa
- [ ] Repaso general: tabla comparativa de los 5 métodos TCP
- [ ] Evaluación de conocimientos
- [ ] Evaluación del curso (encuesta satisfacción)

---

## 📦 Entregables transversales

| Entregable | Estado | Descripción |
|------------|--------|-------------|
| README del repo | ✅ Listo | Índice, contenido, equipamiento, perfil alumno |
| Planificación 16 hs | ✅ Listo | `Resumen_Capacitacion_TCP_16hs.docx` |
| Presentación general | ✅ Listo | `Capacitacion_TCP_DVP12SE.pptx` (28 slides resumen) |
| Guía práctica general | ✅ Listo | `Guia_Practica_TCP_DVP12SE.docx` |
| ROADMAP | ✅ Listo | Este archivo |
| Cheat sheet general | ⬜ Pendiente | 1 página con todos los métodos TCP, puertos, fórmulas |
| Kit de instalación | ⬜ Pendiente | Links de descarga ISPSoft, COMMGR, DCISoft, ModbusClientX |
| Certificado template | ⬜ Pendiente | Diploma de aprobación del curso |

---

## 🏷️ Convención de archivos

```
Redes_Fisico_a_Logico.pptx          → Intro (previo al Módulo 1)
Modulo1_Repaso_Conexion.pptx         → Sección 1
Modulo2_Modbus_TCP.pptx              → Sección 2
Modulo3_Socket_TCP.pptx              → Sección 3 (pendiente)
Modulo4_Webserver.pptx               → Sección 4 (pendiente)
Modulo5_Email_SMTP.pptx              → Sección 5 (pendiente)
Modulo6_PLC_a_PLC_Integrador.pptx    → Sección 6 (pendiente)
Guia_Practica_*.docx                 → Guías por módulo
Evaluacion_*.docx                    → Evaluaciones por módulo
Cheatsheet_*.pdf                     → Referencias rápidas
```

---

## 📅 Orden sugerido de desarrollo

1. ~~Sección 1: Repaso + Conexión~~ ✅
2. **Sección 2: Completar guía + cheat sheet** ← próximo paso
3. Sección 3: Socket TCP (la más técnica, necesita scripts Python)
4. Sección 4: Webserver (la más simple de preparar)
5. Sección 5: Email SMTP (necesita scripts Python + config Gmail)
6. Sección 6: Integrador + evaluación final + certificado
7. Revisión general y prueba piloto con un grupo reducido

---

*Última actualización: Marzo 2026*
