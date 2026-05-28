<!-- generated-by: gsd-doc-writer -->
# Duck Agent Simulation MVP

An elegant, high-level API Bridge that connects AI Agents (e.g., OpenClaw, Hermes, or custom LLM supervisors) to a simulated **Open Duck Mini v2** robot. 

Built using FastAPI, WebSockets, and Pydantic, the project coordinates high-level movement commands, monitors body safety constraints, and streams real-time physical telemetry. It supports both a deterministic kinematic **Mock Simulator** (requiring no physics dependencies) and a scaffolding for wrapping the **Real MuJoCo physics controller** from the `Open_Duck_Playground` library.

![First-Person View of the Duck Robot](fpv_test_frame.png)

---

## 1. Quick Start

The absolute fastest way to install dependencies and run the application:

1. Clone the repository:
   ```bash
   git clone https://github.com/varga0725/duck_sim.git
   cd duck_sim
   ```

2. Run the automated setup script to clone submodules and configure the environment:
   ```bash
   bash scripts/setup_open_duck.sh
   ```

3. Start the FastAPI Bridge server:
   ```bash
   bash scripts/run_bridge.sh
   ```

---

## 2. Key Features

- **AI Agent Interface**: Allows LLM agents to command the duck robot using high-level movement commands (e.g., `walk_forward`, `turn_left`, `stop`, `reset`) rather than micro-controlling joint-level servo angles.
- **FastAPI Bridge**: Exposes REST and WebSocket endpoints for low-latency command ingestion and telemetry broadcasting.
- **MuJoCo & Mock Simulator**: Embodies safety limits (tilt bounds, fall limits, height collapse) and automatically halts target velocities when stability thresholds are breached. Supports both a deterministic kinematic Mock Simulator and the Real MuJoCo physics controller using ONNX gait policies.
- **Real-Time Vision & Perception**: Streams FPV frames, extracts structured object detections (YOLO), and integrates a vision-guided target follower.
- **Spatial World Model**: Maps the surroundings to a 2D occupancy grid and records semantic landmarks.

---

## 3. Installation

### Prerequisites
- Python >= 3.10
- Virtualenv support (`python3 -m venv`)
- OpenCV dependencies (on Linux, `libgl1-mesa-glx` or similar might be required)

### Step-by-Step Installation

1. Clone the repository and submodules:
   ```bash
   git clone https://github.com/varga0725/duck_sim.git
   cd duck_sim
   ```

2. Initialize and configure the local environment (including `Open_Duck_Playground` dependency):
   ```bash
   bash scripts/setup_open_duck.sh
   ```

3. (Alternative Manual Setup) Copy environment variables and install package with dev dependencies:
   ```bash
   cp .env.example .env
   python3 -m venv .venv
   source .venv/bin/activate
   pip install -e ".[dev]"
   ```

---

## 4. Usage & Examples

Once the server is running on `http://127.0.0.1:8765`, you can interact with it using HTTP client tools:

### Health Check
Check the API bridge status and active simulator mode:
```bash
curl http://127.0.0.1:8765/health
```

Response:
```json
{
  "status": "ok",
  "sim_mode": "mock",
  "robot": "open_duck_mini_v2"
}
```

### Get Telemetry / State
Retrieve the current XYZ coordinates, roll/pitch/yaw angles, and foot contacts:
```bash
curl http://127.0.0.1:8765/state
```

### Send High-Level Command
Command the robot to walk forward with a target speed and duration:
```bash
curl -X POST http://127.0.0.1:8765/command \
  -H "Content-Type: application/json" \
  -d '{
    "command": "walk_forward",
    "speed": 0.25,
    "turn": 0.0,
    "duration_sec": 2.0,
    "safety": {
      "stop_on_fall": true,
      "max_pitch_deg": 35.0,
      "max_roll_deg": 35.0
    }
  }'
```

### Telemetry Streaming via WebSockets
Connect to the 10Hz continuous telemetry feed:
```javascript
const ws = new WebSocket("ws://127.0.0.1:8765/ws");

ws.onmessage = (event) => {
  const robotState = JSON.parse(event.data);
  console.log("Telemetry Update:", robotState.position, robotState.fallen);
};
```

---

## 5. License

Refer to the repository owner for licensing and distribution terms.
