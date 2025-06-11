# Qcraft: Quantum Circuit Design, Optimization, and Surface Code Mapping Platform

## What is Qcraft?
Qcraft is an advanced, research-grade platform for quantum circuit design, optimization, and surface code mapping. It leverages reinforcement learning (RL), curriculum learning, and hardware-aware optimization to enable scalable, high-fidelity quantum circuit compilation and error correction. Qcraft is modular, extensible, and production-ready, supporting both classical and quantum error-corrected circuit workflows.

---

## Key Features
- **Reinforcement Learning for Quantum Circuit Optimization**: Device-aware, reward-driven optimization of quantum circuits, supporting gate fusion, commutation, SWAP insertion, and more.
- **Surface Code Multi-Patch Mapping**: RL-based mapping of multiple logical surface code patches to hardware, with advanced reward shaping and curriculum learning.
- **Curriculum Learning**: Progressive training with increasing difficulty, dynamic reward weighting, and robust convergence.
- **Hardware Awareness**: Supports IBM devices (IonQ in progress), native gate sets, and device-specific constraints.
- **Modular and Configurable**: YAML/JSON-driven configuration for all workflows, environments, and training parameters.
- **Logging and Artifact Management**: Automated tracking of training runs, metrics, and model artifacts for reproducibility.

---

## Installation

### Requirements
- **Python:** 3.9–3.11 (3.11 recommended)
- **CUDA:** 12.4 (required for RL training with surface code agents)
- **Tested on:** Linux, NVIDIA RTX 3070, CUDA 12.4, IBM Q devices

### Install from PyPI
```bash
pip install qcraft
```

### Installation

#### Option 1: Install from PyPI (Recommended)
```bash
pip install qcraft
```

#### Option 2: Install from GitHub Release Tarball
Download the latest `qcraft-<version>.tar.gz` from [https://github.com/deba10106/Qcraft.git](https://github.com/deba10106/Qcraft.git) (see Releases tab), then install with:

```bash
pip install /path/to/qcraft-<version>.tar.gz
```

**Note:**
- Python 3.9–3.11 supported (3.11 recommended)
- CUDA 12.4 required for RL training

---

## Usage

### Main GUI
```bash
qcraft
```

### RL Training (Examples)
- **Circuit Optimization RL Training:**
  ```bash
  python -m circuit_designer.workflow_bridge --config configs/optimizer_config.yaml
  ```
- **Surface Code Multi-Patch RL Training:**
  ```bash
  python -m scode.rl_agent.train_multi_patch --config configs/multi_patch_rl_agent.yaml
  ```

### Evaluation and Simulation
- **Evaluation:**
  ```bash
  python -m evaluation.evaluation_framework --config configs/your_eval_config.yaml
  ```
- **Execution Simulation:**
  ```bash
  python -m execution_simulation.execution_simulator --config configs/your_exec_config.yaml
  ```

---

## Reward Functions: Overview

### Surface Code Multi-Patch Agent
- **Highly configurable reward function**: Encourages valid mappings, hardware connectivity, adjacency, resource utilization, error minimization, and logical correctness.
- **Curriculum learning**: Dynamic reward weights and phase multipliers across training stages.
- **See** `configs/multi_patch_rl_agent.yaml` for all tunable parameters.

### Circuit Optimization Module
- **Reward engine**: Penalizes gate count, depth, and SWAPs; rewards native gate usage and penalizes invalid gates.
- **Curriculum learning**: Difficulty and reward weights progress as training advances.
- **See** `configs/optimizer_config.yaml` for all tunable parameters.

---

## Configuration and Customization

- All major workflows and RL environments are configured via YAML files in the `configs/` directory.
- **Surface Code Agent:** `configs/multi_patch_rl_agent.yaml`
- **Circuit Optimization Agent:** `configs/optimizer_config.yaml`
- **Device/Hardware:** `configs/ibm_devices.yaml`, `configs/ionq_devices.yaml`
- **Other:** Logging, visualization, and more via their respective YAML files.

---

## Packaging and PyPI Publishing

To build and publish your own version:
```bash
# Clean previous builds
rm -rf dist/*

# Build the package
python3 setup.py sdist bdist_wheel

# Check the package
pip install twine

# Upload to PyPI
twine upload dist/*
```

---

## Support and Extensibility
- Qcraft is modular and extensible for new devices, reward functions, and optimization passes.
- Contributions and feedback are welcome for further research and development.

---

## Citation
If you use Qcraft in academic work, please cite the corresponding paper or this repository.

---

For detailed technical documentation, architecture, and workflow explanations, please refer to the full README in the source repository.
