# Repositorio_PE107220

> Repositorio del proyecto **PAPIME PE107220** — bancos de prueba de robótica (UNAM, Departamento de Ingeniería Mecatrónica).

- **Ruta:** `repositorios/Repositorio_PE107220`
- **Remoto git:** `https://github.com/mrg-mex/Repositorio_PE107220` (rama `main`)
- **Enfoque:** MATLAB/Simulink + Dynamixel + CAD.

> **Control de versiones (fecha de esta documentación):** al día de hoy, **28 de agosto de 2026**, el repositorio se encuentra en la rama `main` con el último commit `ee6ac19` (*"Add files via upload"*) fechado el **14 de abril de 2022**, sincronizado con su remoto `origin` (trabajo limpio, sin cambios pendientes). Nótese que este proyecto está **inalterado desde 2022** (repositorio de archivo). Esta ficha se creó fuera del repositorio (en `repositorios/documentacion/`) y no modifica su historial git.

---

## 1. Qué es

Es el repositorio del proyecto **PAPIME PE107220** *"Fortalecimiento de la enseñanza de la asignatura de robótica mediante la elaboración de material didáctico"*, financiado por la **DGAPA** (UNAM). Su propósito es servir de **archivo de entrega** con las notas, el manual/material de control de motores **Dynamixel** y los archivos de los tres **bancos de prueba** docentes:

- **Robótica serial** (`Banco_serial`) — brazo de 4 GDL.
- **Robótica paralela** (`Banco_paralelo`) — robot Delta plano.
- **Robótica móvil** (`Banco_Movil`) — tres robots (2,0) con sistema de visión.

## 2. Qué hace y cómo es, en general

Permite **simular** los bancos en Simulink/Simscape Multibody (importados desde SolidWorks) y **controlar** los motores **Dynamixel AX-12A** desde MATLAB mediante S-Functions. Incluye CAD de los mecanismos, cinemática inversa del Delta, y documentación de configuración de los motores.

## 3. Tecnologías

| Tecnología | Uso |
|---|---|
| MATLAB / Simulink | Simulación de robots e interfaces de control (`.slx`, `.m`) |
| Simscape Multibody 7.4 | Simulación física (`.xml` + `_DataFile.m`) |
| SolidWorks / Autodesk Inventor | CAD (`STEP`, `.stl`, `.iam`, `.ipt`) |
| DynamixelSDK / C | Control de motores AX-12A |
| S-Functions MATLAB | Bloques Simulink read/write de Dynamixel |
| Dynamixel Wizard 2.0 | Configuración de motores (documentada) |

## 4. Estructura de carpetas

```
Repositorio_PE107220/
├── README.md
├── Reporte de los Bancos de prueba.pdf      # Informe de 10 págs.
├── Banco_Movil/                             # Banco móvil (fotos)
├── Banco_paralelo/                          # Banco paralelo (Delta)
│   └── ROBOT_PARALELO_PROYECTO/
│       ├── *.STEP                           # Piezas del Delta
│       ├── calculo_de_tetas.m               # Cinemática inversa
│       ├── calculo_de_xp_yp_tp.m            # Planeación de trayectorias
│       ├── myfunction_pierna1/2/3.m         # Ecuaciones de piernas
│       ├── delta_completo.xml
│       ├── simulacion_robot_delta.slx
│       └── slprj/                           # Caché GENERADA
├── Banco_serial/                            # Banco serial (4 GDL)
│   ├── CAD/                                 # Inventor/SolidWorks + STL
│   └── Simulacion_Banco_Serial/             # Simscape + STEP
│       ├── Banco_Serial_Completo.slx / .xml / _DataFile.m
│       └── slprj/                           # Caché GENERADA
└── Manual_Dynamixel/                        # Control de motores
    ├── Dynamixel Simulink Library/          # Librería MathWorks (2017)
    └── Dynamnixel/                          # Proyecto propio
        ├── comandos_config.txt
        ├── variables_dynamixel.m
        ├── prueba_dynamixel.slx / read_write_sl.slx
        └── Documentos/ (.docx)
```

## 5. Documentos presentes (para qué sirven)

- **`Reporte de los Bancos de prueba.pdf`** — informe de 10 páginas con metodología, CAD, simulaciones y fotos de los tres bancos, conclusiones y trabajo futuro.
- **README.md** — visión general, contenido, autores y agradecimiento.
- **`Configuración Inicial de los motores Dynamixel.docx`** (1.7 MB) — guía detallada: diagrama de conexión con U2D2, Dynamixel Wizard 2.0, COM port, protocolo 1.0, baudrate, registros (ID=3, Torque Enable=24, Goal Position=30, Moving Speed=32) y SDK en MATLAB.
- **`Manual de implemtación.docx`** — formato para la implementación técnica del control (nótese el typo "implemtación").
- **README de la librería terceros** (`manual. Setup README.txt`) — uso de la librería MathWorks de Dynamixel para Simulink.
- **READMEs de 1 línea** en `Manual_Dynamixel/` y `Dynamnixel/`.

## 6. Lo que se asumiría que hay (pero falta)

- **`.gitignore` — AUSENTE (el problema crítico).**
- **`LICENSE`** — no hay licencia a nivel raíz (solo la licencia de la librería vendida).
- **READMEs por banco** — no queda claro qué archivo abrir primero en cada banco.
- **Scripts `.m` legibles** para todos los cálculos — varios quedan embebidos en los `.slx`.
- **Indicación de versión de MATLAB / Simulink** requerida para los modelos propios.
- **CI / tests / CHANGELOG / CONTRIBUTING** — ausentes.

## 7. Qué está mal y cómo se corregiría

### El problema más grave: artefactos generados commiteados
De 215 archivos trackeados, **~134 son artefactos generados**. Con un `.gitignore` apropiado se reducen drásticamente y se evita ruido en diffs/reproducibilidad:
- `slprj/`, `_jitprj/`, `_sfprj/` (cachés de build de Simulink).
- `.slxc` (caché compilado).
- `.slx.autosave` (`Banco_Serial_Completo.slx.autosave`) — transitorio, **nunca** debe commitecarse.
- `sl_proj.tmw`, código generado `slcc_interface_*.c`.

Se recomienda: añadir `.gitignore` con esas rutas y limpiar el historial (`git rm --cached` + `filter-branch`/BFG) para eliminar los binarios ya rastreados.

### Otros problemas
1. **Dos nombres para lo mismo:** carpeta `Dynamnixel` (mal escrita) vs `Dynamixel Simulink Library`; el README raíz habla de "Manual de control Dynamixel".
2. **Typos:** `Manual de implemtación.docx` (implementación), y "paralélelo" en el PDF.
3. **Rutas absolutas de Windows** en `comandos_config.txt` (p. ej. `D:\Repositorios\DynamixelSDK-3.7.31\...`, `C:\Users\johnc\...`) → volverlas relativas o documentar la descarga del SDK.
4. **Nombres con acentos y espacios** (`Base_móvil`, `Eslabón_1`, "Dynamixel Simulink Library") → complican el manejo cross-platform en git; considerar renombrar.
5. **Binarios grandes vendidos** (STEP de hasta 11 MB, librería empaquetada) → inflan el repo (~42 MB).
6. **Herramienta CAD mixta:** serie usa Inventor (`.iam`/`.ipt`) y paralela se importa de SolidWorks → documentar la cadena.
7. **Código duplicado:** `slDxl.m` vs `slDxl_prueba.m` (evolución/experimento lado a lado) → limpiar.
8. **Sin instrucciones de ejecución/uso** para el control Dynamixel (solo en los `.docx` y el README vendido).

## 8. Diagrama sugerido (descripción en texto)

Se recomiendan dos diagramas:

1. **Cadena de simulación (CAD → simulación → control)**: un diagrama de flujo que arranque en el modelo CAD (SolidWorks/Inventor), continúe con la exportación e importación vía `smimport` (que produce el XML y el `_DataFile.m`), siga con la construcción del modelo Simscape Multibody en Simulink, y termine en el control del Dynamixel (S-Function de lectura/escritura hacia el motor AX-12A vía U2D2). Este diagrama aclara cómo conectar el mundo CAD con la simulación y el control real.

2. **Limpieza del repositorio (git flow)**: un diagrama de proceso recomendado para sanear el repo: crear el `.gitignore` con `slprj/`, `_jitprj/`, `_sfprj/`, `*.slxc`, `*.slx.autosave` → `git rm --cached` de los artefactos → limpiar el historial con BFG/filter-branch → verificar que el repo compile simule desde cero. Este diagrama serviría de guía práctica de mantenimiento.
