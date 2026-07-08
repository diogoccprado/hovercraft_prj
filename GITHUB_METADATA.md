# GitHub Metadata — Recommended Settings

Apply these manually in the GitHub web UI or via `gh` CLI. **Do not run automatically.**

## Recommended repository name

After inspection, the repo spans embedded firmware, host-side control/simulation, and route planning. Best fit:

```
hovercraft-control-simulation
```

Alternatives if you want to emphasize a specific aspect:

- `hovercraft-trajectory-control` — if highlighting trajectory tracking
- `hovercraft-planning-control` — if highlighting the planner GUI + control loop

## Recommended About description

```
Hovercraft trajectory tracking and control simulation with planning, logging, and modular control components.
```

## Recommended topics

```
robotics
control
trajectory-tracking
simulation
python
cplusplus
docker
mobile-robots
```

## Suggested pinned repo sentence

> ESP32 hovercraft with host-side EKF, vision-based localization, MQTT trajectory planning, and closed-loop control.

## Manual rename instructions

### Option A: GitHub web UI

1. Go to **Settings → General → Repository name**
2. Change to `hovercraft-control-simulation`
3. Click **Rename**

### Option B: GitHub CLI

```bash
gh repo rename hovercraft-control-simulation --repo diogoccprado/hovercraft_prj
```

### Update local remote after rename

```bash
cd ~/hovercraft_prj
git remote set-url origin git@github.com:diogoccprado/hovercraft-control-simulation.git
```

## Set About description and topics (CLI)

```bash
gh repo edit diogoccprado/hovercraft-control-simulation \
  --description "Hovercraft trajectory tracking and control simulation with planning, logging, and modular control components." \
  --add-topic robotics --add-topic control --add-topic trajectory-tracking \
  --add-topic simulation --add-topic python --add-topic cplusplus \
  --add-topic docker --add-topic mobile-robots
```

> Replace `diogoccprado` with your GitHub username if different.
