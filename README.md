# LIF Neuron Simulation with Input Noise Analysis

**Python implementation of a Leaky Integrate-and-Fire (LIF) neuron with systematic noise analysis**

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Overview

This repository contains a Python implementation of a **Leaky Integrate-and-Fire (LIF) neuron** model with Gaussian input current noise. The simulation systematically investigates how input noise intensity modulates:

- **Firing rate** (spikes per second)
- **Spike timing regularity** (Coefficient of Variation of Inter-Spike Intervals)

This work is inspired by the spiking neural network models used in computational neuroscience, including those in Tomé et al. (Nature Neuroscience, 2024). Understanding single-neuron dynamics under noisy input is a foundational step toward building larger networks with STDP-based plasticity.

## Model Description

### LIF Neuron Dynamics

The subthreshold membrane potential follows:
τ_m * dV/dt = (V_rest - V) + R_m * I(t)

text

where:
- `τ_m` = membrane time constant (20 ms)
- `V_rest` = resting potential (-65 mV)
- `V_th` = spike threshold (-50 mV)
- `V_reset` = reset potential (-65 mV)
- `R_m` = membrane resistance (10 MΩ)

### Input Noise Model

Total input current:
I(t) = I_base + σ * ξ(t)

text

where `ξ(t)` is Gaussian white noise (zero mean, unit variance), and `σ` is the noise intensity parameter. Noise is implemented using the Euler-Maruyama method:
ΔV_stochastic = (R_m * σ / τ_m) * √(dt) * 𝒩(0,1)

text

## 🔬 Experiments

### Experiment 1: Standard LIF vs Moderate-Noise LIF
- Compare membrane potential traces with `σ = 0.0` and `σ = 1.0`
- Visualize spike patterns via raster plots (15 trials per condition)

### Experiment 2: Firing Rate vs Noise Intensity
- Vary `σ` from 0.0 to 3.0 (step 0.3)
- 20 trials per σ, each lasting 3000 ms
- Compute mean firing rate ± standard deviation

### Experiment 3: Spike Timing Irregularity (ISI & CV)
- Same parameter sweep as Experiment 2
- Compute Inter-Spike Intervals (ISI) and Coefficient of Variation (CV)
- CV = std(ISI) / mean(ISI)
- CV increases with irregularity (CV = 0 for perfect periodicity, CV ≈ 1 for Poisson process)

## Key Results

| Noise Level | Firing Rate (Hz) | CV (Irregularity) | Interpretation |
|-------------|------------------|-------------------|----------------|
| σ = 0.0 | ~24.3 | ~0.48 | Regular, near-periodic firing |
| σ = 1.0 | ~24.0 | ~0.74 | Irregular, jittered spikes |
| σ = 3.0 | ~29.9 | ~1.02 | Highly irregular, near-Poisson |

**Observations:**
1. **Firing rate increases with noise** — moderate noise can paradoxically enhance firing (stochastic resonance phenomenon)
2. **CV increases monotonically with noise** — higher noise makes spike timing less predictable
3. **At very high noise (σ > 2.5)**, CV exceeds 1.0, indicating a Poisson-like irregular regime

##  Requirements

```bash
pip install numpy matplotlib
No additional deep learning frameworks required.

Usage
Run the complete simulation
bash
python lif_noise_simulation.ipynb
Or run the notebook cell by cell in Jupyter/Colab.

Key functions
python
# Simulate a single LIF neuron
time, V, spike_times, spike_train = simulate_lif(
    T=500.0,          # duration (ms)
    dt=0.1,           # time step (ms)
    sigma=1.0,        # noise intensity
    I_base=1.8        # baseline current (nA)
)

# Compute firing rate (Hz)
rate = compute_firing_rate(spike_times, T)

# Compute ISI and CV
isi = compute_isi(spike_times)
cv = compute_cv(isi)
