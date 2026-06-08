## `jetson-remote-dev` Skill

A Kilocode-ready agentic skill for parallel host ↔ Jetson Orin Nano development. Install the `.skill` file via the Kilocode extension in Cursor.

### What the skill covers

**§1–2 — Environment & layout convention**
Defines `.jetson-env` (your device config file), a standard project directory structure, and the core rule: engine files *only* ever live in `models/engines/` and *only* get built on the Jetson.

**§3 — SSH setup**
SSH multiplexing config (`ControlMaster`) so repeated agent SSH calls don't incur new handshake overhead. Agent always does a connectivity check before any remote task.

**§4 — rsync sync strategy**
Bidirectional sync with proper excludes. Includes a watch-mode pattern for live file-change-triggered sync during active editing sessions.

**§5 — Engine build workflow**
Full `trtexec` based engine build sequence: ONNX export on host → sync ONNX → SSH build on Jetson → pull engine back. Includes a ready-to-use `build_engine.sh` template and time estimates for Orin Nano.

**§6 — DeepStream pipeline testing**
SSH-exec patterns for bare Python and Docker-containerized pipelines, live log tailing, GST_DEBUG output filtering.

**§7 — Agentic dev loop**
The core loop diagram + a `dev_cycle.sh` the agent runs after every code change: validate → sync → remote test → fetch logs → parse.

**§8 — Monitoring & debugging**
`tegrastats`, GStreamer plugin checks, `pyds` binding checks, and a `parse_test_result.py` pattern for automated log error detection.

**§9 — Docker workflow**
`buildx` cross-compile on host → push to your local registry (`192.168.1.50:5000`) → pull and run on Jetson. Includes an incremental update pattern (volume-mount src/, no rebuild).

**§10–11 — `.jetson-env` template + agent decision tree**
Pre-filled with your project's values (`bhojm`, `bhojm-jetson-1`, `192.168.1.50`). The decision tree tells the agent which section to jump to based on task type.

**Reference files** (loaded on demand):
- `deepstream-config-snippets.md` — DS 7.x `nvinfer`, `nvtracker` (DeepOC-SORT), RTSP source/sink config blocks
- `tensorrt-troubleshooting.md` — Common build failures on JetPack 6.2 / TRT 10.3.x with fixes
