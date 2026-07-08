# Real Hovercraft Control System

Control and software stack for a **physical hovercraft robot**, combining ESP32 embedded firmware, host-side perception and control, and MQTT-based communication.

## Project goal

Develop a closed-loop autonomy stack for a real hovercraft platform that:

1. Senses attitude on-board (MPU6050 IMU via ESP32) and actuates motor/servo via PWM
2. Runs state estimation, vision, and trajectory control on a host PC
3. Executes autonomous trajectory following or manual teleop on the physical platform
4. Logs runtime state and supports route planning through companion visualization tools

This is a **real robotics project** with physical hardware—not a simulation-only codebase.

## System architecture

```
┌─────────────────────┐     MQTT (Wi-Fi)      ┌──────────────────────────────┐
│  ESP32 (on-board)   │ ◄──────────────────► │  Host PC                     │
│  MPU6050 IMU        │   hovercraft/imu     │  hovercraft_pipeline.cpp     │
│  Motor + Servo PWM  │   hovercraft/cmd     │  EKF + vision + controller   │
└─────────────────────┘                      │  (multi-threaded)            │
                                             └──────────┬───────────────────┘
                                                        │ MQTT
                                             ┌──────────▼───────────────────┐
                                             │  Route planner GUI (optional) │
                                             │  Trajectory / IMU viewers     │
                                             └──────────────────────────────┘
```

The ESP32 handles low-latency sensing and actuation. The host runs heavier perception, fusion, and control. Planning and control logic currently execute on the host; ESP32 `controller` and `route_planner` components remain stubs.

## Hardware / software components

### Physical platform

| Component | Role |
|-----------|------|
| **Hovercraft body** | Physical platform under test |
| **ESP32** | On-board microcontroller (firmware in `main/`) |
| **MPU6050 IMU** | Attitude sensing with DMP (yaw/pitch/roll) |
| **ESC + servo** | Thrust and steering actuation via PWM |
| **Host PC + camera** | Vision-based localization and control loop (`/dev/video0`) |

### Software stack

| Component | Path | Role |
|-----------|------|------|
| **ESP32 firmware** | `main/main.cpp` | IMU read, PWM actuation, Wi-Fi, MQTT telemetry |
| **Host pipeline** | `host_tools/hovercraft_pipeline.cpp` | Multi-threaded EKF, OpenCV vision, trajectory tracking, teleop/autonomous modes |
| **Trajectory viewer** | `host_tools/hovercraft_traj_viewer.cpp` | Reference trajectory and estimated pose visualization |
| **Stop test** | `host_tools/hovercraft_stop_test.cpp` | Vision-based motor braking experiments |
| **Route planner GUI** | `interfacePlanTraj/interface.py` | Grid-based route creation and MQTT publish |
| **IMU viewers** | `host_tools/mpu_realtime*.py` | Real-time IMU plotting from MQTT |
| **Message buffer** | `bufferCircular/` | Host-side circular buffer library |
| **Docker broker** | `docker/` | Local Mosquitto MQTT broker for development |

## Communication pipeline

MQTT broker: `broker.hivemq.com:1883` (hardcoded; local Mosquitto available via Docker).

| Topic | Direction | Payload |
|-------|-----------|---------|
| `hovercraft/imu` | ESP → host | Yaw/pitch/roll JSON |
| `hovercraft/cmd` | host → ESP | `{"motor_us":..., "servo_us":...}` |
| `hovercraft/traj` | planner → pipeline | Route waypoints JSON |
| `hovercraft/ekfstate` | pipeline → viewer | Estimated pose |
| `hovercraft/soli_traj` | pipeline → planner | Route request (`"PEDIR"`) |

## Main tools and scripts

| Tool | Language | Purpose |
|------|----------|---------|
| `hovercraft_pipeline` | C++ | Main closed-loop control on physical platform |
| `hc_traj_viewer` | C++ | Trajectory visualization during experiments |
| `hovercraft_stop_test` | C++ | Braking / vision stop experiments |
| `interface.py` | Python | Route planning GUI |
| `mpu_realtime.py` | Python | Live IMU monitoring over MQTT |
| `idf.py` | ESP-IDF | Build and flash ESP32 firmware |

**Tech stack:** ESP-IDF, FreeRTOS, C/C++, OpenCV, libmosquitto, Python (paho-mqtt, pyqtgraph, tkinter), Docker.

## How to run / build

### ESP32 firmware

Requires ESP-IDF (`$IDF_PATH`). Wi-Fi credentials in `main/main.cpp`. Copy `components/net_mqtt/include/secrets.example.h` to `secrets.h` if needed.

```bash
idf.py build flash monitor
```

### Host control pipeline (physical experiments)

Requires camera (`/dev/video0`), reachable MQTT broker, and keyboard input (`1` = autonomous, `2` = teleop).

```bash
./host_tools/hovercraft_pipeline
```

Writes state transitions to `hovercraft_log.csv` during runs.

Precompiled binaries exist in `host_tools/`. Rebuild instructions:

```bash
# TODO: add tested CMake build commands — Docker Dockerfile has commented build steps
```

### Companion tools

```bash
./host_tools/hc_traj_viewer                              # trajectory viewer
python3 host_tools/mpu_realtime.py --broker broker.hivemq.com --topic hovercraft/imu
cd interfacePlanTraj && python3 interface.py             # route planner GUI
cd docker && docker compose up                           # local Mosquitto broker
```

> **TODO:** Add `requirements.txt` and tested end-to-end setup from a clean clone.

## Results / demo

![System pipeline architecture](https://github.com/user-attachments/assets/342b6715-d2ee-475d-9af4-81c8fd6b2b04)

<!-- TODO: Add photos of physical hovercraft, trajectory tracking video, EKF plots -->

- [ ] Photo of physical hovercraft platform
- [ ] Autonomous trajectory following video (real hardware)
- [ ] EKF / control performance plot
- [ ] Route execution log excerpt

## My contribution

Sole developer of the hovercraft autonomy stack, except for the **visualization / web tools** (trajectory viewer, IMU realtime plots, route planner GUI).

| Area | Contribution |
|------|--------------|
| **ESP32 firmware** | MPU6050 integration, DMP tuning, PWM motor/servo actuation, Wi-Fi/MQTT |
| **IMU tuning** | Calibration, DMP configuration, fusion input preparation |
| **Host control pipeline** | Multi-threaded `hovercraft_pipeline.cpp`: EKF, vision, trajectory tracking, teleop/autonomous modes |
| **Controller** | PI-style trajectory tracking and actuator command publishing |
| **Threading** | Parallel IMU, vision, control, and MQTT communication threads |
| **MQTT integration** | Topic design and ESP32 ↔ host message flow |
| **Docker / broker** | Mosquitto container for local development |

**Not my work:** `hovercraft_traj_viewer.cpp`, `mpu_realtime*.py`, `interfacePlanTraj/interface.py`.

## Status and next steps

| Component | Status |
|-----------|--------|
| Physical hovercraft platform | Available |
| ESP32 IMU + actuation | Implemented |
| Host EKF + vision + control | Implemented |
| Trajectory following (real hardware) | Implemented |
| Route planner / viewers | Present (not authored by me) |
| ESP32 controller/planner stubs | TODO in firmware |
| Documentation | README and portfolio docs |
| Repo hygiene | `.venv/`, binaries, logs untracked (see `CLEANUP_NOTES.md`) |

**Next steps:**
- [ ] Add physical platform photos and experiment videos
- [ ] Document CMake rebuild for host binaries
- [ ] Add `requirements.txt` for Python tools

## License

See [LICENSE](LICENSE).
