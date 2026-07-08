# Cleanup Notes — Tracked Artifacts

These files are currently tracked in git but should not be. They have been added to `.gitignore`. **Do not run the commands below until you have reviewed them.** They remove files from the git index only—not from disk.

## 1. Python virtual environment (`.venv/`)

**Status:** 2,116 files tracked under `.venv/`

`.venv/` has been added to `.gitignore`. To stop tracking without deleting the local environment:

```bash
cd ~/hovercraft_prj
git rm -r --cached .venv
git commit -m "Stop tracking .venv virtual environment"
```

After this, recreate the venv locally if needed:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install numpy paho-mqtt pyqtgraph matplotlib
```

## 2. Precompiled host binaries

**Status:** Three ELF binaries tracked in `host_tools/`

| File | Size (approx.) |
|------|----------------|
| `host_tools/hovercraft_pipeline` | 872 KB |
| `host_tools/hc_traj_viewer` | 580 KB |
| `host_tools/hovercraft_stop_test` | 432 KB |

These are build artifacts. They have been added to `.gitignore`. To untrack:

```bash
cd ~/hovercraft_prj
git rm --cached host_tools/hovercraft_pipeline host_tools/hc_traj_viewer host_tools/hovercraft_stop_test
git commit -m "Stop tracking precompiled host binaries"
```

> **Note:** Visitors cloning the repo will need build instructions to regenerate these. Add tested CMake commands to the README when available.

## 3. Runtime log file

**Status:** `hovercraft_log.csv` tracked (~198 lines of state transitions)

Added to `.gitignore`. To untrack:

```bash
cd ~/hovercraft_prj
git rm --cached hovercraft_log.csv
git commit -m "Stop tracking runtime log file"
```

## 4. Already clean

The following are **not** tracked (no action needed):

- `__pycache__/` directories
- `*.pyc` files
- ESP-IDF `build/` directory

## Recommended order

Run all `git rm --cached` commands in a single commit:

```bash
cd ~/hovercraft_prj
git rm -r --cached .venv
git rm --cached host_tools/hovercraft_pipeline host_tools/hc_traj_viewer host_tools/hovercraft_stop_test
git rm --cached hovercraft_log.csv
git status   # verify only these paths are staged for removal
git commit -m "Remove tracked venv, binaries, and runtime logs from index"
```

**Do not push** until you are ready. The next push will remove these files from the remote repository history going forward (they remain in past commits unless you rewrite history).
