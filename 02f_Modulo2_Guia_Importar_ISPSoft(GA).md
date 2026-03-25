# Guia: Importar programa IL en ISPSoft

## Requisitos previos

- **ISPSoft v3.x** instalado (descarga gratuita en [delta-emea.com](https://www.delta-emea.com))
- **WPLSoft v2.x** (alternativa, misma pagina de descarga)
- Archivo: `ips texto v2.txt` (programa corregido con comentarios)

---

## Metodo 1: Copiar/Pegar en ISPSoft (Recomendado)

### Paso 1 — Crear proyecto nuevo

1. Abrir **ISPSoft**
2. Menu **File → New**
3. Configurar:
   - **Project Name**: `Monitoreo_3Sensores` (o el nombre que prefieran)
   - **PLC Type**: `DVP-SE` (o `DVP-SE2` segun modelo exacto)
   - **PLC Model**: `DVP12SE11R`
   - **Communication**: `Ethernet`
4. Click **OK**

### Paso 2 — Abrir editor en modo IL

1. En el arbol del proyecto (panel izquierdo), expandir **Programs**
2. Doble-click en **Program0** para abrir el editor
3. Cambiar a modo **Instruction List**:
   - Menu **View → Instruction List**
   - O click en el icono **IL** en la barra de herramientas del editor
   - (El icono parece un documento con texto, al lado del icono de Ladder)

### Paso 3 — Preparar el archivo

1. Abrir `ips texto v2.txt` con un editor de texto (Notepad, Notepad++, VS Code)
2. **Eliminar todas las lineas de comentario** (las que empiezan con `;`)
   - En **Notepad++**: Ctrl+H → buscar `^;.*\r?\n` → reemplazar por nada → modo **Expresion Regular** → Reemplazar Todo
   - En **VS Code**: Ctrl+H → buscar `^;.*\n` → reemplazar por nada → activar icono **Regex** (.*) → Reemplazar Todo
3. **Eliminar lineas vacias** sobrantes
4. Seleccionar todo (Ctrl+A) y copiar (Ctrl+C)

### Paso 4 — Pegar en ISPSoft

1. En el editor IL de ISPSoft, posicionar el cursor al inicio
2. **Ctrl+V** para pegar
3. Si aparece algun error de formato:
   - Verificar que no haya espacios extra al inicio de cada linea
   - Verificar que la instruccion `END` este al final

### Paso 5 — Compilar

1. Menu **Compile → Build** (o presionar **F7**)
2. Revisar la ventana **Output** en la parte inferior:
   - `0 Error(s), 0 Warning(s)` → programa listo
   - Si hay errores, revisar el numero de linea indicado

### Paso 6 — Transferir al PLC

1. Conectar el PLC por **Ethernet** (configurar IP en el mismo rango)
2. Menu **Communication → Communication Settings**:
   - **Type**: Ethernet
   - **IP**: la IP del PLC (ej: `192.168.1.1`)
   - Click **Search** para detectar automaticamente
3. Menu **Communication → Transfer to PLC** (o Ctrl+F5)
4. Seleccionar **Program + Comments** → OK

---

## Metodo 2: Usando WPLSoft (alternativa)

WPLSoft trabaja nativamente con IL de texto y es mas tolerante con el formato.

### Pasos

1. Abrir **WPLSoft**
2. Menu **File → New**:
   - **PLC Type**: `DVP-SE`
   - Click OK
3. Menu **Edit → Paste** (o Ctrl+V) con el contenido del `.txt` copiado
   - Alternativamente: **File → Import → Text File**
4. Compilar: **Compile → Compile All** (F4)
5. Para convertir a ISPSoft: **File → Export → ISPSoft Project (.mpu)**

---

## Verificacion rapida post-carga

Despues de cargar el programa al PLC, verificar en el monitor:

| Que verificar | Como | Resultado esperado |
|---|---|---|
| Programa corriendo | ISPSoft → Monitor → Run | LED RUN encendido |
| M1000 = ON | Monitor M1000 | Siempre ON (reloj sistema) |
| D160 = 1 | Monitor D160 | Estado 1 (inicial) |
| M4 responde | Forzar D7016 > D2 | M4 = ON |
| Codigos default | Monitor C70,C71,C72 | 3,3,3 (codigo "333") |

---

## Solucion de problemas comunes

| Error | Causa | Solucion |
|---|---|---|
| `Syntax Error` en linea con `;` | ISPSoft no acepta comentarios con `;` | Eliminar lineas de comentario |
| `Unknown Instruction` | Modelo de PLC incorrecto | Verificar que sea DVP12SE11R |
| `Address Out of Range` en D7xxx | Modelo sin registros extendidos | Usar DVP-SE (no DVP-ES o DVP-SA) |
| No compila `LD>= D7016 D2` | Version antigua de ISPSoft | Actualizar ISPSoft a v3.x o superior |
| `Communication Error` al transferir | IP mal configurada | Verificar IP del PLC con ping |

---

*Documento para Capacitacion TCP/IP — VALBOL*
*Archivo de programa: `ips texto v2.txt`*
