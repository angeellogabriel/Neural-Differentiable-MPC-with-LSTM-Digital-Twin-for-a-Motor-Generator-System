Neural Differentiable MPC with LSTM Digital Twin
(Motor–Generator 15V Experimental Platform)

This repository presents a fully data-driven nonlinear control framework based on a differentiable architecture composed of:

An LSTM Digital Twin trained from experimental I/O data.

An end-to-end trainable neural controller optimized through a differentiable closed-loop simulation.

The controller is trained directly through the plant model using backpropagation, enabling gradient-based optimization of long-horizon tracking performance under constraints and disturbances.

⚠️ The dataset used in this work is private (own experimental acquisition) and is not included in this repository.

🚀 Main Contributions

Data-driven identification of nonlinear motor–generator dynamics using LSTM

Multi-step stable rollout validation

Fully differentiable closed-loop control training

Disturbance-aware digital twin using exogenous input dist ∈ {0,1}

Fine-tuned disturbance-aware LSTM controller

Robust tracking under real disturbance profiles

⚙️ Experimental Setup

Sampling time: Ts ≈ 0.03 s

Actuation range: 0–15 V

Output: generator terminal voltage

Excitation signals:

PRBS_LOW

PRBS_HIGH

STEPS_HIGH

PRBS_HIGH_DIST

📊 Plant Identification (Digital Twin)

One-step prediction model:
$$
\hat{y}_{k+1} = f_\theta\big([u,y]_{k-w+1:k}\big)
$$
Disturbance-aware model:
$$
\hat{y}_{k+1} = f_\theta\big([u,y,d]_{k-w+1:k}\big)
$$
Where $u_k$ is the input voltage.


Differentiable Controller Training

The neural controller outputs $u_k$​ and is optimized through the differentiable digital twin by minimizing:
$$
J =
\sum_{k=1}^{T}
\left(
Q e_k^2
+
R u_k^2
+
S (u_k - u_{k-1})^2
\right)
+
Q_T e_T^2
$$
with:
$$
e_k = r_k - \hat{y}_k
$$
Subject to:
$$
0 \le u_k \le 15 \text{ V}
$$

This enables gradient-based optimization of closed-loop performance.

Final Results (Disturbance-aware Controller)

Closed-loop test using real disturbance profile:

Metric	Value
RMSE (V)	0.1427
IAE	31.6495
u_min (V)	6.95
u_max (V)	13.41
Disturbance rate	16.5%
<img width="1728" height="1361" alt="closedloop_tracking_dist" src="https://github.com/user-attachments/assets/64fa7f04-b26d-4eda-95d2-7036573611f3" />
<img width="1688" height="1361" alt="control_input_u" src="https://github.com/user-attachments/assets/88df1a6d-b568-49c0-b415-4cc6fa37e3b4" />
<img width="1702" height="1361" alt="disturbance_profile" src="https://github.com/user-attachments/assets/6c037e30-cf8a-4eff-a661-071cfd494a37" />

