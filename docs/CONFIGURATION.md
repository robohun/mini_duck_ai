<!-- generated-by: gsd-doc-writer -->
# Configuration Guide

This guide details all environment variables, configuration parameters, default settings, and overrides used across the Duck Agent Simulation MVP system.

---

## 1. Environment Variables

The application is configured primarily through environment variables loaded from a `.env` file at runtime. 

| Variable Name | Required / Optional | Default Value | Description |
|---------------|---------------------|---------------|-------------|
| `DUCK_SIM_MODE` | Optional | `mock` | Active simulation mode. Must be one of `mock`, `real`, or `webcam`. |
| `DUCK_DYNAMICS_MODE` | Optional | `legacy` | Active robotics dynamics mode. Must be one of `legacy`, `hybrid`, `dynamic`, `pure_physics`. |
| `DUCK_HYBRID_QVEL_XY_SCALE` | Optional | `1.0` | Scaling coefficient for linear velocity inputs under hybrid dynamics. Must be one of `0.0`, `0.5`, `1.0`. |
| `DUCK_HYBRID_Z_FORCE_SCALE` | Optional | `1.0` | Scaling coefficient for vertical force constraints under hybrid dynamics. Must be one of `0.0`, `0.5`, `1.0`. |
| `DUCK_HYBRID_RP_QVEL_ZERO_SCALE` | Optional | `1.0` | Scaling coefficient for roll/pitch velocity dampening under hybrid dynamics. Must be one of `0.0`, `0.5`, `1.0`. |
| `DUCK_HYBRID_TORSO_ORIENTATION_SCALE` | Optional | `1.0` | Scaling coefficient for torso orientation tracking under hybrid dynamics. Must be one of `0.0`, `0.5`, `1.0`. |
| `DUCK_POLICY_CONTRACT_WARNINGS` | Optional | `false` | Enable verbose warnings when ONNX locomotion policy boundaries are breached. |
| `DUCK_ONNX_MODEL_PATH` | Optional | `duck_agent_sim/models/BEST_WALK_ONNX_2.onnx` | Path to the ONNX locomotion policy model. |
| `BRIDGE_HOST` | Optional | `127.0.0.1` | Binding host address for the FastAPI server. |
| `BRIDGE_PORT` | Optional | `8765` | Listening port for the FastAPI server. |
| `SIM_DT` | Optional | `0.05` | Simulation timestep (dt) in seconds. |
| `GEMINI_API_KEY` | Optional | None | API key for the Google Gemini Multimodal Live API client. <!-- VERIFY: Live Gemini API credentials in cloud environments --> |
| `GOOGLE_API_KEY` | Optional | None | Alternative/fallback key name for Google Gemini APIs. |
| `HERMES_API_URL` | Optional | `http://127.0.0.1:8000` | Backend API URL mapping for the Hermes Developer Agent. <!-- VERIFY: Deployed API base URLs and staging domain configurations --> |
| `DUCK_MULTIPROCESS` | Optional | `false` | Run the simulator in a separate process communicating via Shared Memory IPC. |

---

## 2. Configuration Files

No dedicated configuration file types (such as JSON, TOML, or YAML) are consumed directly by the Python application code beyond standard `.env` environment loading and the package manifest `pyproject.toml`. 

For local development convenience, you can copy the `.env.example` file to `.env` in the repository root and modify the values as needed.

---

## 3. Required vs. Optional Settings

- **Required Settings**: No environment variables are strictly required to launch the mock simulator, as all options have fallback default values. However, if `DUCK_SIM_MODE=real` is specified, `duck_agent_sim/models/BEST_WALK_ONNX_2.onnx` must exist on the disk, or a custom model path must be supplied using `DUCK_ONNX_MODEL_PATH`. If the ONNX model is missing or unreadable, the real simulator initialization will fail during startup.
- **Optional Settings**: All scales and network parameters (such as host, port, and step time) default to the safe values documented above.

---

## 4. Per-Environment Overrides

To manage overrides across development, testing, and production:
- **Local Development**: Populate the root-level `.env` file.
- **Unit Tests**: The `pytest` test suite overrides settings inside specific test cases or through fixture setups. It does not overwrite the developer's local `.env` file.
- **Deployment Environments**: Cloud or container configurations should inject the corresponding variables directly into the system environment to override local file settings.
