# Generative Video Frame Interpolation using Modified RIFE  
**Optimizing RIFE for Low-Resource Environments via Architecture-Aware Pruning**

## Overview
Video Frame Interpolation (VFI) synthesizes intermediate frames between consecutive video frames to improve frame rate, enable slow-motion playback, and enhance visual smoothness. While state-of-the-art deep learning models like **RIFE (Real-Time Intermediate Flow Estimation)** achieve excellent interpolation quality and real-time performance on desktop GPUs, their computational cost makes deployment on low-resource or edge devices challenging.

This project focuses on **systematically optimizing the RIFE model for low-resource environments** by applying **channel-wise pruning techniques**, while preserving interpolation quality.

The work includes:
- Reproducing and validating the baseline RIFE model
- Diagnosing failures in naive pruning approaches
- Designing an **architecture-aware pruning strategy**
- Quantifying efficiency–quality trade-offs using standard metrics

---

## Key Contributions
- Baseline validation of the pre-trained RIFE model on the UCF-101 benchmark, matching reported PSNR, SSIM, and real-time inference results.
- Diagnosis of naive global pruning failure, identifying catastrophic pruning of critical early layers.
- Architecture-aware channel pruning using L1-norm importance with protected layers and minimum channel constraints.
- Achieved ~40% inference speedup with controlled degradation in interpolation quality.
- Built a reproducible evaluation pipeline with automated benchmarking and result export (CSV/JSON).

---

## Methodology

### 1. Baseline Validation
- Loaded the official pre-trained RIFE model.
- Evaluated on the UCF-101 interpolation benchmark.
- Measured:
  - PSNR
  - SSIM
  - Inference latency (ms)
  - FPS
  - FLOPs and parameter count

Baseline performance closely matched values reported in the original RIFE paper, validating the experimental setup.

---

### 2. Channel Importance Estimation
- Computed **channel-wise L1-norm importance** for all convolutional layers.
- Importance calculated per output channel to guide pruning decisions.

---

### 3. Pruning Strategy

#### Naive Global Pruning (Diagnostic)
- Applied global thresholding across all layers.
- Resulted in severe quality degradation due to:
  - Over-pruning of early and critical layers
  - Architectural imbalance

#### Architecture-Aware Pruning (Proposed)
- Introduced protected layers to preserve critical network components.
- Applied layer-wise pruning thresholds instead of a global threshold.
- Enforced minimum channel constraints to prevent structural collapse.
- Performed soft pruning by zeroing out weights (mask-based pruning).

---

### 4. Evaluation Protocol
Each pruned model was evaluated using:
- **Interpolation Quality:** PSNR, SSIM
- **Efficiency:** Inference time (ms), FPS
- **Trade-offs:** Quality drop vs. sparsity level

Experiments were run across multiple sparsity levels to identify the optimal balance between efficiency and visual fidelity.

---

## Results Summary
- Baseline RIFE achieved high PSNR/SSIM with real-time performance.
- Architecture-aware pruning delivered:
  - ~40% increase in inference speed
  - Only modest PSNR degradation at higher sparsity levels
- Demonstrated that structure-preserving pruning enables effective compression of VFI models for efficient inference in low-resource environments.
