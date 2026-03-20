# LLM-OWA: Multitask Fine-tuning Agentic AI for Collaborative Scheduling in FMS

## 📖 Overview

This repository outlines the technical implementation of the **Orchestrator-Workers Agents (LLM-OWA)** framework, an agentic AI solution for collaborative scheduling in flexible manufacturing systems (CSFMS). Designed for the Industry 5.0 paradigm, this framework utilizes a hierarchical multi-agent system powered by an Integrated Scheduling Large Language Model (ISLLM) to perform multi-objective scheduling, heterogeneous resource management, and dynamic anomaly response.

## ✨ Key Features

* **Hierarchical Agent Architecture**: Employs a `LeaderAgent` for global orchestration and edge-layer agents (`ProdAgent`, `TransAgent`, `AuxAgent`) for localized execution on machines, automated guided vehicles (AGVs), and workers equipped with augmented reality (AR).


* **Multitask Fine-Tuning (MFT)**: Utilizes a novel fine-tuning strategy combining task-specific core parameter identification/freezing with Drop and Rescale (DARE) fusion to prevent cross-task interference in the ISLLM.


* **Multi-Objective Optimization**: Optimizes schedules for minimal makespan, reduced energy consumption, and balanced worker fatigue.


* **Resilient Rescheduling**: Features autonomous perception and semantic-driven reasoning to dynamically respond to state failures (e.g., machine breakdowns), task delays, and emergency order insertions.


* **Agent Skills & Tool Invocation**: Integrates external algorithms (e.g., NSGA-II, KMMA) and hyperparameter design tools (e.g., Optuna) via model context protocols (MCP).


---

## 🏗️ Architecture Design

The framework consists of three primary layers:

1. **Data and Model Management Layer**: Organizes task-oriented knowledge and maintains the ISLLM artifacts using a meta-framework called Manufacturing Knowledge Fabric (MKF) to generate high-quality datasets.


2. **Orchestrator-Workers Agents (OWA) Layer**: The cognitive core of the system.
* The orchestrator layer houses the `LeaderAgent`.


* The edge layer houses resource-specific agents utilizing the ReAct paradigm (Observation, Reasoning, Action).



3. **Physical Workshop Layer**: The actual manufacturing environment comprising machines, AGVs, workers, sensors, and edge computing servers.


---

## 🧠 Core Modules

### 1. Agents Configuration

* **LeaderAgent**: Operates on a plan-and-execute paradigm to generate initial schedules and rescheduling directives. It oversees objectives management, comprehensive decision making, and task allocation.


* **ProdAgent**: Responsible for machine-level production execution and monitoring.


* **TransAgent**: Manages AGV dispatching, routing, and transportation coordination.


* **AuxAgent**: Oversees auxiliary operations and human-machine interactions via AR interfaces.



### 2. ISLLM Multitask Capabilities

The ISLLM is fine-tuned to handle three specific CSFMS workflows:

* **Task 1**: Mapping heterogeneous FMS domain knowledge into standardized, machine-readable semantic structures.


* **Task 2**: Utilizing agent skills to select appropriate scheduling algorithms and configure hyperparameters.


* **Task 3**: Semantic interpretation of abnormal events and execution of reasoning-driven rescheduling strategies.


### 3. MFT Strategy (DARE + Parameter Freezing)

To preserve specialized knowledge across tasks, the framework uses an advanced MFT method:

* Executes per-task independent short-cycle fine-tuning with QLoRA to identify core parameter regions based on update amplitudes.


* Freezes task-specific core parameters using a binary mask during calibration.


* Applies DARE (Drop and Rescale) to fuse non-core parameters, randomly dropping redundant weights and rescaling the remainder to achieve model sparsification without catastrophic forgetting.


---

## 💻 Environment & Setup

### Hardware Requirements

* **CPU**: Multi-core processor (8 cores recommended).


* **RAM**: 32 GB system memory.


* **GPU**: Single 24 GB VRAM GPU.



### Software & Deployment

* **Environment**: Python 3.8.


* **Orchestration**: LangChain (for agent management and communication).


* **Inference Engine**: vLLM with continuous batching, KV-cache reuse, PagedAttention, and FlashAttention enabled.


* **Model Format**: The merged fine-tuned checkpoint is deployed with 4-bit weight-only quantization (INT4/AWQ) while activations remain in FP16.


* **Recommended Base Models**: Gemma3-12B or Qwen3-8B (Gemma3-12B is highly recommended for lightweight deployment avoiding MoE routing overhead).


---

## 📊 Evaluation Benchmarks

The LLM-OWA framework was tested against 33 extended benchmark instances (CSFMS1-CSFMS33) and compared against state-of-the-art baselines including NSGA-II, NSGA-III, KMMA, HMPSAC, and EvoXOpt.

* **Solution Quality**: Achieved a 3.03% average value improvement over baselines, dominating on Hypervolume (HV), Inverted Generational Distance (IGD), and Spacing (SP) metrics.

* **Rescheduling Stability**: Demonstrated highly consistent dynamic responses under state failures, task-time delays, and emergency orders, achieving a 0.9146 Decision Consistency Index (DCI) and a 0.9496 Interpretability Score (InS).
