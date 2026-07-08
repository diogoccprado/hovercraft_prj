# Portfolio Card — Real Hovercraft Control System

**Title:** Real Hovercraft Control System

**Hook:** Embedded + host-side control stack for a physical hovercraft robot with IMU sensing, vision, EKF fusion, and MQTT-based trajectory control.

**Problem:** A hovercraft needs low-latency actuator control on embedded hardware while running perception, state estimation, and trajectory tracking on a host computer—all on a real physical platform.

**What I built:**
- ESP32 firmware: MPU6050 IMU integration, DMP tuning, PWM actuation, Wi-Fi/MQTT
- Host C++ pipeline: multi-threaded EKF, OpenCV vision, PI trajectory controller, teleop/autonomous modes on real hardware
- MQTT communication layer and Dockerized Mosquitto broker setup
- IMU calibration and sensor fusion tuning

**Not my work:** trajectory viewer, IMU realtime plots, route planner GUI.

**Tools:** ESP-IDF, C++, Python, OpenCV, MQTT, MPU6050, ESP32, Docker

**Result/demo:** Pipeline architecture diagram included. Physical platform photos and experiment video TODO.

**Links:**
- Repository: `hovercraft_prj` (recommended rename: `hovercraft-robot-control`)
