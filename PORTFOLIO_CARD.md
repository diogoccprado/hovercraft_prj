# Portfolio Card — Hovercraft Trajectory Control

**Title:** Hovercraft Trajectory Control and Simulation

**Hook:** ESP32 + host-PC hovercraft autonomy stack with IMU/vision fusion, MQTT trajectory planning, and closed-loop control.

**Problem:** A hovercraft needs low-latency actuator control on embedded hardware while running heavier perception, state estimation, and trajectory tracking on a host computer.

**What I built:**
- ESP32 firmware: MPU6050 IMU integration, DMP tuning, PWM actuation, Wi-Fi/MQTT
- Host C++ pipeline: multi-threaded EKF, OpenCV vision, PI trajectory controller, teleop/autonomous modes
- MQTT message flow and Dockerized Mosquitto broker setup
- IMU calibration and sensor fusion tuning

**Not my work:** trajectory viewer, IMU realtime plots, route planner GUI (visualization layer).

**Tools:** ESP-IDF, C++, Python, OpenCV, MQTT, MPU6050, Docker, tkinter, pyqtgraph

**Result/demo:** Pipeline architecture diagram included. Demo video to be added.

**Links:**
- Repository: `hovercraft_prj` (recommended rename: `hovercraft-control-simulation`)
