
# ArduPilot + ROS 2 + Gazebo UAV Simulation

Simulation and flight-control workflow integrating **ArduPilot SITL, ROS 2 Humble, Gazebo Sim, MAVLink and MAVProxy** for UAV testing and autonomous mission execution.

## Overview

This project documents a Software-in-the-Loop (SITL) UAV simulation environment built using ArduPilot, ROS 2 Humble and Gazebo Sim.

The simulated aircraft was configured with ArduCopter and connected to Gazebo through the ArduPilot Gazebo plugin. MAVProxy was used to monitor the vehicle and send flight commands through MAVLink.

The simulation was used to test:

- Flight-controller initialization
- Simulated IMU and GPS data
- ArduPilot EKF initialization
- Vehicle arming
- GUIDED flight control
- Takeoff and landing
- Position and altitude commands
- Autonomous AUTO missions
- Waypoint navigation
- MAVLink communication

## System Architecture

```text
                    ROS 2 Humble
                         │
                    ~/ardu_ws
                         │
                ardupilot_gazebo
                         │
                         ▼
                    Gazebo Sim
                         │
                  ArduPilotPlugin
                         │
                         ▼
                  ArduCopter SITL
                         │
                       MAVLink
                         │
                         ▼
                     MAVProxy
                         │
                 Flight Commands
                         │
                         ▼
                  Simulated UAV
````

## Technologies

* ROS 2 Humble
* ArduPilot / ArduCopter SITL
* Gazebo Sim
* MAVLink
* MAVProxy
* Python
* Ubuntu 22.04

## Simulation Environment

The ROS 2 workspace used for the simulation was:

```text
~/ardu_ws
```

The ArduPilot source tree was maintained separately:

```text
~/ardupilot
```

ROS 2 Humble and the workspace environment were sourced before running the simulation:

```bash
cd ~/ardu_ws

source /opt/ros/humble/setup.bash
source install/setup.bash
```

Gazebo was configured to use the ArduPilot simulation plugins and resources.

## Gazebo Integration

The ArduPilot Gazebo integration provided the simulated UAV model, sensors, physics and ArduPilot plugin.

The simulation used the `iris_runway` world.

The Gazebo log confirmed loading of the ArduPilot plugin:

```text
Loaded system [ArduPilotPlugin]
```

The simulated vehicle also provided sensor data including IMU and GPS through the simulation environment.

## ArduCopter SITL

ArduCopter was run in Software-in-the-Loop mode.

SITL replaces the physical flight controller with a software-based flight controller running on the computer.

```text
Physical System:

Sensors → Pixhawk → Motors

SITL:

Simulated Sensors → ArduCopter SITL → Simulated Motors/Physics
```

The ArduPilot SITL environment was built from:

```text
~/ardupilot
```

## Flight Control

MAVProxy was used as the MAVLink control and monitoring interface.

The simulated vehicle was successfully tested in:

* GUIDED mode
* AUTO mode

Flight operations included:

```text
ARM
 ↓
TAKEOFF
 ↓
MOVEMENT / POSITION COMMANDS
 ↓
WAYPOINT MISSION
 ↓
LAND
 ↓
DISARM
```

## Autonomous Mission Testing

An autonomous mission was tested using ArduPilot's AUTO mode.

The mission workflow included:

```text
TAKEOFF
   ↓
WAYPOINT
   ↓
WAYPOINT
   ↓
LAND
```

The simulated UAV successfully followed the commanded mission.

## Sensor and State Verification

The simulation was used to observe simulated flight-controller data including:

* IMU
* GPS
* Yaw
* Roll
* Pitch
* Altitude
* Velocity
* EKF state
* Flight mode
* Battery state

ArduPilot's EKF initialization and sensor alignment were also observed during startup.

## Gazebo Environment Configuration

Gazebo resource and plugin paths were configured using environment variables such as:

```bash
export GZ_VERSION=harmonic

export GZ_SIM_SYSTEM_PLUGIN_PATH=~/ardu_ws/install/ardupilot_gazebo/lib/ardupilot_gazebo:$GZ_SIM_SYSTEM_PLUGIN_PATH

export GZ_SIM_RESOURCE_PATH=~/ardu_ws/install/ardupilot_gazebo/share:$GZ_SIM_RESOURCE_PATH

export SDF_PATH=$GZ_SIM_RESOURCE_PATH
```

The Gazebo simulation world was launched using:

```bash
gz sim -v4 -r ~/ardu_ws/install/ardupilot_gazebo/share/ardupilot_gazebo/worlds/iris_runway.sdf
```

## Results

The completed simulation demonstrated:

* Successful Gazebo world initialization
* Successful ArduPilot Gazebo plugin loading
* ArduCopter SITL initialization
* Simulated sensor initialization
* MAVLink communication
* Vehicle arming
* GUIDED flight control
* Takeoff and landing
* Waypoint mission execution
* AUTO mode operation

## Media

Screenshots and flight videos from the simulation are included in the `media/` directory.

## Future Work

The next stage of this work is to extend the simulation toward autonomous UAV applications using ROS 2.

Potential extensions include:

* ROS 2 offboard control
* Computer vision
* Object detection
* Target tracking
* Obstacle detection
* Path planning
* Autonomous landing
* Multi-UAV coordination

## Project Context

This project was developed as part of hands-on work with UAV flight-control systems, simulation, ROS 2 and autonomous systems.

It complements physical flight-controller experimentation by providing a safe simulation environment for testing flight-control and autonomy concepts before deployment on real hardware.

```

That way the README stays **professional and readable**, while your actual technical work is still documented underneath it.
```
