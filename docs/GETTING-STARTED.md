<!-- generated-by: gsd-doc-writer -->
# Getting Started

This guide walks you through setting up your environment, installing dependencies, and running your first simulation with the Duck Agent Simulation MVP.

---

## 1. Prerequisites

Before installing the project, make sure you have the following installed on your system:
- **Python**: Version `3.10` or higher is required.
- **Git**: Required for cloning the repository and managing submodules.
- **Virtualenv**: Support for Python virtual environments (`python3 -m venv` or `uv`).
- **MuJoCo** (Optional): If you plan to run the simulator in `real` mode with full physical dynamics, you must have MuJoCo physics support configured. If you do not have MuJoCo, you can run in `mock` mode.

---

## 2. Installation Steps

Follow these steps to clone the repository and configure your development environment:

1. **Clone the repository**:
   ```bash
   git clone https://github.com/varga0725/duck_sim.git
   cd duck_sim
   ```

2. **Initialize submodules and download dependencies**:
   The project requires the `Open_Duck_Playground` repository. Run the provided helper script to clone the submodule and setup the local environment:
   ```bash
   bash scripts/setup_open_duck.sh
   ```

3. **Configure Environment Variables**:
   Copy the template configuration file and populate your API credentials:
   ```bash
   cp .env.example .env
   ```
   *Note: If you plan to connect to the Google Gemini Multimodal Live API, open the `.env` file and insert your API key under `GEMINI_API_KEY`.*

4. **Verify package installation**:
   Ensure you are in the active virtual environment:
   ```bash
   source .venv/bin/activate
   pip install -e ".[dev]"
   ```

---

## 3. First Run

To start the FastAPI Bridge API with the default mock kinematics simulator:

1. Start the API bridge:
   ```bash
   bash scripts/run_bridge.sh
   ```

2. Open another terminal and verify the API is responsive:
   ```bash
   curl http://127.0.0.1:8765/health
   ```
   You should receive a JSON response confirming status `ok` and simulator mode `mock`.

---

## 4. Common Setup Issues

Here are the most common issues encountered during setup and how to resolve them:

### Issue 1: Failed to run real simulator due to missing ONNX model
- **Symptom**: The server crashes on startup in `real` mode with an error indicating that `BEST_WALK_ONNX_2.onnx` could not be found.
- **Resolution**: Ensure you have successfully run the setup script: `bash scripts/setup_open_duck.sh`. This script clones the required models into the package's model directory. If the issue persists, verify that the file exists at `duck_agent_sim/models/BEST_WALK_ONNX_2.onnx`.

### Issue 2: MuJoCo physics engine loading error
- **Symptom**: Importing `mujoco` fails, or MuJoCo crashes due to missing dynamic link libraries or layout files.
- **Resolution**: Verify that the MuJoCo submodule has been checked out correctly. If your environment does not support MuJoCo, switch the simulator to mock mode by exporting `DUCK_SIM_MODE=mock` in your environment or editing `.env`.

### Issue 3: Missing Gemini Live API key
- **Symptom**: Running voice control or live streaming scenarios logs authentication failures.
- **Resolution**: Open `.env` and set `GEMINI_API_KEY` to a valid API key obtained from [Google AI Studio](https://aistudio.google.com/).

---

## 5. Next Steps

Now that you have configured and run the simulation bridge, refer to the following guides to proceed:
- **[DEVELOPMENT.md](DEVELOPMENT.md)**: Guidelines on code styling, build tools, branches, and PR submissions.
- **[TESTING.md](TESTING.md)**: Instructions on running unit and integration tests.
- **[ARCHITECTURE.md](ARCHITECTURE.md)**: Deep dive into the component structures and request data flows.
- **[CONFIGURATION.md](CONFIGURATION.md)**: Detailed description of all available environment variables and dynamics settings.
