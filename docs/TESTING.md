<!-- generated-by: gsd-doc-writer -->
# Testing Guide

This guide details the testing framework, test suite organization, command invocations, and helper patterns for running and writing tests in the Duck Agent Simulation MVP.

---

## 1. Test Framework and Setup

The project uses **pytest** as its primary test runner and test assertion framework.

### Setup
Before running tests, ensure you have:
1. Activated your local virtual environment:
   ```bash
   source .venv/bin/activate
   ```
2. Installed the development dependencies:
   ```bash
   pip install -e ".[dev]"
   ```
   This installs `pytest` and `httpx` (used for integration testing of the FastAPI endpoints).

---

## 2. Running Tests

You can execute different parts of the test suite depending on your needs.

### Run all tests
To run the full test suite (excluding experimental tests as configured in `pyproject.toml`):
```bash
pytest
```

### Run in verbose mode
To print more details about each executed test case:
```bash
pytest -v
```

### Run a specific test file
To isolate testing to a single file:
```bash
pytest tests/test_api.py
```

### Run a specific test case
To execute a single test case within a file:
```bash
pytest tests/test_api.py -k "test_api_health"
```

---

## 3. Writing New Tests

All test files must be placed in the `tests/` directory and follow standard naming conventions:
- **Test files**: Prefix with `test_` (e.g. `tests/test_<name>.py`).
- **Test functions**: Prefix with `test_` (e.g. `def test_feature_behavior():`).

### Test client pattern (FastAPI)
For endpoint and API routing tests, inject the standard `client` fixture using FastAPI's `TestClient` (as shown in `tests/test_api.py`):

```python
from fastapi.testclient import TestClient
from duck_agent_sim.main import app
import pytest

@pytest.fixture
def client():
    with TestClient(app) as c:
        yield c

def test_my_endpoint(client):
    response = client.get("/health")
    assert response.status_code == 200
```

### Mocking and patching
Use `monkeypatch` (built into pytest) or `unittest.mock` to mock hardware interfaces, camera streams, and web services:

```python
def test_sim_mode_override(client, monkeypatch):
    from duck_agent_sim.bridge import api
    monkeypatch.setattr(api, "DUCK_SIM_MODE", "webcam")
    
    response = client.get("/camera/info")
    assert response.json()["mode"] == "webcam"
```

---

## 4. Coverage Requirements

No strict test coverage thresholds are configured in the `pyproject.toml` or the python env setup. However, developers are highly encouraged to maintain coverage above 80% on all new modules and helper functions.

---

## 5. CI Integration

Currently, there are no continuous integration (CI) workflows configured for this repository. All formatting and test validation must be run locally prior to submitting changes to the main integration branch.
