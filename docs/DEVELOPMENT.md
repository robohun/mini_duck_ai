<!-- generated-by: gsd-doc-writer -->
# Development Guide

This guide outlines the local development setup, code formatting styles, linting configurations, branching rules, and PR flows for contributing to the Duck Agent Simulation MVP.

---

## 1. Local Setup

To configure your machine for active code contribution:

1. Follow the **[Getting Started Guide](GETTING-STARTED.md)** to clone the repository and run initial setup.
2. Install the package in editable development mode:
   ```bash
   pip install -e ".[dev]"
   ```
   This installs the core package along with development requirements (`pytest`, `httpx`, `ruff`, `mypy`).
3. Set your local environment mode. During local development, exporting `DUCK_SIM_MODE=mock` is recommended to avoid heavy physics simulation requirements.

---

## 2. Development & Formatting Commands

Since this is a Python project managed through `pyproject.toml`, tasks are executed using standard CLI tools rather than npm scripts.

| Task | Command | Description |
|------|---------|-------------|
| **Start server** | `bash scripts/run_bridge.sh` | Starts the FastAPI reload server at `127.0.0.1:8765`. |
| **Run tests** | `pytest` | Runs all unit and integration tests under the `tests/` directory. |
| **Lint code** | `ruff check .` | Audits the codebase for syntax issues, import styling, and standard errors. |
| **Format code** | `ruff format .` | Automatically formats all python files according to PEP 8 standards. |
| **Type Check** | `mypy .` | Runs static type assertions using mypy. |

---

## 3. Code Style & Quality

Modifications to the python codebase must adhere to the configured rules:

- **Linter & Formatter (Ruff)**: Configured in `pyproject.toml`. The line length is restricted to **88 characters** (matching standard Black/Ruff specifications). The linter enforces:
  - `E` / `W` (pycodestyle errors and warnings)
  - `F` (Pyflakes checks)
  - `I` (isort import ordering rules)
- **Type Safety (Mypy)**: All new functions and services should include explicit type annotations.

---

## 4. Branch Conventions

Contributions should be organized into branches using the following prefixes:

- **Features**: `feat/` prefix (e.g., `feat/add-camera-distortion`)
- **Bug Fixes**: `fix/` prefix (e.g., `fix/telemetry-latency`)
- **Documentation**: `docs/` prefix (e.g., `docs/update-installation`)
- **Chores & CI**: `chore/` or `ci/` prefix (e.g., `chore/bump-dependencies`)

The default integration branch is **`main`**. Direct commits to `main` are restricted.

---

## 5. Pull Request (PR) Process

Before submitting a Pull Request, verify your work:

1. **Verify Formatting**: Run formatting and lint audits:
   ```bash
   ruff format .
   ruff check .
   ```
2. **Type Checking**: Verify type annotations compile without errors:
   ```bash
   mypy .
   ```
3. **Execute Test Suite**: Run all tests and verify they pass:
   ```bash
   pytest
   ```
4. **Submit PR**: Open a PR targeting the `main` branch. Provide a clear description of the modifications, references to any issues resolved, and verification results. A peer review is required before merging.
