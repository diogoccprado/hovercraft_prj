# GitHub Publication Checklist

Manual steps to finalize [github.com/diogoccprado](https://github.com/diogoccprado) project repos after documentation commits are pushed.

---

## 1. Rename repos on GitHub

| Current name | New name |
|--------------|----------|
| `Robocup-Work-2024` | `robocup-work-robot-software-2024` |
| `hovercraft_prj` | `hovercraft-robot-control` |
| `LiDAR---Camera-Calibration---Annotations-and-Fixes-` | `lidar-camera-calibration-ros2` |

### GitHub web UI

1. Repo → **Settings** → **General** → **Repository name**
2. Enter new name → **Rename**

### GitHub CLI

```bash
gh repo rename robocup-work-robot-software-2024 --repo diogoccprado/Robocup-Work-2024
gh repo rename hovercraft-robot-control --repo diogoccprado/hovercraft_prj
gh repo rename lidar-camera-calibration-ros2 --repo diogoccprado/LiDAR---Camera-Calibration---Annotations-and-Fixes-
```

---

## 2. Update local remotes after GitHub renames

```bash
cd ~/Robocup-Work-2024
git remote set-url origin git@github.com:diogoccprado/robocup-work-robot-software-2024.git

cd ~/hovercraft_prj
git remote set-url origin git@github.com:diogoccprado/hovercraft-robot-control.git

cd ~/LiDAR---Camera-Calibration---Annotations-and-Fixes-
git remote set-url origin git@github.com:diogoccprado/lidar-camera-calibration-ros2.git
```

Verify:

```bash
git remote -v
```

---

## 3. Optional local folder renames

```bash
cd ~
mv Robocup-Work-2024 robocup-work-robot-software-2024
mv hovercraft_prj hovercraft-robot-control
mv LiDAR---Camera-Calibration---Annotations-and-Fixes- lidar-camera-calibration-ros2
```

---

## 4. About descriptions

Paste in **Settings → General → Description** for each repo:

| Repo | About description |
|------|-------------------|
| `robocup-work-robot-software-2024` | Robot software stack for RoboCup@Work experiments with C++/Python ROS packages, launch files, Docker setup, and simulation assets. |
| `hovercraft-robot-control` | Control and software stack for a real hovercraft robot, including embedded firmware, host-side tools, trajectory/control logic, logging, and visualization. |
| `lidar-camera-calibration-ros2` | ROS 2 workflow for LiDAR-camera extrinsic calibration using Livox Mid-360, RealSense D435, rosbag2, SAM masks, and CalibAnything. |

### CLI (after rename)

```bash
gh repo edit diogoccprado/robocup-work-robot-software-2024 \
  --description "Robot software stack for RoboCup@Work experiments with C++/Python ROS packages, launch files, Docker setup, and simulation assets."

gh repo edit diogoccprado/hovercraft-robot-control \
  --description "Control and software stack for a real hovercraft robot, including embedded firmware, host-side tools, trajectory/control logic, logging, and visualization."

gh repo edit diogoccprado/lidar-camera-calibration-ros2 \
  --description "ROS 2 workflow for LiDAR-camera extrinsic calibration using Livox Mid-360, RealSense D435, rosbag2, SAM masks, and CalibAnything."
```

---

## 5. Topics

### robocup-work-robot-software-2024
`robocup`, `robocup-at-work`, `robotics`, `ros`, `mobile-manipulation`, `navigation`, `cplusplus`, `python`, `cmake`, `docker`

### hovercraft-robot-control
`robotics`, `hovercraft`, `embedded-systems`, `control`, `trajectory-tracking`, `esp32`, `mqtt`, `python`, `cplusplus`, `real-robot`

### lidar-camera-calibration-ros2
`ros2`, `lidar`, `camera-calibration`, `extrinsic-calibration`, `livox`, `realsense`, `calibanything`, `segment-anything`, `computer-vision`, `robotics`

---

## 6. Suggested pinned repo order

After the three repos above are renamed:

1. `genesis-go2-sim`
2. `lidar-camera-calibration-ros2`
3. `hovercraft-robot-control`
4. `robocup-work-robot-software-2024`
5. _Later:_ `adaptive-odom-filter-ros2`
6. _Later:_ `semantic-mapping-nav` / ARGUS

Do **not** pin `diogoccprado.github.io` yet — website not published in this phase.

---

## 7. Media TODOs

| Repo | Add later |
|------|-----------|
| **Hovercraft** | Photo/video of physical platform, control/logging screenshot, trajectory plot |
| **RoboCup** | Robot/team photo, competition or demo video, launch/RViz screenshot |
| **LiDAR** | RViz alignment screenshot, calibration before/after projection image, transform example |

---

## 8. Profile settings (after website phase)

- [ ] Set GitHub profile bio
- [ ] Add portfolio URL to profile website field
- [ ] Create and push `diogoccprado` profile README repo
- [ ] Deploy `diogoccprado.github.io`

---

_Last updated: 2026-07-08_
