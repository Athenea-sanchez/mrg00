# Papime_PE110923_rserial

> Repositorio de **robótica serial** del proyecto PAPIME PE110923 (UNAM, Facultad de Ingeniería).

- **Ruta:** `repositorios/Papime_PE110923_rserial`
- **Remoto git:** `https://github.com/arrg-mx/Papime_PE110923_rserial`
- **Rama:** `main` · **Commits:** ~83 · **Archivos trackeados:** 245
- **Tamaño:** ~163 MB (122 MB en `robo_lab`, sobre todo STL).
- **Licencia:** MIT.

> Nota: "serial" aquí significa **manipuladores seriales** (cadenas cinemáticas abiertas), **no** comunicación por puerto serie/RS-232. No hay código de Arduino/serial-port en el repo.

> **Control de versiones (fecha de esta documentación):** al día de hoy, **28 de agosto de 2026**, el repositorio se encuentra en la rama `main` con el último commit `3395eab` (*"Update Practica_8.ipynb"*) fechado el **11 de junio de 2025**, sincronizado con su remoto `origin` (trabajo limpio, sin cambios pendientes). Esta ficha se creó fuera del repositorio (en `repositorios/documentacion/`) y no modifica su historial git.

---

## 1. Qué es

Es el repositorio de **robótica serial** del proyecto **PAPIME PE110923**: simulación de **manipuladores seriales** con ROS 2 + Gazebo, control de articulaciones y material educativo.

**Responsable:** Dr. Víctor Javier González Villela. Participantes: M.I. Erik Peña Medina, Ing. Felipe Rivas Campos, M.I. Daniel Haro Mendoza.

## 2. Qué hace y cómo es, en general

Proporciona:

- **Robots seriales simulados:** **SCARA**, **Scorbot ER-4**, brazo **Dofbot**, robot **omnidireccional (X3)** y el manipulador móvil **Omni+Dofbot**.
- **Mundos Gazebo** (mesas, cajas, paredes) y configs **RViz**.
- **Control ROS 2**: controladores de trayectoria articular, posición, velocidad y gripper; e **inversa del Scorbot** mediante resolución de ángulos.
- **Material educativo**: notebooks Jupyter y **8 prácticas** de laboratorio.
- **Bases ROS 2** en Python y C++ (nodos, tópicos, msg/srv).

## 3. Tecnologías

| Tecnología | Uso |
|---|---|
| Python 3 | 29 `.py` (nodos, launch, tests) |
| C++ | 8 `.cpp` (nodos, servicios, mensajes) |
| Xacro / URDF | 33 `.xacro` + 2 `.urdf` |
| SDF / World | 4 `.world` + modelos `.sdf` |
| YAML (`ros2_control`) | 5 configs de controladores |
| RViz | 9 configs |
| Jupyter | 12 notebooks |
| STL | 39 mallas |
| CMake / ament | 8 `CMakeLists.txt` |
| ROS 2 | Humble (`rclpy`, `rclcpp`, `ros2_control`, `gazebo_ros`) |

## 4. Estructura de carpetas

```
Papime_PE110923_rserial/
├── README.md                     # Visión, créditos, VM, índice (salta el punto 4)
├── LICENSE                       # MIT
├── docs/                         # Notebooks de teoría + imágenes
│   ├── Conceptos_basicos.ipynb
│   ├── Nodos_y_Topicos.ipynb     # Stub casi vacío
│   ├── Robotica_serial_control.ipynb  # Control SCARA (con código)
│   └── Sensores_basicos.ipynb
├── practicas/                    # Practica_1..8.ipynb (plantillas)
├── ros2_basic/                   # Bases ROS 2
│   ├── nodes_topics_py/          # Python
│   ├── nodes_topics_cpp/         # C++
│   └── messages_services_pkg/    # msg/srv + ejemplos C++
├── scara_pkgs/
│   ├── scara_description/        # URDF, sensores, controladores
│   └── scara_bringup/            # mundos, 6 launches, tests de trayectoria
├── scorbot_pkgs/
│   ├── scorbot_description/      # URDF con mallas reales
│   └── scorbot_bringup/          # launch + test con inversa
└── robo_lab/
    ├── dofbot_bringup/  +  dofbot_description/
    ├── omni_description/
    └── omni_dofbot_bringup/
```

## 5. Documentos presentes (para qué sirven)

- **README.md** — propósito, créditos, enlace a la VM del curso y **índice con numeración rota** (1,2,3,5,6,7 — falta el 4).
- **15 `.md`** distribuidas: `docs/jupyter_docs.md`, `practicas/practicas.md`, `ros2_basic.md`, `scara_pkg.md`, `scorbot_pkg.md`, `robo_lab/robots_lab.md` (instalación/ejecución).
- **12 notebooks**: `docs/` de teoría y `practicas/Practica_1..8.ipynb` (plantillas: URDF, robot móvil URDF, nodos/tópicos, interfaces, Gazebo, control de posición/velocidad, sensores).
- **Placeholders sin valor:** varios `borrame.md` (incluso con contenido "om"/"X3"), `delate.md`, `README.md` ("Interfaces") y `Readme.md` vacíos.

## 6. Lo que se asumiría que hay (pero falta)

- **`.gitignore`** — no existe.
- **Quick-start en README raíz** — no hay instrucciones de build; solo enlace a VM. Las de build están en los `.md` por área.
- **Paquete `omni_dofbot_description` — AUSENTE.** `omni_dofbot_bringup/launch/omni_dofbot_controller.launch.py` lo referencia, y `robots_lab.md` documenta `omni_bringup` y `omni_dofbot_description`, pero **no existen** → el launch no corre tal cual.
- **README por paquete** — casi ningún paquete tiene el suyo.
- **`message_and_services.ipynb`** — mencionado en `ros2_basic.md` pero no presente en `docs/`.
- **`urdf.rviz`** en `scara_bringup` — referenciado por un launch pero inexistente.
- **Tests** — solo los 3 lint automáticos de `nodes_topics_py`.

## 7. Qué está mal y cómo se corregiría

### Build / ejecución
1. **`omni_dofbot_controller.launch.py`** referencia el paquete inexistente `omni_dofbot_description`, además de usar el world de `dofbot_bringup` y un `wheel_velocity_controller` de omni → crear el paquete o corregir el launch.
2. **`scorbot_bringup/CMakeLists.txt`** instala `DIRECTORY launch rviz world src` pero **no existe `world/`** → quitar esa entrada (fallaría `colcon build`).
3. **Typos en launches:** `trajectory_controller_scara.launch.py` y `tes2_...` referencian `scara_trayectory_rviz.rviz` (real: `scara_trajectory_controller_rviz.rviz`); `tes2_...` usa `test_2_world.wold` (debe ser `.world`); `roberto_pos_controller_scara.launch.py` busca `rviz/urdf.rviz` (solo existe `scara_rviz.rviz`).
4. **`setup.py` de `nodes_topics_py`**: entry point `pub_node_py` no coincide con el módulo `node_pub_py.py` → corregir.
5. **`position_test.py`** no se instala en `scara_bringup`; `omni_dofbot_bringup/CMakeLists.txt` tiene un `install(PROGRAMS ...)` vacío.

### Nombres / consistencia
6. **Mismatch carpeta vs `package.xml`:** las carpetas son `scara_bringup`/`scara_description` pero declaran paquetes `examen_bringup`/`examen_description` → unificar.
7. **Ruta absoluta hardcodeada** en `scara_description/package.xml` (`/home/robousr/ROS2Dev/robot_2025_ws/...`) → usar `$(find ...)`.
8. `dofbot_description` usa `$(find ...)` mientras `omni_description` usa `$(find-pkg-share ...)` → estandarizar.
9. **README raíz salta el punto 4** del índice → renumerar.
10. **`scara_pkg.md` contenido malformado:** URLs sueltas (`https://youtu.be/...`) incrustadas dentro del árbol de directorios → corregir el marcado.
11. **`robots_lab.md` desincronizado:** documenta nombres de packages/launches que no coinciden con el repo real.
12. Archivos casi idénticos duplicados (SCARA `trajectory_test.py` vs `tray_test.py`; `service_server.cpp` vs `servicio_server.cpp`) → refactorizar/parametrizar.
13. Typos: `template_pytnon_node.py`, `tes2_trayectory...`, `omni_dofbot_trayectory_rviz.rviz`, `dofbot_trajectory_rviz.rviz`.
14. Junk: `borrame.md` ×5, `delate.md`, `Readme.md` vacíos → eliminar.
15. `docs/imagenes` con ~49 imágenes, varias sin usar; y `Conceptos_basicos.ipynb` referencia capturas (`gazebo_1.png`) que **no existen** → alinear imágenes.

## 8. Diagrama sugerido (descripción en texto)

Se recomiendan dos diagramas:

1. **Flujo de control de trayectoria** (cadena de bloques): arrancar en el launch del `bringup`, que carga la descripción (URDF/xacro) y el config YAML del controlador, luego ejecuta el servidor de controladores de `ros2_control`, y finalmente un script de prueba (`trajectory_test.py`) que publica `JointTrajectory` en el tópico del controlador; los comandos mueven el modelo en Gazebo y `joint_state_publisher`/`rviz2` muestran el resultado. Este diagrama aclara el camino *código → controlador → simulación → visualización*.

2. **Algoritmo de cinemática inversa del Scorbot**: un diagrama de flujo con las etapas: entrada de poses objetivo de la herramienta → construcción de matrices de rotación → productos punto/cruz para resolver los ángulos de las 4 articulaciones (función `get_joint_positions`) → validación de ángulos → publicación de la trayectoria articular resultante. Este diagrama ayuda a entender el único módulo de IK del repositorio.
