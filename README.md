# Hovercraft Trajectory Control and Simulation

Autonomous hovercraft control stack with ESP32 firmware, host-side sensor fusion, trajectory following, and MQTT-based route planning.

## Project goal

Build a closed-loop hovercraft control system that:

1. Reads IMU data on an ESP32 and drives motor/servo actuators
2. Fuses IMU and camera vision on a host PC for pose estimation
3. Follows planned trajectories in autonomous mode or accepts teleop input
4. Publishes routes from a GUI planner over MQTT

The ESP32 `controller` and `route_planner` components are stubs; planning and control logic currently run on the host.

## Main components

| Component | Path | Role |
|-----------|------|------|
| **ESP32 firmware** | `main/main.cpp` | MPU6050 IMU (DMP), PWM motor/servo, Wi-Fi, MQTT publish/subscribe |
| **Host pipeline** | `host_tools/hovercraft_pipeline.cpp` | Multi-threaded: OpenCV vision, EKF fusion, trajectory tracking, teleop/autonomous modes |
| **Trajectory viewer** | `host_tools/hovercraft_traj_viewer.cpp` | OpenCV visualization of reference trajectory and estimated pose |
| **Stop test** | `host_tools/hovercraft_stop_test.cpp` | Vision-based motor braking test |
| **Route planner GUI** | `interfacePlanTraj/interface.py` | Tkinter/matplotlib grid UI; saves routes and publishes via MQTT |
| **IMU viewers** | `host_tools/mpu_realtime*.py` | Real-time IMU plotting from MQTT (pyqtgraph) |
| **Circular buffer** | `bufferCircular/` | Host-side C++ message buffer library |
| **Docker setup** | `docker/` | Mosquitto broker + host container |

### MQTT topics (hardcoded to `broker.hivemq.com:1883`)

| Topic | Direction | Payload |
|-------|-----------|---------|
| `hovercraft/imu` | ESP → host | Yaw/pitch/roll JSON |
| `hovercraft/cmd` | host → ESP | `{"motor_us":..., "servo_us":...}` |
| `hovercraft/traj` | planner → pipeline | Route waypoints JSON |
| `hovercraft/ekfstate` | pipeline → viewer | Estimated pose |
| `hovercraft/soli_traj` | pipeline → planner | Route request (`"PEDIR"`) |

## Tech stack

| Layer | Technologies |
|-------|-------------|
| Embedded | ESP-IDF, FreeRTOS, C/C++, MPU6050, I2C, LEDC PWM, Wi-Fi, MQTT |
| Host C++ | OpenCV, libmosquitto, nlohmann/json |
| Host Python | tkinter, matplotlib, paho-mqtt, pyqtgraph, numpy |
| Messaging | MQTT (Eclipse Mosquitto in Docker; public HiveMQ broker in code) |
| Container | Docker (Ubuntu 22.04), docker-compose |

## Repository structure

```
hovercraft_prj/
├── main/                   # ESP32 firmware entry
├── components/             # ESP-IDF components (IMU, MQTT, controller stub, planner stub)
├── host_tools/             # Host C++ pipeline, viewers, Python IMU tools
├── interfacePlanTraj/      # Route planning GUI
├── bufferCircular/         # C++ circular message buffer
├── docker/                 # Dockerfile + docker-compose (Mosquitto broker)
└── hovercraft_log.csv      # Runtime state log (should not be tracked — see CLEANUP_NOTES.md)
```

## How to run

### ESP32 firmware

Requires ESP-IDF (`$IDF_PATH` set). Wi-Fi credentials are in `main/main.cpp` (`WIFI_SSID`, `WIFI_PASS`). Copy `components/net_mqtt/include/secrets.example.h` to `secrets.h` if needed.

```bash
idf.py build flash monitor
```

### Host pipeline (main control loop)

Requires camera (`/dev/video0`), reachable MQTT broker, and keyboard for mode selection (`1` = autonomous, `2` = teleop).

```bash
./host_tools/hovercraft_pipeline
```

Precompiled binaries are present in `host_tools/`. To rebuild from source:

```bash
# TODO: add tested CMake build commands — Docker Dockerfile has commented build steps
```

### Trajectory viewer

```bash
./host_tools/hc_traj_viewer
```

### Route planning GUI

```bash
cd interfacePlanTraj
python3 interface.py
# Dependencies: tkinter, matplotlib, paho-mqtt
```

### IMU realtime viewer

```bash
python3 host_tools/mpu_realtime.py --broker broker.hivemq.com --topic hovercraft/imu
```

### Docker (MQTT broker)

```bash
cd docker
docker compose up
```

Starts Mosquitto on port 1883 and a host container. C++ build steps in the Dockerfile are commented out.

> **TODO:** Add `requirements.txt` and tested end-to-end setup from a clean clone.

## Results / demo

![Hovercraft pipeline architecture](https://github.com/user-attachments/assets/342b6715-d2ee-475d-9af4-81c8fd6b2b04)

<!-- TODO: Add trajectory tracking video, EKF plot, or competition/demo footage -->

- [ ] Trajectory following demo video
- [ ] EKF vs. ground-truth plot
- [ ] Route planner GUI screenshot

## My contribution

Sole developer of the hovercraft autonomy stack, except for the **web/visualization tools** (trajectory viewer, IMU realtime plots, route planner GUI).

| Area | Contribution |
|------|--------------|
| **ESP32 firmware** | IMU (MPU6050) integration, DMP tuning, PWM motor/servo actuation, Wi-Fi/MQTT telemetry |
| **IMU tuning** | MPU6050 calibration, DMP configuration, and sensor fusion input preparation |
| **Host control pipeline** | Multi-threaded C++ pipeline (`hovercraft_pipeline.cpp`): EKF, vision processing, trajectory tracking, teleop/autonomous modes |
| **Controller** | PI-style trajectory tracking controller and actuator command publishing |
| **Threading architecture** | Parallel IMU, vision, control, and MQTT communication threads |
| **MQTT integration** | Topic design and message flow between ESP32 and host |
| **Docker / broker setup** | Mosquitto broker container configuration |

**Not my work:** `hovercraft_traj_viewer.cpp`, `mpu_realtime*.py`, `interfacePlanTraj/interface.py` (visualization and route planner GUI).

## Status

| Component | Status |
|-----------|--------|
| ESP32 IMU + actuation | Implemented |
| Host EKF + vision pipeline | Implemented |
| Trajectory following (host) | Implemented |
| Route planner GUI | Implemented |
| ESP32 controller/planner stubs | TODO in firmware |
| Documentation | README and portfolio docs |
| Repo hygiene | `.venv/`, binaries, and logs removed from tracking (see `CLEANUP_NOTES.md`) |

## License

See [LICENSE](LICENSE).
