# Main Repository Structure Example

This repository illustrates a recommended repository organization to avoid code duplication, version divergence, and maintenance issues when working with multiple devices (e.g., DeepLink, PiLink, dev PC).
The key idea is functionality-based, not device-based repositories.

---

## Current structure

A device-based structure such as:

```
DeepLink-01/
 └── SendData (v0.1.0)

PiLink-01/
 ├── SendData (v0.3.0)
 └── SendMails
```

leads to:

* Code duplication (e.g., `SendData` exists in multiple devices)
* Version drift across devices
* Bugs fixed in one device but not propagated to others
* Increasing maintenance cost over time

If a bug is fixed in Deeplink there is no guarantee that Pilink will be updated, which can break compatibility or behavior.

---

## Proposed suggestion: Functionality-Based

Instead of grouping code by device, group it by **functionality**.

The main repository acts as an organizer and high-level view, while sub-repositories or sub-folders, as does not matter (sub-repositories can be pulled independently) can be used independently when needed.

---

## Recommended Structure

```
Main/
├── Scripts/
│   ├── src/
│   │   ├── send_data/
│   │   └── send_emails/
│   ├── pyproject.toml   # Dependencies ONLY for scripts
│   ├── VERSION          # Scripts versioning
│   └── README.md
│
├── TrainModels/
│   ├── src/
│   │   ├── train/
│   │   └── validation/
│   ├── pyproject.toml   # Heavy ML dependencies
│   ├── VERSION          # Training versioning
│   └── README.md
│
└── README.md            # This file
```

---
 
## Repository Roles and example as  RUNBOOK

### 1. Main Repository

* Organizes all components
* Provides a clear overview of the system

Clone everything for deveveloping:

```
git clone https://github.com/emoralesv-Deepbro/main_example
```

---

### 2. Scripts Repository

Contains **runtime scripts** used on devices such as:

* DeepLink
* PiLink
* Jetson

Examples:

* Send data through TCP or UDP
* Send automatic emails

Characteristics:

* Lightweight
* No ML frameworks
* Minimal resource usage

Dependencies include:

* fastapi
* uvicorn
* pydantic
* networking utilities

No installation of:

* PyTorch
* TensorFlow
* ONNX Runtime
* CUDA

Clone only functionalities:

```
git clone https://github.com/emoralesv-Deepbro/scripts_repo_example2
```

Install in editable mode:

```
pip install -e scripts_repo_example2/
```

Available commands:

* Execute `send_data` → works
* Execute `send_emails` → works
* Execute `train` → fails (expected)

This failure is intentional, since training dependencies are not installed.

---

### 3. Training Models Repository

Contains **training, validation, and deployment code** for ML models.

Characteristics:

* Can be run **independently**
* Heavy dependencies

Includes:

* PyTorch
* TensorFlow
* ONNX Runtime
* CUDA-related libraries

Clone training repo:

```
git clone https://github.com/emoralesv-Deepbro/dev_repo_training_models_example1
```

Install:

```
pip install -e dev_repo_training_models_example1/
```

This enables:

* Model training
* Validation
* Export and deployment workflows

---

## Alternative: Wheel-Based Installation

Instead of cloning the entire training repository, you can:

* Build a `.whl` package
* Distribute the installer

Example:

```
pip install training_models_example.whl
```

This avoids downloading the full repository while still enabling functionalities functionality.

---

## Updating Code

To update only what you need:

```
cd Scripts/
git pull
```

or

```
cd TrainModels/
git pull
```

No impact on other components.

---

