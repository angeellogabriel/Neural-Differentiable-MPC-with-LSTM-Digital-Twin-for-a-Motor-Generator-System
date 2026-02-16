This repository implements a fully data-driven control framework where:

an LSTM digital twin identifies the motor–generator dynamics directly from I/O data, and

an LSTM controller is trained end-to-end through a differentiable closed-loop simulation to track references under constraints and disturbances.

Highlights

-LSTM plant model trained from PRBS excitation (Ts ≈ 0.03 s)

-Multi-step rollout stability validated

-Differentiable closed-loop training (controller learns through the plant model)

-Disturbance-aware plant model using dist as an exogenous input

-Fine-tuned disturbance-aware LSTM controller

-Final robust tracking under real disturbance patterns (from dataset)

Dataset

Input: u_volts (actuation voltage)

Output: y (measured terminal voltage)

Disturbance flag: dist (0/1)

Segments: PRBS_LOW, PRBS_HIGH, STEPS_HIGH, PRBS_HIGH_DIST

Method Overview

Plant identification (Digital Twin):

$𝑦^𝑘+1=𝑓𝜃([𝑢,𝑦]𝑘−𝑤+1:𝑘)$
and disturbance-aware version:

𝑦^𝑘+1=𝑓𝜃([𝑢,𝑦,𝑑]𝑘−𝑤+1:𝑘)

Differentiable Controller Training:
The controller produces 
𝑢𝑘 to minimize:

𝐽=∑𝑘=1𝑇𝑄𝑒𝑘²+𝑅𝑢𝑘²+𝑆(𝑢𝑘−𝑢𝑘−1)²+𝑄𝑇𝑒𝑇²

with 
𝑒𝑘=𝑟𝑘−𝑦^𝑘ek, subject to:

0≤𝑢𝑘≤15V
Key Results (example)

Plant model (with disturbance): 1-step RMSE ≈ 0.025 V on PRBS_HIGH_DIST

Robust closed-loop tracking with real disturbance patterns:

RMSE ≈ 0.143 V

IAE ≈ 31.65

Control limits respected (u in [6.95, 13.42] V)
