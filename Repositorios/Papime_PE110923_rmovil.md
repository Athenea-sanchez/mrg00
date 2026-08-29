# Papime_PE110923_rmovil

> Repositorio de **robótica móvil** del proyecto PAPIME PE110923 (UNAM, Facultad de Ingeniería).

- **Ruta:** `repositorios/Papime_PE110923_rmovil`
- **Remoto git:** `https://github.com/arrg-mx/Papime_PE110923_rmovil`
- **Rama:** `main`
- **Tamaño:** ~156 MB (240 archivos), mayormente mallas STL.
- **Licencia:** MIT (raíz).

> **Control de versiones (fecha de esta documentación):** al día de hoy, **28 de agosto de 2026**, el repositorio se encuentra en la rama `main` con el último commit `f505ba4` (*"Update ros2_basics.md"*) fechado el **11 de junio de 2025**, sincronizado con su remoto `origin` (trabajo limpio, sin cambios pendientes). Esta ficha se creó fuera del repositorio (en `repositorios/documentacion/`) y no modifica su historial git.

---

## 1. Qué es

Es la parte **móvil** del proyecto **PAPIME PE110923** ("Crear un laboratorio de Robótica Remota para la realización de prácticas de teleoperación, planeación de rutas y coordinación de movimientos y transporte de materiales para robótica y robótica móvil en niveles de licenciatura y posgrado").

Contiene **paquetes de simulación ROS 2** y **material de enseñanza** para robots móviles: diferenciales, omnidireccionales y manipuladores móviles, más cuadernos Jupyter y prácticas. Es uno de los tres repos relacionados (los otros son `Papime_PE110923_rserial` para robots seriales y `ros2-docs` para redes).

## 2. Qué hace y cómo es, en general

Permite simular y probar:

- **Robot diferencial (2,0)** — paquetes `diff_pkgs`.
- **Robot omnidireccional** — paquetes `omni_pkgs` (también lo usan los alumnos como ejercicio).
- **Robots de laboratorio ARRG** (`robo_lab`): brazo **Dofbot**, base **omni (X3)**, y el **omni-dofbot** (manipulador móvil).
- **Prácticas de curso** (8) y **bases de ROS 2** en Python y C++.

El control se basa en `ros2_control`/`ros2_controllers` con controladores de velocidad y de trayectoria articular.

## 3. Tecnologías

| Tecnología | Uso |
|---|---|
| ROS 2 Humble | Paquetes de simulación (launch, `package.xml`) |
| Python 3 | Launch `.py`, nodos, tests |
| C++ | Nodos en `ros2_basics` |
| Xacro / URDF | Descripciones de robots |
| YAML | Config de controladores |
| RViz | Visualización |
| Gazebo | Mundos de simulación |
| STL | Mallas 3D (hasta ~13 MB) |
| Jupyter | Cuadernos educativos |

## 4. Estructura de carpetas

```
Papime_PE110923_rmovil/
├── README.md                  # Visión general (español)
├── LICENSE                    # MIT
├── docs/                      # 6 notebooks + ~50 capturas
├── diff_pkgs/                 # Robot diferencial
│   ├── diff_bringup/          # launch, rviz, world
│   └── diff_description/      # URDF/Xacro
├── omni_pkgs/                 # Robot omnidireccional
│   ├── omnidirectional_bringup/
│   └── omnidirectional_description/
├── robo_lab/                  # Robots de laboratorio ARRG
│   ├── dofbot_bringup/  +  dofbot_description/
│   ├── omni_bringup/  +  omni_description/
│   └── omni_dofbot_bringup/  +  omni_dofbot_description/
├── practicas/                 # 8 prácticas (Jupyter)
└── ros2_basics/               # Bases ROS 2
    ├── nodes_topics_py/       # Python
    ├── nodes_topics_cpp/      # C++
    └── messages_services_pkg/ # msg/srv personalizados
```

## 5. Documentos presentes (para qué sirven)

- **README.md** — objetivo, personal, índice de contenidos, enlace a VM (OneDrive), enlaces a repos hermanos.
- **LICENSE** — MIT.
- **13 archivos `.md`** de apoyo: `docs/jupyter_docs.md`, `diff_pkgs.md`, `omni_pkgs.md`, `robo_lab.md` (el más completo: instalación/ejecución y solución de problemas de Gazebo), `ros2_basics.md`, `practicas.md`.
- **14 notebooks** (6 en `docs/`, 8 prácticas).
- **Placeholders sin valor:** varios `borrame.md` y `Readme.md` vacíos (incluido uno dentro de una carpeta de mallas), y un `docs/jupyter_docs,md` con coma en el nombre (duplicado erróneo de `jupyter_docs.md`).

## 6. Lo que se asumiría que hay (pero falta)

- **`.gitignore`** — no existe (importante por las ~156 MB de STL, outputs de notebooks y artefactos `build/install/log`).
- **`CHANGELOG.md` / versionado** — ausente.
- **`CONTRIBUTING.md`** — sin guías de colaboración.
- **Metadata de `package.xml`** — varios paquetes con `<description>TODO: Package description</description>` y `<license>TODO: License declaration</license>`.
- **Notebook referenciado ausente** — `jupyter_docs.md` menciona `Robotica_serial_control.ipynb` que pertenece al repo serial y no está aquí.

## 7. Qué está mal y cómo se corregiría

### Ejecución
1. **`setup.py` de `nodes_topics_py`** registra el entry point `pub_node_py = nodes_topics_py.pub_node_py:main`, pero el archivo en disco es `node_pub_py.py` → el entry point no coincide y **fallaría el build**. Renombrar o corregir `setup.py`.
2. **`messages_services_pkg`**: `src/servicio_server.cpp` es duplicado huérfano (no se compila) → eliminar.
3. **`omnidirectional_bringup/CMakeLists.txt`** instala un directorio `scripts` inexistente → eliminar de `DIRECTORY`.

### Paths y consistencia
4. **Ruta absoluta hardcodeada** en `omnidirectional_description/package.xml`: `gazebo_model_path="/home/robousr/ROS2Dev/omnidirectional_ws/install/..."` → usar `$(find ...)`.
5. **`setup.py`** con `maintainer_email='robousr@todo.todo'` y `description='TODO: ...'` → rellenar.

### Duplicados / nomenclatura
6. Dos implementaciones paralelas de lo mismo: `diff_pkgs`/`omni_pkgs` y `robo_lab` — confuso. Unificar o documentar cuál usar.
7. Estilos de launch **inconsistentes**: `omni_pkgs` usa el binario `gazebo` antiguo y API `xacro` directa; `robo_lab` usa `gazebo_ros`/`IncludeLaunchDescription` moderno → estandarizar.
8. Mundos duplicados: `test_w_1.world` en `diff_bringup` y `omnidirectional_bringup`; `Mundo_mesa_y_cajas.world` en omni y omni_dofbot → consolidar.

### Typos / archivos basura
9. `docs/Nodes _Topics.ipynb` (espacio antes de `_`) inconsistente con `Nodos_y_Topicos.ipynb`.
10. Typos en nombres: `template_pytnon_node.py`, `omnidirectional_gz_propierties.xacro`, `omni_dofbot_trayectory_rviz.rviz`.
11. `docs/jupyter_docs,md` (coma) `= jupyter_docs.md` → eliminar el duplicado.
12. `robo_lab/.../borrame.md` y `Readme.md` vacíos → eliminar.
13. **Docs desincronizadas:** `omni_pkgs.md` tiene encabezado de "robot diferencial" pero trata del omnidireccional; `jupyter_docs.md` pertenece al repo serial.

## 8. Diagrama sugerido (descripción en texto)

Conviene un **diagrama de arquitectura de paquetes + flujo de ejecución** dividido en dos partes:

1. **Mapa de dependencias entre paquetes**: un diagrama de cajas que muestre las tres familias (`diff_pkgs`, `omni_pkgs`, `robo_lab`) y, dentro de `robo_lab`, cómo `omni_dofbot_description` reutiliza mallas/macros de `dofbot_description` y `omni_description` mediante `$(find ...)`. Esto ayudaría a ver qué reutilizar y qué consolidar.

2. **Flujo de arranque de un robot móvil**: proceso en cascada desde la descripción URDF/Xacro → `robot_state_publisher` (publica `/tf` y `/joint_states`) → carga del mundo en Gazebo con `spawn_entity` → activación del controlador de velocidad (`ros2_control`) → envío de órdenes con un script/nodo de prueba → RViz para visualización. A este flujo se le añadiría una nota sobre qué launch usar según la familia (diff, omni o dofbot).
