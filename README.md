# Neural Differentiable MPC with LSTM Digital Twin (Motor–Generator 15V)

This repository implements a fully data-driven control framework where:
1) an **LSTM digital twin** identifies the motor–generator dynamics directly from I/O data, and  
2) an **LSTM controller** is trained end-to-end through a **differentiable closed-loop simulation** to track references under constraints and disturbances.

> **Note:** The dataset used in this work is **private** (own experimental acquisition) and is not included in this repository.

---

## Highlights
- ✅ LSTM plant identification from PRBS excitation (Ts ≈ 0.03 s)  
- ✅ Multi-step rollout stability validated  
- ✅ Differentiable closed-loop training (controller learns through the plant model)  
- ✅ Disturbance-aware digital twin using `dist` (0/1) as exogenous input  
- ✅ Fine-tuned disturbance-aware LSTM controller  
- ✅ Robust tracking under real disturbance patterns

---

## Expected dataset format (private)
Place your dataset file at:


Required columns:
- `tempo` (float)
- `segmento` (string)
- `u_volts` (float)  ← actuation voltage
- `y` (float)        ← measured terminal voltage
- `dist` (int 0/1)   ← disturbance flag

Example segments used:
- `PRBS_LOW`, `PRBS_HIGH`, `STEPS_HIGH`, `PRBS_HIGH_DIST`

---

## Method Overview

### Plant identification (Digital Twin)
One-step prediction:
$\[
\hat{y}_{k+1} = f_\theta([u,y]_{k-w+1:k})
\]$
Disturbance-aware version:
$\[
\hat{y}_{k+1} = f_\theta([u,y,d]_{k-w+1:k})
\]$

### Differentiable Controller Training
The controller produces \(u_k\) to minimize:
$\[
J = \sum_{k=1}^{T} Q e_k^2 + R u_k^2 + S (u_k-u_{k-1})^2 + Q_T e_T^2
\]$
with $\(e_k = r_k - \hat{y}_k\)$, subject to:
$\[
0 \le u_k \le 15\text{ V}
\]$

---
Results:
<img width="1728" height="1361" alt="closedloop_tracking_dist" src="https://github.com/user-attachments/assets/d5c8f223-ef96-405c-b162-a73d591817b3" />
<img width="1688" height="1361" alt="control_input_u" src="https://github.com/user-attachments/assets/506253b8-2c06-43cc-a43e-801f1f3f212b" />
<img width="1702" height="1361" alt="disturbance_profile" src="https://github.com/user-attachments/assets/c4381eee-675a-4f81-8adb-e1d8ef8d3cbc" />
[summary_metrics.csv](https://github.com/user-attachments/files/25348198/summary_metrics.csv)
RMSE_V,IAE,u_min_V,u_max_V,dist_percent
0.14271368086338043,31.649505615234375,6.9515485763549805,13.415555000305176,16.500000655651093

