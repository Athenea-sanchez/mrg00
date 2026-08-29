# Robotica_movil_PE107220

> Repositorio de prácticas del curso de **Robótica Móvil** (PAPIME PE107220, MRG-MX, UNAM).

- **Ruta:** `repositorios/Robotica_movil_PE107220`
- **Remoto git:** `https://github.com/mrg-mex/Robotica_movil_PE107220` (rama `main`)
- **Tamaño:** ~7.2 MB · **18 archivos** · 8 commits (2022-03-21 → 2022-04-04).
- **Idioma:** español (documentación) · código en inglés.

> **Control de versiones (fecha de esta documentación):** al día de hoy, **28 de agosto de 2026**, el repositorio se encuentra en la rama `main` con el último commit `7814432` (*"Update README.md"*) fechado el **4 de abril de 2022**, sincronizado con su remoto `origin` (trabajo limpio, sin cambios pendientes). Nótese que este proyecto está **inalterado desde 2022** (repositorio de archivo). Esta ficha se creó fuera del repositorio (en `repositorios/documentacion/`) y no modifica su historial git.

---

## 1. Qué es

Es el repositorio de **prácticas de laboratorio** para la asignatura de "Robótica y Robótica Móvil" (Facultad de Ingeniería UNAM, Departamento de Ingeniería Mecatrónica), dentro del proyecto **PAPIME PE107220**. Organizado por el grupo **MRG-MX** (responsables: Dr. Víctor Javier González Villela, M.I. Alejandro Ruiz Esparza Rodríguez, M.I. Erik Peña Medina).

Contiene **6 prácticas progresivas** que cubren visión por computadora, comunicación WiFi, control de velocidad (encoder/PID), cinemática diferencial, campos potenciales artificiales y visión por AprilTag.

## 2. Qué hace y cómo es, en general

Cada práctica es autocontenida y consta de: un **manual PDF** (Objetivo → Metas → Antecedentes → Materiales → Desarrollo → Resultados → Conclusiones), los **códigos Arduino (ESP8266)** y un **modelo Simulink (R2020a)**. Las prácticas usan el **robot RM20 (aluminio)** con drivers de motor (MD25) y comunicación UDP vía WiFi.

**Las 6 prácticas:**
1. **Sistema de visión** con ReacTIVision (marcadores fiduciales, TUIO/UDP → Simulink).
2. **Conexión WiFi** — control inalámbrico con ESP8266 (NodeMCU) y UDP.
3. **Encoder de cuadratura y control de velocidad PID** — 3 sketches Arduino progresivos.
4. **Robot móvil diferencial (2,0)** — cinemática con MD25 e I2C.
5. **Campos potenciales artificiales** y seguimiento de trayectorias con retroalimentación visual.
6. **Visión AprilTag** — sistema fiducial basado en ROS (solo PDF).

## 3. Tecnologías

| Tecnología | Uso |
|---|---|
| Arduino IDE / C++ (`.ino`) | Sketches ESP8266 (UDP, encoder, PWM, PID, MD25) |
| MATLAB / Simulink R2020a | Todos los modelos `.slx` + bloques MATLAB Function |
| ESP8266 (NodeMCU / ESPino) | Cliente/servidor WiFi-UDP en los robots |
| WiFi / UDP | Comunicación Simulink ↔ robot |
| I2C / MD25 | Driver de motor ("ATACANTE", addr 0x58) |
| Puente H L293D | Control de velocidad de motores |
| Encoders de cuadratura | Medición de velocidad (interrupciones) |
| ReacTIVision + cliente C# TUIO | Visión de la práctica 1 |
| AprilTag + ROS | Visión de la práctica 6 (documentada) |

## 4. Estructura de carpetas

```
Robotica_movil_PE107220/
├── README.md                               # Objetivo, responsables, índice
├── Simulacion/
│   └── SimulacionRM20.slx                  # Modelo completo
├── Practica_1_SistemaVision/
│   ├── RM_P01_SistemaDeVision.pdf          # Manual 32 págs.
│   └── Matlab/ComunicacionUDP.slx
├── Practica_2_ConexionWifi/
│   ├── RM_P02_ConexionWiFi.pdf
│   ├── ESP8266/Udp.ino
│   └── Matlab/ComunicacionUDP.slx
├── Practica_3_ControlVelocidad/
│   ├── RM_P03_ControlDeVelocidad.pdf
│   └── ESP8266/ (practica1_encoder.ino, practica2_puenteH.ino, practica3_controlPID.ino)
├── Practica_4_RMDiferencial/
│   ├── RM_P04_RobotMovilDiferencial.pdf
│   ├── ESP8266/RM20_aluminio/ (RM20_aluminio.ino, ProgramarESPino.txt)
│   └── Matlab/ControlRM20.slx
├── Practica_5_CamposPotenciales/
│   ├── RM_P05_APFyTrayectoria.pdf
│   └── Matlab/ControlRM20.slx
└── Practica_6_VisionAprilTag/
    └── RM_P06_AprilTag.pdf
```

## 5. Documentos presentes (para qué sirven)

- **6 PDFs de práctica** (ver tabla en la sección 4) — manuales completos con teoría y procedimiento.
- **README.md** — objetivo del proyecto, responsables, índice de prácticas, contacto y agradecimiento PAPIME. No incluye guía de instalación ni explicación de la estructura.
- **`ProgramarESPino.txt`** — pasos para poner el ESPino en modo bootloader.

## 6. Lo que se asumiría que hay (pero falta)

- **`LICENSE`** — no existe (repo educativo público de la org `mrg-mex` sin licencia).
- **`.gitignore`** — no existe.
- **Código de Visión** (práctica 1): no hay cliente C# TUIO ni configuración de ReacTIVision (`camera.xml`).
- **Recursos de visión** (PDFs de marcadores/imprimir, fotos, esquemas) — todo está embebido en los PDFs.
- **READMEs por práctica** y changelog.
- **Scripts MATLAB en `.m`** — el código está embebido dentro de los `.slx` (más difícil de revisar/diferenciar).
- **Código de la práctica 6** (AprilTag/ROS) — solo existe el PDF.
- **Documentación del formato del paquete de visión** — solo se infiere del parser embebido (cadena de 17 caracteres que empieza con 'M').

## 7. Qué está mal y cómo se corregiría

1. **Credenciales WiFi hardcodeadas** — SSID `MiRed` y contraseña `123456789` en `Udp.ino` y `RM20_aluminio.ino`. En `Udp.ino` ya se usa el patrón `#ifndef STASSID`; extenderlo o usar archivo de config para que cada alumno ponga su red.
2. **Código muerto / comentado** — `RM20_aluminio.ino::connectWifi()` **retorna `true` incondicionalmente** y la lógica de espera de conexión está comentada → puede fallar en silencio (no verifica que esté conectado). Revisar.
3. **Modelos duplicados** — `ControlRM20.slx` es el mismo en prácticas 4 y 5; el parser de visión se repite en 3 modelos → centralizar en una carpeta `lib/` compartida.
4. **Nomenclatura de carpetas inconsistente** — `Practica_2_ConexionWifi`/`Practica_5_CamposPotenciales` (descriptivas) vs `Practica_4_RMDiferencial` (abreviada); el sketch `RM20_aluminio` está anidado en un subfolder dentro de la práctica 4, mientras los demás sketches están directo en `ESP8266/`.
5. **Práctica 6 sin código/MATLAB** — estructura asimétrica respecto a las demás.
6. **Mezcla español/inglés** en comentarios y nombres → unificar criterio.
7. **README muy breve** — falta tabla de contenido práctica→archivos, "cómo ejecutar" y requisitos de versión (MATLAB R2020a, core ESP8266 de Arduino, soporte Simulink para Arduino).
8. **Hygiène de git** — autor local (`Athenea-sanchez`) distinto de los autores de commits; sin tags/releases.
9. **Typos** — `Aranque/Paro` (debería "Arranque"), `packetBufffer` en `Udp.ino`.
10. **Práctica 2 desincronizada con su PDF** — su `ComunicacionUDP.slx` duplica el parser de la práctica 1, pero el PDF narra otra cosa.

## 8. Diagrama sugerido (descripción en texto)

Se recomiendan dos diagramas:

1. **Bucle de control de velocidad PID** (diagrama de bloques de retroalimentación): típico bucle cerrado en el que la consigna de velocidad (`u`, setpoint) se compara con la velocidad medida del encoder (cuadrúpedo, 400 cuentas/vuelta), la diferencia pasa por el controlador PID (con anti-windup y saturación a ±1023), genera el PWM hacia el puente H L293D, y el motor produce la medición real que realimenta. Este diagrama ayudaría a entender la práctica 3.

2. **Flujo de datos Visión → Simulink → Robot** (canal de comunicación): un diagrama de proceso desde la cámara con ReacTIVision, que publica datos TUIO al cliente C#, se envían por UDP a Simulink (bloque `UDP_Protocol` + parser de la cadena de 17 caracteres con posición/orientación), donde el controlador (APF/cinemática) calcula velocidades de ruedas, se convierten a bytes MD25 (128±u) y se transmiten por UDP al ESP8266 del robot, que mueve los motores I2C. Este diagrama unifica el flujo completo de las prácticas 1, 2, 4 y 5 en un solo vistazo.
