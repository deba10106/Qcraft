# Qcraft: A Modular Platform for Quantum Circuit Optimization and Surface Code Mapping via Reinforcement Learning

## Abstract

Qcraft is a research-grade, modular desktop application for quantum circuit design, optimization, and surface code mapping. It leverages reinforcement learning (RL) to address the challenges of scalable, hardware-aware quantum compilation and error correction. This work presents the architecture, configurable workflows, and the novel RL-based surface code mapping module, highlighting the scientific motivation, reward function design, and future research directions.

---

Join Developer Community: https://docs.google.com/forms/d/e/1FAIpQLScyQAMenZaJy58T4qcdYRDXbcqNG5HzNxLQeukdkYvhx1iXWQ/viewform?usp=sharing&ouid=105826528724887272549

## 1. Introduction

Quantum computing promises exponential speedups for certain problems, but practical realization is hindered by noise, limited connectivity, and hardware constraints. Surface codes are a leading error correction technique, but mapping logical qubits to physical hardware remains a complex, high-dimensional optimization problem. Qcraft addresses this by providing a unified, extensible platform for:

- Quantum circuit design and editing

- Hardware-aware circuit optimization

- RL-driven surface code mapping

- Artifact management and reproducibility

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

### Usage

To launch the Qcraft desktop application, simply run:

```bash
qcraft
```

---


Join Developer Community: https://docs.google.com/forms/d/e/1FAIpQLScyQAMenZaJy58T4qcdYRDXbcqNG5HzNxLQeukdkYvhx1iXWQ/viewform?usp=sharing&ouid=105826528724887272549



# 🚀 Introducing Qcraft: A Reinforcement Learning-Powered Desktop App for Fault-Tolerant Quantum Circuit Design

**What if you could build fault-tolerant quantum circuits that adapt to the quirks of real hardware — right from your desktop?**

Meet **Qcraft** — a soon-to-be open-source quantum application that merges error correction, device-aware layout mapping, and circuit optimization into one modular, extensible system.

---

## ⚛️ Why Qcraft?

Today's quantum computers are noisy and limited by hardware constraints like qubit connectivity, gate fidelities, and decoherence times. If you're building fault-tolerant quantum applications, you need to solve several complex problems:

* Map logical qubits onto imperfect hardware grids

* Design surface code layouts that minimize logical error

* Optimize circuits under noise, depth, and gate constraints

* Adapt to different backends like **IBM**, **IonQ**, or **Rigetti**

**Qcraft does all this.**

It contains:

* A **Multi-Patch Surface Code Layout Mapper** powered by Reinforcement Learning (RL)

* An optional **Circuit Optimizer**, also enhanced by RL

* Full **YAML-based configurability**

* Support for **connectivity-aware layout optimization**

* Early support for **heavy-hex (IBM)** and **all-to-all (IonQ)** devices

---

## 🧠 Surface Code Layout Mapping: The Reward Model

The first core module of Qcraft takes a hardware graph and maps **multiple surface code patches** — one per logical qubit — such that:

* Each patch is placed respecting connectivity

* Patches don't overlap

* Logical qubits are fully and validly mapped

* The overall layout minimizes SWAPs and decoherence

### 🎯 RL Reward Function

Here's how we **teach** the agent what “good” looks like — with **reward weights**:

```yaml
reward_function:

valid_mapping: 10.0

invalid_mapping: -20.0

overlap_penalty: -5.0

connectivity_bonus: 2.0

adjacency_bonus: 1.0

inter_patch_distance_penalty: -1.0

resource_utilization_bonus: 0.5

error_rate_bonus: 1.0

logical_operator_bonus: 1.0

fully_mapped_bonus: 2.0

mapped_qubit_bonus: 0.1

unmapped_qubit_penalty: -0.05

connected_bonus: 1.0

disconnected_graph_penalty: -0.1

normalization: running_mean_std

normalization_constant: 10.0

dynamic_weights: true

phase_multipliers:

hardware_adaptation_gate_error: 2.0

hardware_adaptation_swap: 2.0

noise_aware_logical_error: 2.5

structure_mastery_stabilizer: 3.0
```

### 🧮 What These Weights Mean

| Term | Meaning |

| ----------------------------------------------- | -------------------------------------------------------------------- |

| `valid_mapping`, `invalid_mapping` | Encourage valid layout generation; heavily penalize illegal mappings |

| `overlap_penalty` | Prevent multiple patches from using the same physical qubit |

| `connectivity_bonus` | Reward layouts where stabilizers align with physical couplings |

| `adjacency_bonus` | Promote tightly packed patches to reduce communication costs |

| `inter_patch_distance_penalty` | Encourage proximity among interacting logical qubits |

| `resource_utilization_bonus` | Prefer layouts that efficiently use available qubits |

| `error_rate_bonus` | Use qubits with lower error rates, encouraging noise-aware mapping |

| `logical_operator_bonus` | Reward well-defined logical X/Z operators |

| `fully_mapped_bonus` | Additional bonus if all logical qubits are successfully placed |

| `connected_bonus`, `disconnected_graph_penalty` | Incentivize connected logical layouts |

| `normalization` | Normalize reward across episodes using running mean/std |

| `phase_multipliers` | Dynamically boost certain rewards during curriculum stages |

### 🧪 What More Can Be Added?

* `t1_weight`, `t2_weight`: Use coherence times in scoring

* `thermal_noise_penalty`: Penalize use of thermally unstable qubits

* `communication_delay_penalty`: Estimate and penalize between-patch signal delay

* `hardware_preference_bonus`: Prefer specific regions of the chip

---

## 🧮 Circuit Optimizer Module: Reward Breakdown

The second module (optional) optimizes logical circuits under hardware constraints. It can be disabled via config if you prefer rule-based scheduling.

### 🎯 RL Reward Function

```yaml
reward_weights:

gate_count: -1.0

depth: -0.5

swap_count: -2.0

native_gate_bonus: 1.0

invalid_gate_penalty: -5.0
```

### 🧠 What These Weights Do

| Term | Meaning |

| ---------------------- | ---------------------------------------------------- |

| `gate_count` | Penalize longer circuits |

| `depth` | Reduce total gate layers for lower latency and error |

| `swap_count` | Strongly penalize SWAPs due to connectivity limits |

| `native_gate_bonus` | Reward circuits that use device-native gates |

| `invalid_gate_penalty` | Penalize usage of gates unsupported by hardware/code |

### 🔍 What More Could Be Added?

* `parallelism_bonus`: Reward circuits that allow simultaneous operations

* `t_gate_penalty`: Penalize high-cost T gates if magic state distillation is expensive

* `idle_qubit_penalty`: Discourage leaving qubits idle for long periods

* `dynamic_depth_penalty`: Increase penalty with noise accumulation

---

## 🧩 Modular, Configurable, and Growing

Qcraft is designed to be modular, extensible, and fully configurable. Want to switch out the reward model? Just edit your YAML file. Want to plug in a different hardware backend? Update your graph file.

**All this works on your desktop — no cloud setup required.**

---

## 🧪 Try It, Break It, Improve It

I’m looking for **testers**, **contributors**, and **curious minds** who want to build the future of **hardware-aware, fault-tolerant quantum programming**.

🔗 [GitHub – Qcraft](https://github.com/deba10106/Qcraft)

Feel free to clone, test, raise issues, and reach out if you'd like to contribute!

Let’s build Qcraft together.

---


Join Developer Community: https://docs.google.com/forms/d/e/1FAIpQLScyQAMenZaJy58T4qcdYRDXbcqNG5HzNxLQeukdkYvhx1iXWQ/viewform?usp=sharing&ouid=105826528724887272549



**#QuantumComputing #ReinforcementLearning #SurfaceCode #OpenSource #QEC #QuantumErrorCorrection #CircuitOptimization #FaultTolerance #IBMQuantum #IonQ #Qcraft #ModularDesign**

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

---

## 🔭 Future Focus

Qcraft's roadmap includes:

1. **Hamiltonian-Driven Circuit Generation:**
- Adding more functionalities to generate circuits directly from user-defined or standard Hamiltonians, enabling quantum simulation and algorithm development workflows.
2. **Support for Advanced Code Patches:**
- Extending mapping and optimization to other error correction codes, including quantum LDPC (QLDPC) and other next-generation codes, beyond surface codes.
