# AI-Driven Resource Provisioning System for On-Premises Virtualized Infrastructure

## Project Overview

This project proposes an **AI-driven infrastructure optimization system** designed for **on-premises virtualized environments**. The system predicts future workload demand using machine learning and recommends the **optimal number of virtual machines (VMs)** required while considering **multi-resource constraints, heterogeneous hardware, and infrastructure capacity limits**.

Unlike traditional reactive scaling approaches that respond only after overload occurs, this system performs **predictive scaling** by forecasting workload patterns and estimating future infrastructure resource requirements.

The system also incorporates **explainable AI techniques** so that infrastructure operators can understand why specific scaling decisions were made.

---

# System Objectives

The proposed system aims to:

- Predict future workload demand using AI models
- Estimate future infrastructure resource requirements
- Recommend the optimal number of virtual machines
- Ensure infrastructure scaling respects **on-prem capacity constraints**
- Support **heterogeneous hardware environments**
- Optimize scaling decisions using **multi-resource analysis**
- Provide **explainable infrastructure recommendations**

---

# System Architecture

High-level system pipeline:
Infrastructure Metrics
↓
Data Collection Layer
↓
AI Workload Prediction Model
↓
AI Resource Demand Model
↓
Capacity-Aware VM Recommendation Engine
↓
Heterogeneous VM Placement Optimizer
↓
Explainable Decision Engine
↓
Infrastructure Dashboard

---

# Core System Modules

## 1. AI Workload Prediction Model

### Purpose

Predict future workload demand based on historical infrastructure metrics.

### Input Data

Collected infrastructure metrics:

| Metric | Description |
|------|-------------|
| request_rate | requests per second |
| cpu_usage | CPU utilization (%) |
| memory_usage | RAM utilization (%) |
| network_io | network throughput |
| response_latency | application latency |

Example dataset:
timestamp req/sec cpu% memory% network
10:00 200 40 52 100MB
10:05 260 45 58 110MB
10:10 300 50 61 120MB


### Model

Recommended model:

**LSTM (Long Short-Term Memory)**

Why LSTM:
- Handles time-series data effectively
- Captures workload patterns over time
- Widely used in workload forecasting research

Architecture:

Input Sequence (past workload metrics)
↓
LSTM Layers
↓
Dense Layer
↓
Predicted Workload


Example output:


Predicted requests next 10 minutes = 550/sec


Alternative models:

- XGBoost Regression
- Prophet Time Series Model

---

## 2. AI Resource Demand Model

### Purpose

Convert predicted workload into **future resource demand estimates**.

Instead of using fixed rules, machine learning models predict resource usage.

### Input Features


predicted_request_rate
active_users
network_io
historical_cpu_usage
historical_memory_usage


### Model

Recommended model:

**Random Forest Regression**

Outputs:


predicted_cpu_usage
predicted_memory_usage
predicted_network_usage


Example prediction:


CPU forecast = 78%
Memory forecast = 83%
Network usage = 40%


Alternative models:

- Multi-output Neural Networks
- Gradient Boosted Trees

---

## 3. Capacity-Aware VM Recommendation

### Purpose

Ensure scaling recommendations respect **on-prem infrastructure limits**.

Unlike cloud providers, on-prem clusters have **finite capacity**.

### Step 1: Define VM Capacity

Example VM configuration:


1 VM
CPU = 2 cores
RAM = 8GB


### Step 2: Compute Required VMs

Example predicted demand:


CPU demand = 40 cores
RAM demand = 160GB


VM requirement:


CPU VMs = 40 / 2 = 20
RAM VMs = 160 / 8 = 20


Final VM requirement:


required_vms = max(cpu_vms, ram_vms)


### Step 3: Capacity Check

Example cluster capacity:


max_vm_capacity = 24


Decision logic:


if required_vms <= max_vm_capacity
scaling safe
else
capacity risk detected


Example dashboard warning:


Capacity limit reached. Required VMs exceed cluster limit.


---

## 4. Hardware Heterogeneity Handling

### Problem

On-prem data centers contain **servers with different capacities**.

Example cluster:

| Server | CPU | RAM |
|------|------|------|
| server1 | 16 cores | 64GB |
| server2 | 32 cores | 128GB |
| server3 | 8 cores | 32GB |

### Resource Representation

Server capacity vector:


server_capacity = [cpu, ram, network]


VM resource demand:


vm_requirement = [cpu, ram, network]


Placement rule:


vm_cpu ≤ server_cpu_available
vm_ram ≤ server_ram_available


### Placement Algorithm

Recommended algorithm:

**Best-Fit Decreasing**

Steps:


Sort VMs by resource demand

Place VM on server with least remaining capacity that still fits

Update server capacity


Example placement result:


Server2 → 6 VMs
Server1 → 3 VMs
Server3 → 1 VM


---

## 5. Multi-Resource Scaling

### Purpose

Prevent scaling decisions based solely on CPU usage.

The system considers multiple resource constraints.

### Resources considered


CPU
Memory
Network


### Resource Utilization Calculation


cpu_utilization = used_cpu / total_cpu
memory_utilization = used_memory / total_memory
network_utilization = used_network / total_network


### Example Prediction


CPU forecast = 60%
Memory forecast = 92%
Network forecast = 40%


Scaling decision:


Scale infrastructure due to memory bottleneck


### VM Requirement Calculation


cpu_vms = predicted_cpu / cpu_per_vm
mem_vms = predicted_memory / memory_per_vm
net_vms = predicted_network / network_per_vm


Final VM requirement:


required_vms = max(cpu_vms, mem_vms, net_vms)


---

## 6. Explainable Infrastructure Decisions

### Purpose

Provide transparency into AI scaling decisions.

### Method

Use **SHAP (SHapley Additive Explanations)** to explain model predictions.

SHAP identifies which input features influenced the decision.

Example SHAP output:


Prediction: CPU usage = 82%

Feature importance:
request_rate → 60%
memory_usage → 25%
network_io → 15%


Dashboard explanation:


Scaling triggered due to high request rate growth.


---

# Dashboard Features

### Infrastructure Status


Active VMs: 8
CPU usage: 65%
Memory usage: 72%
Network usage: 48%


### Workload Prediction


Predicted requests next hour: 620/sec


### Resource Forecast


CPU forecast: 78%
Memory forecast: 84%


### Scaling Recommendation


Current VMs: 8
Recommended VMs: 12
Scale up by: +4


### Decision Explanation


Scaling triggered due to predicted memory pressure and request growth.


---

# AI Models Used in the System

| Module | Model |
|------|------|
Workload Prediction | LSTM / XGBoost |
Resource Demand Prediction | Random Forest |
VM Placement | Best-Fit-Decreasing (or Reinforcement Learning optional) |
Explainability | SHAP |

---

# Differences from Existing Research

Most existing VM placement and scaling research focuses on:

- Energy optimization
- VM consolidation
- heuristic or metaheuristic placement algorithms
- reactive scaling strategies

Common limitations in current research:

- assumes **infinite cloud capacity**
- assumes **homogeneous servers**
- focuses only on **CPU usage**
- lacks **explainability**
- does not address **on-prem infrastructure constraints**

The proposed system differs in several ways:

1. **On-Prem Capacity Awareness**

Most research assumes cloud environments with unlimited scalability.  
This system explicitly models **finite infrastructure capacity**.

2. **Heterogeneous Infrastructure Support**

Existing models often assume identical servers.  
This system handles **different server configurations**.

3. **Multi-Resource Scaling**

Instead of CPU-only scaling, the system analyzes:

- CPU
- memory
- network bandwidth

4. **Explainable AI Decisions**

The system integrates **SHAP explainability** to justify scaling decisions.

5. **Predictive Infrastructure Optimization**

Unlike reactive autoscaling systems, this architecture performs **predictive scaling using machine learning models**.

---
