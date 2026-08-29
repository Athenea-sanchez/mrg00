# Material_base_clases

> Repositorio de material base para las clases de robótica UNAM (Papime PE110923).

- **Ruta:** `repositorios/Material_base_clases`
- **Remoto git:** `https://github.com/arrg-mx/Material_base_clases`
- **Rama:** `main` · **Commits:** ~21 (2025-02-16 → 2025-08-27)
- **Idioma de la documentación:** español (español / ROS 2).

> **Control de versiones (fecha de esta documentación):** al día de hoy, **28 de agosto de 2026**, el repositorio se encuentra en la rama `main` con el último commit `9ee0ce0` (*"Captura de datos"*) fechado el **27 de agosto de 2025**, sincronizado con su remoto `origin` (trabajo limpio, sin cambios pendientes). Esta ficha se creó fuera del repositorio (en `repositorios/documentacion/`) y no modifica su historial git.

---

## 1. Qué es

Es un repositorio de **material docente** para los cursos de robótica de la UNAM (Facultad de Ingeniería), dentro del proyecto **PAPIME PE110923** ("Desarrollo de un laboratorio de robótica remoto..."). Reúne cuadernos de teoría, prácticas y simulaciones construidas con **ROS 2 + Gazebo**.

El README raíz es muy breve (2 líneas): *"Archivos de material base para las clases de robótica UNAM"*.

## 2. Qué hace y cómo es, en general

Proporciona una progresión de aprendizaje de robótica con ROS 2:

1. **Conceptos básicos** (RViz/Gazebo, plugins, articulaciones).
2. **Nodos y tópicos** y **mensajes/servicios** personalizados.
3. **Prácticas** de construcción de URDF y nodos en C++ y Python.
4. **Robots simulados:** diferencial (`my_robot` / `diff_robot`), omnidireccional de 4 ruedas, **SCARA** (examen) y **Scorbot ER5**.
5. **Exámenes / aplicaciones** de control de posición y trayectoria para SCARA.

## 3. Tecnologías

| Tecnología | Uso |
|---|---|
| ROS 2 (Humble, `ament_cmake` / `ament_python`) | Framework principal · 15 paquetes `package.xml` |
| C++ (`rclcpp`) | 8 archivos `.cpp` (nodos, servicios) |
| Python (`rclpy`) | ~35 archivos `.py` (nodos, launch, tests) |
| URDF / Xacro | 41 `.xacro` + 4 `.urdf` |
| Gazebo (SDF) | 5 `.world` + modelos `.sdf` |
| RViz | 14 configs `.rviz` |
| YAML | 6 configuraciones de controladores |
| Jupyter | 7 `.ipynb` |
| STL | 5 mallas para Scorbot |
| CMake | `CMakeLists.txt` en cada paquete |

## 4. Estructura de carpetas

```
Material_base_clases/
├── README.md                          # Descripción de 2 líneas
├── message_and_services.ipynb         # Interfaces personalizadas msg/srv
├── ROS2+For+Beginners+Level+2+-+Final+Project+Instructions.pdf
├── Materiales/                        # Material de clase
│   ├── Conceptos_basicos.ipynb
│   ├── Nodos_y_Topicos.ipynb
│   ├── Robotica_serial_control.ipynb  # Control SCARA
│   ├── Robot_movil_diferencial.ipynb
│   ├── Sensores_basicos.ipynb
│   └── imagenes/                      # 49 capturas PNG
├── dofbotexperimental/                # Utilidad de captura DOFBOT
├── demo_pkg/                          # Tutorial Python (pub/sub/params)
├── practica1_description/             # Primer URDF
├── practica_2_cpp/                    # Nodos C++
├── practica_2_py/                     # Nodos Python
├── my_robot_bringup/  y  my_robot_description/   # Robot diferencial v1
├── diff_bringup/  y  diff_description/           # Robot diferencial v2
├── omnidirectional_bringup/  y  omnidirectional_description/  # Omni
├── examen_bringup/  y  examen_description/       # Examen / SCARA
├── scorbot_bringup/  y  scorbot_description/     # Scorbot ER5
└── srv_act_pkg/                       # Interfaces msg/srv + ejemplos C++
```

## 5. Documentos presentes (para qué sirven)

- **README.md** — solo el título y una línea de descripción.
- **`ROS2+For+Beginners+Level+2+-+Final+Project+Instructions.pdf`** — instrucciones del proyecto final del curso "ROS 2 for Beginners Level 2".
- **7 notebooks `.ipynb`** — teoría y ejercicios (ver sección 4).
- Placeholders sin valor: `dofbotexperimental/Readme.md` ("Saludos"), `srv_act_pkg/README.md` ("Interfaces") y varios `Readme.md` vacíos.

## 6. Lo que se asumiría que hay (pero falta)

- **`.gitignore`** — no existe; de hecho hay un `__pycache__/` commiteado.
- **`LICENSE`** — no hay licencia a nivel raíz (solo `demo_pkg` y `srv_act_pkg` declaran MIT en `package.xml`; el resto usa `TODO: License declaration`).
- **README completo** — no hay instrucciones de build (`colcon build`), dependencias ni cómo ejecutar.
- **CI / `.github/`** — ausente.
- **Dependencia `my_interface`** — `demo_pkg` la declara e importa (`from my_interface.msg import ...`) pero **no existe en el repo**, impidiendo build/run tal cual.
- **`src/` de workspace** — los paquetes están en la raíz sin un wrapper de workspace ni nota.

## 7. Qué está mal y cómo se corregiría

### Build / ejecución (crítico)
1. **`omnidirectional_bringup/CMakeLists.txt`** instala un directorio `scripts` que **no existe** → falla `colcon build`. Eliminarlo de `DIRECTORY`.
2. **`srv_act_pkg/CMakeLists.txt`** instala un directorio `launch` que **no existe** → eliminar.
3. **`demo_pkg`** depende de `my_interface` inexistente → crear el paquete o eliminar la dependencia.
4. **`examen_bringup`/`scorbot_bringup`**: el CMake usa `find_package(rclpy/control_msgs/...)` que sus `package.xml` **no declaran** → añadir dependencias al manifiesto.

### Typos / inconsistencias
5. Launches SCARA referencian `rviz/scara_trayectory_rviz.rviz`, pero el archivo real es `scara_trajectory_controller_rviz.rviz` → corregir nombre.
6. `tes2_...launch.py` usa `world/test_2_world.wold` (debería ser `.world`).
7. Se hace spawn del SCARA con nombre de entidad `'dofbot'` (erróneo, copia/pega) en varios launches.
8. `srv_act_pkg/src/servicio_server.cpp` es un duplicado huérfano que no se compila → eliminarlo.
9. `position_test.py` no se instala en `examen_bringup/CMakeLists.txt` → no corre desde install space.
10. Nombre de archivo con typos: `tes2_trayectory_controller_scara.launch.py` ("tes2"/"trayectory").

### Organización
11. Archivos duplicados byte-idénticos: `test_w_1.world` en tres paquetes; `arm.xacro`/`arm_gazebo.xacro` idénticos en `my_robot_*` y `diff_*`. Consolidar y compartir.
12. Dos stacks diferenciales paralelos (`my_robot_*` y `diff_*`) muy solapados → unificar.
13. Nombres de carpeta inconsistentes: `worlds/` vs `world/`.
14. `__pycache__/` commiteado → añadir `.gitignore`.
15. Tests de boilerplate (copyright/flake8/pep257) sin valor y sin tests reales de los nodos.
16. Comentarios informales/poco profesionales (nota tipo oración "In nomine patris...", "Comment 4 robots mobiles") en algunos launches.
17. Ruta absoluta hardcodeada en `examen_description/package.xml` (`/home/robousr/...`) → usar `$(find ...)`.
18. Scripts Python repartidos entre `src/`, `scripts/` y subcarpetas de módulo sin criterio → estandarizar.

## 8. Diagrama sugerido (descripción en texto)

Para documentar este repo conviene generar **dos diagramas**:

1. **Flujo de build y ejecución del workspace ROS 2**: un diagrama de proceso en cascada que arranque en `colcon build`, continúe con `source install/setup.bash`, luego la selección del paquete de `bringup` (p. ej. `my_robot_bringup`), el lanzamiento de `robot_state_publisher` que publica el URDF/xacro, la carga del mundo en **Gazebo** con el `spawn_entity`, y finalmente la apertura de **RViz** para visualización. Este diagrama serviría de "mapa de arranque" para un estudiante.

2. **Grafo de nodos/tópicos ROS 2** para un robot diferencial: representar cada nodo como una caja (robot_state_publisher, joint_state_publisher, gazebo_ros control, nodo test de trayectoria, RViz) y las flechas como los tópicos publicados/suscritos (por ejemplo `/cmd_vel`, `/odom`, `/joint_states`, `/tf`). Este diagrama aclara cómo fluye la información entre la simulación y los controladores.
