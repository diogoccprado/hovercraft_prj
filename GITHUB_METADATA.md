# GitHub Metadata — Recommended Settings

Apply these manually in the GitHub web UI or via `gh` CLI. **Do not run automatically.**

## Recommended repository name

```
hovercraft-robot-control
```

This name reflects a **real physical hovercraft robotics project** (embedded firmware + host control), not a simulation-only repo.

Alternatives:

- `hovercraft-control-system` — emphasizes full software stack
- `hovercraft-embedded-control` — emphasizes ESP32 firmware
- `hovercraft-trajectory-control` — emphasizes trajectory tracking

Avoid `hovercraft-control-simulation` — the physical hovercraft hardware exists.

## Recommended About description

```
Control and software stack for a real hovercraft robot, including embedded firmware, host-side tools, trajectory/control logic, logging, and visualization.
```

Conservative alternative:

```
Real hovercraft robotics project with embedded control, host-side tooling, communication, logging, and trajectory-control experiments.
```

## Recommended topics

```
robotics
hovercraft
embedded-systems
control
trajectory-tracking
esp32
mqtt
python
cplusplus
real-robot
```

## Suggested pinned repo sentence

> Real hovercraft robot: ESP32 firmware, host-side EKF and vision, MQTT communication, and closed-loop trajectory control on physical hardware.

## Manual rename instructions

### Option A: GitHub web UI

1. Go to **Settings → General → Repository name**
2. Change to `hovercraft-robot-control`
3. Click **Rename**

### Option B: GitHub CLI

```bash
gh repo rename hovercraft-robot-control --repo diogoccprado/hovercraft_prj
```

### Update local remote after rename

```bash
cd ~/hovercraft_prj
git remote set-url origin git@github.com:diogoccprado/hovercraft-robot-control.git
```

## Set About description and topics (CLI)

```bash
gh repo edit diogoccprado/hovercraft-robot-control \
  --description "Control and software stack for a real hovercraft robot, including embedded firmware, host-side tools, trajectory/control logic, logging, and visualization." \
  --add-topic robotics --add-topic hovercraft --add-topic embedded-systems \
  --add-topic control --add-topic trajectory-tracking --add-topic esp32 \
  --add-topic mqtt --add-topic python --add-topic cplusplus --add-topic real-robot
```

> Replace `diogoccprado` with your GitHub username if different.
