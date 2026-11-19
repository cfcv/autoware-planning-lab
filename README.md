# Autoware Planning Lab

**An open engineering lab for autonomous driving simulation, classical planning, and learning-based planning.**

This project is dedicated to understanding, implementing, benchmarking, and improving autonomous driving planning algorithms using Autoware, CARLA, and NVIDIA NuRec.

The goal is to go beyond running existing frameworks: explore their architectures, understand their algorithms, develop C++ implementations, introduce meaningful improvements, and evaluate those changes through reproducible closed-loop simulations.

The project is organized as a progressive engineering roadmap, from simulation setup to classical planning algorithms, embedded deployment, and AI-based planning.

> **Project status:** Under development. The repository is being built progressively through the milestones described below.

## 1. Project Objectives

The project has five main objectives:

1. **Build a reusable simulation environment** using CARLA and NVIDIA NuRec, including neural reconstruction of real-world driving scenes.
2. **Master Autoware's planning architecture**, from mission and behavior planning to motion planning and trajectory validation.
3. **Implement and improve planning algorithms** in C++, with attention to correctness, numerical optimization, robustness, and real-time performance.
4. **Establish reproducible benchmarks** to evaluate planning quality, safety-related metrics, computational performance, and closed-loop behavior.
5. **Explore learning-based and hybrid planning**, comparing neural trajectory generation with classical approaches and investigating their integration.

The long-term objective is a complete research and engineering environment where different planning algorithms can be developed and evaluated under comparable conditions.

## 2. Technology Stack

| Component | Technology |
|---|---|
| Autonomous driving framework | Autoware 2.1.0 |
| Simulation | CARLA 1.0 |
| Neural reconstruction and rendering | NVIDIA NuRec 26.04 |
| Robotics middleware | ROS 2 |
| Main programming languages | C++ and Python |
| Development and deployment | Docker, Linux |
| Mapping | Lanelet2, OpenDRIVE |
| Planning and optimization | Graph search, rule-based planning, trajectory optimization, MPC |
| Learning-based planning | Diffusion models, end-to-end driving, hybrid planning |
| Embedded deployment | NVIDIA Jetson Thor |
| Benchmarking | CARLA closed-loop scenarios, ROS 2 logs, structured metrics |

**Versioning strategy:** Autoware 2.1.0 is our initial pinned planning baseline. CARLA 1.0 and NuRec 26.04 are the target simulation stack. Their combined integration is an engineering task that must be validated, not an assumed working configuration.

CARLA 0.9.16 remains a fallback reference for the official Autoware 2.1.0 CARLA demonstration.

## 3. Simulation Strategy

The project begins by establishing a simulation environment that can also be used independently of Autoware.

### Phase A — Standalone NuRec + CARLA

The first objective is to integrate reconstructed real-world scenes into CARLA.

Our initial reconstruction pipeline is:

**Physical AI dataset → NCore → NVIDIA NuRec → USDZ scene**

We will then evaluate:

**NuRec USDZ → CARLA 1.0 → Neural sensors → Closed-loop ego control**

This phase focuses on:

- Loading reconstructed USDZ scenes.
- Validating coordinate frames, sensor calibration, and road-network information.
- Generating neural camera and LiDAR observations.
- Controlling the ego vehicle independently of the recorded trajectory.
- Establishing a reusable closed-loop simulator for external driving agents.

This simulation infrastructure is intended to support other autonomous driving research projects, not just Autoware.

### Phase B — Autoware Integration

Once the standalone simulator is working, we will integrate Autoware.

The target architecture is:

**CARLA + NuRec → ROS 2 sensors → Autoware → Planning and control → CARLA ego vehicle**

The initial focus will be Autoware's classical planning stack. Learning-based and hybrid planning will be introduced in later milestones.

The integration will require validation of the CARLA 1.0 native ROS 2 interface, map alignment, localization, sensor data transport, and control feedback.

## 4. Project Roadmap

Development is organized into nine milestones.

| Milestone | Focus | Main Outcome |
|---|---|---|
| **M0 — Project Setup** | Repository, tools, documentation, and development workflow | Reproducible project foundation |
| **M1 — NuRec, CARLA & Autoware** | Standalone neural simulation followed by Autoware integration | Working closed-loop simulation |
| **M2 — Architecture Exploration** | ROS 2 graph, perception, localization, and planning interfaces | Complete planning architecture understanding |
| **M3 — Benchmark Framework** | Scenarios, metrics, repeatability, and automated evaluation | Reproducible benchmark infrastructure |
| **M4 — Mission Planning** | Lanelet2 routing, graph search, and routing costs | C++ routing experiments and improvements |
| **M5 — Behavior Planning** | Scene modules, decision logic, traffic rules, and safety checks | Improved behavior decision-making |
| **M6 — Motion Planning** | Vehicle models, trajectory optimization, velocity planning, and numerical solvers | Motion planning implementations and benchmarks |
| **M7 — System Integration & Jetson Thor** | Full-stack integration, robustness, profiling, and embedded execution | Validated integrated system |
| **M8 — AI & Hybrid Planning** | Diffusion planning, learned trajectories, validation, and fallback | Classical vs. learned vs. hybrid comparison |

See the [GitHub Issues](https://github.com/cfcv/autoware-planning-lab/issues) and [Milestones](https://github.com/cfcv/autoware-planning-lab/milestones) for detailed tasks and progress.

## 5. Development Methodology

Each planning subsystem will follow the same engineering process:

**Understand → Reproduce → Benchmark → Implement → Compare → Integrate → Validate → Document**

### Understand
Study the architecture, algorithms, source code, mathematical formulations, and relevant interfaces.

### Reproduce
Run the original implementation, reproduce its behavior, and establish a known working configuration.

### Benchmark
Develop controlled experiments and measure performance before introducing changes.

### Implement
Develop reference algorithms and meaningful improvements, primarily in C++ for classical planning components.

### Compare
Evaluate the original and modified implementations under equivalent conditions.

### Integrate
Connect changes to the existing autonomy stack and verify compatibility with upstream and downstream components.

### Validate
Test nominal behavior, edge cases, robustness, failure handling, and real-time constraints.

### Document
Record design decisions, algorithms, experiments, results, limitations, and lessons learned.

The guiding principle is that engineering improvements should be supported by **reproducible evidence**, not only qualitative observations.

## 6. Repository Organization

The planned repository structure separates documentation, simulation infrastructure, independent algorithm experiments, and benchmarks.

- `docs/`
  - `setup/` — Development environment and configuration
  - `simulation/` — CARLA, NuRec, and ROS 2 integration
  - `architecture/` — Autoware architecture and interfaces
  - `benchmarks/` — Evaluation methodology and metrics
  - `mission-planning/` — Routing algorithms and experiments
  - `behavior-planning/` — Decision logic and scene modules
  - `motion-planning/` — Trajectory generation and optimization
  - `integration/` — System-level validation and profiling
  - `jetson-thor/` — Embedded deployment and performance
  - `ai-planning/` — Learning-based and hybrid planning
- `simulation/`
  - `carla-nurec/` — Reusable neural simulation configurations and tools
- `experiments/`
  - `mission-planning/`
  - `behavior-planning/`
  - `motion-planning/`
  - `ai-planning/`
- `benchmarks/`
  - `scenarios/` — Scenario definitions
  - `configs/` — Experiment configurations
  - `runner/` — Benchmark execution tools
  - `metrics/` — Evaluation metrics
  - `results/` — Benchmark results and reports

These directories will be added incrementally as the corresponding milestones are implemented.

Large datasets, model weights, neural scene assets, and generated artifacts will be stored outside Git, with their versions and checksums documented when required for reproducibility.

## 7. Benchmarking Principles

All planning evaluations should prioritize:

- Reproducible scenarios and deterministic configurations where possible.
- Clearly defined metrics and experimental conditions.
- Equivalent inputs and constraints when comparing algorithms.
- Multiple trials to account for variability.
- Explicit handling of failed and incomplete runs.
- Separation of planning errors from perception, localization, and control failures.
- Performance measurements including latency distributions and resource utilization.
- Transparent reporting of limitations and regressions.

The benchmark framework will be developed in M3 and progressively extended throughout the project.

## 8. Getting Started

The repository is currently in its setup phase.

Clone the project:

`git clone https://github.com/cfcv/autoware-planning-lab.git`

The initial development sequence is:

1. Complete the project setup and documentation tasks in M0.
2. Prepare a GPU-capable environment for CARLA 1.0 and NuRec 26.04.
3. Run the official NuRec–CARLA integration using a reference scene.
4. Import and validate our reconstructed PAI USDZ scene.
5. Demonstrate closed-loop vehicle control without Autoware.
6. Integrate Autoware 2.1.0 and validate the classical planning stack.

Installation commands, container configurations, and reproducible execution instructions will be added as each environment is validated.

## 9. References

### Autoware
- [Autoware GitHub](https://github.com/autowarefoundation/autoware)
- [Autoware 2.1.0 Release](https://github.com/autowarefoundation/autoware/releases/tag/2.1.0)
- [Autoware Documentation](https://autowarefoundation.github.io/autoware-documentation/)

### Simulation
- [CARLA Simulator](https://github.com/carla-simulator/carla)
- [CARLA 1.0 Documentation](https://carla-ue5.readthedocs.io/)
- [CARLA + NuRec Integration](https://carla-ue5.readthedocs.io/en/latest/nvidia_nurec/)
- [CARLA + Autoware Integration](https://carla-ue5.readthedocs.io/en/latest/autoware/)

### Neural Reconstruction
- [NVIDIA NuRec Documentation](https://docs.nvidia.com/nurec/)

### Robotics
- [ROS 2 Documentation](https://docs.ros.org/)
- [Lanelet2](https://github.com/fzi-forschungszentrum-informatik/Lanelet2)

## 10. Project Scope

This is an independent engineering and research project built using open-source autonomous driving tools and publicly available technical resources.

The project is not affiliated with or officially endorsed by the Autoware Foundation, CARLA, or NVIDIA.

The experiments are intended for research and controlled simulation environments. Simulation results do not establish suitability for deployment on public roads.
