This repository implements a fully data-driven control framework where:

an LSTM digital twin identifies the motor–generator dynamics directly from I/O data, and

an LSTM controller is trained end-to-end through a differentiable closed-loop simulation to track references under constraints and disturbances.

Highlights

✅ LSTM plant model trained from PRBS excitation (Ts ≈ 0.03 s)

✅ Multi-step rollout stability validated

✅ Differentiable closed-loop training (controller learns through the plant model)

✅ Disturbance-aware plant model using dist as an exogenous input

✅ Fine-tuned disturbance-aware LSTM controller

✅ Final robust tracking under real disturbance patterns (from dataset)

Dataset

Input: u_volts (actuation voltage)

Output: y (measured terminal voltage)

Disturbance flag: dist (0/1)

Segments: PRBS_LOW, PRBS_HIGH, STEPS_HIGH, PRBS_HIGH_DIST

Method Overview

Plant identification (Digital Twin):

𝑦
^
𝑘
+
1
=
𝑓
𝜃
(
[
𝑢
,
𝑦
]
𝑘
−
𝑤
+
1
:
𝑘
)
y
^
	​

k+1
	​

=f
θ
	​

([u,y]
k−w+1:k
	​

)

and disturbance-aware version:

𝑦
^
𝑘
+
1
=
𝑓
𝜃
(
[
𝑢
,
𝑦
,
𝑑
]
𝑘
−
𝑤
+
1
:
𝑘
)
y
^
	​

k+1
	​

=f
θ
	​

([u,y,d]
k−w+1:k
	​

)

Differentiable Controller Training:
The controller produces 
𝑢
𝑘
u
k
	​

 to minimize:

𝐽
=
∑
𝑘
=
1
𝑇
𝑄
𝑒
𝑘
2
+
𝑅
𝑢
𝑘
2
+
𝑆
(
𝑢
𝑘
−
𝑢
𝑘
−
1
)
2
+
𝑄
𝑇
𝑒
𝑇
2
J=
k=1
∑
T
	​

Qe
k
2
	​

+Ru
k
2
	​

+S(u
k
	​

−u
k−1
	​

)
2
+Q
T
	​

e
T
2
	​


with 
𝑒
𝑘
=
𝑟
𝑘
−
𝑦
^
𝑘
e
k
	​

=r
k
	​

−
y
^
	​

k
	​

, subject to:

0
≤
𝑢
𝑘
≤
15
 V
0≤u
k
	​

≤15 V
Key Results (example)

Plant model (with disturbance): 1-step RMSE ≈ 0.025 V on PRBS_HIGH_DIST

Robust closed-loop tracking with real disturbance patterns:

RMSE ≈ 0.143 V

IAE ≈ 31.65

Control limits respected (u in [6.95, 13.42] V)
