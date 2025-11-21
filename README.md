# pool-cleaner-teleop-ros2
#Implementación de ROS2, Rviz y Gazebo para un robot acuático teleoperado para la limpieza de la superficie de agua en piscinas
Este repositorio contiene el workspace de ROS 2 (`mi_ws`) para la implementación y simulación de un robot acuático teleoperado para la limpieza del espejo de agua en piscinas.  
Incluye:
- Descripción del robot (URDF/SDF) y lanzadores para RViz y Gazebo.
- Nodos de teleoperación con joystick PS4.
- Simulación 2D del barco.
- Control de una tortuga en `turtlesim` para marcar trayectoria.

Estructura del workspace: 
mi_ws/
├── src/
│   ├── barco_description/   # Modelo del barco, URDF/SDF, lanzadores RViz/Gazebo
│   └── control_tortuga/     # Nodos de teleoperación, simulación 2D, teleop turtle
└── log/                     # Logs de colcon (se pueden borrar sin problema

---

## 1. Requisitos

- Ubuntu 22.04 (recomendado)
- ROS 2 Humble instalado
- Joystick (por ejemplo, control de PS4) conectado por USB
- Paquetes adicionales:

/// COLCON BUILD///
cd ~/mi_ws
colcon build
source install/setup.bash

////////////////////
JOYSTICK
///////////////////
1. CALIBRACION E INSTALACION DE JOYSTICK
1️⃣ Conectar y verificar que Linux ve el mando

Conectar el control:

Por USB 

Ver si el sistema lo reconoce como joystick:
ls /dev/input/js*

Si todo está bien, deberías ver algo como:
/dev/input/js0

Ese js0 es el que usaremos en los parámetros de ROS.

2️⃣ Instalar dependencias necesarias
Paquetes que les debes decir que instalen una sola vez:

sudo apt update

# Nodo de joystick para ROS 2
sudo apt install ros-humble-joy

# Herramientas de prueba/calibración en Linux
sudo apt install joystick jstest-gtk

3️⃣ Calibrar / probar el joystick en Linux (fuera de ROS)
3.1 Herramienta gráfica: jstest-gtk (la que tú decías 👀)
LES HICE VIDEO EXPLICITO DE LA CALIBRACION

jstest-gtk

2. NODO JOY
cd ~/mi_ws
source install/setup.bash

ros2 run joy joy_node \
  --ros-args \
  -p dev:="/dev/input/js0" \
  -p deadzone:=0.05 \
  -p autorepeat_rate:=20.0

3. COMPROBACION NODO JOY
cd ~/mi_ws
source install/setup.bash
ros2 topic echo /joy

/// MANUAL DE TERMINALES ///

1.Cómo lanzar todo paso a paso 
Terminal 1 – Barco + RViz (tu launch de antes)
cd ~/mi_ws
source install/setup.bash
ros2 launch barco_description display.launch.py

Terminal 2 – Nodo del joystick
cd ~/mi_ws
source install/setup.bash
ros2 run joy joy_node
# (si te pide el dispositivo: ros2 run joy joy_node --ros-args -p dev:="/dev/input/js0")

Terminal 3 – Teleop del barco con PS4
cd ~/mi_ws
source install/setup.bash
ros2 run control_tortuga ps4_teleop_barco

Terminal 4 – Simulación 2D del barco
cd ~/mi_ws
source install/setup.bash
ros2 run control_tortuga barco_sim_2d

Terminal 5 - Turtle para marcar la trayectoria.
cd ~/mi_ws
source install/setup.bash
ros2 run turtlesim turtlesim_node

Terminal 6 - Nodo del joystick tortuga
cd ~/mi_ws
source install/setup.bash
ros2 run control_tortuga ps4_teleop_turtle

Terminal 7- Gazebo
cd ~/mi_ws
source install/setup.bash
ros2 launch barco_description barco_gazebo.launch.py


///////////////
CAMARA
//////////////
Para modificaciones de RED
abrir esp32.ino

Para abrir interfaz
abrir esp32normal
```bash
sudo apt update

# Nodo de joystick para ROS 2
sudo apt install ros-humble-joy

# Herramientas de prueba/calibración del joystick en Linux
sudo apt install joystick jstest-gtk
