---
layout: page
title: service system design with congestion
description: Two conic formulations benchmarked against a cutting-plane algorithm across 355 instances — a 63× median speedup.
img: assets/img/projects/misoco.png
importance: 2
category: optimization
---

**Julia · JuMP · Gurobi · Mixed-Integer Second-Order Cone Programming**

A Julia/JuMP reproduction of Góez & Anjos (2019), benchmarked against
Elhedhli's (2006) exact outer-linear-approximation algorithm across **355
instances**.

### The problem

Design a network of service facilities under stochastic demand and congestion,
making three interleaved decisions: **which** facilities to open, **what
capacity** to install at each, and **which customers** to assign where —
minimising opening cost + delay cost + assignment cost.

Each open facility behaves as an M/M/1 queue, so the delay term is a ratio of
arrival rate to spare service rate: nonlinear, and undefined for a facility that
stays closed. That single quirk is what all three methods exist to handle.

### The three methods

| Method                           | Idea                                                                                                             |
| :------------------------------- | :--------------------------------------------------------------------------------------------------------------- |
| **Algorithm 1** (Elhedhli 2006)  | Relax the concave term with tangent cuts, re-solve, add cuts until the bound gap closes — one MILP per iteration |
| **MISOCO 6** (Góez & Anjos 2019) | Express congestion via traffic intensity and make the capacity constraint conic — one MISOCP, 2m rotated cones   |
| **MISOCO 7** (Góez & Anjos 2019) | Same, but linearise the bilinear term exactly — only m rotated cones, all of dimension 3                         |

### Results

355 instances = 71 Holmberg problems × 5 delay-cost levels. Gurobi, 600 s limit,
1% MIP gap, identical settings throughout.

|                      | Algorithm 1 | MISOCO 6 |    MISOCO 7 |
| :------------------- | ----------: | -------: | ----------: |
| Mean solve time      |    140.62 s |   3.99 s |  **1.79 s** |
| Median solve time    |     34.80 s |   1.30 s |  **0.60 s** |
| Max solve time       |   1175.90 s | 201.40 s | **35.70 s** |
| Fastest on           |        0.0% |    11.3% |   **88.7%** |
| Instances over 600 s |          28 |        0 |           0 |

Total solver time across the benchmark: **13.9 hours** for the cutting-plane
baseline versus **10.6 minutes** for Formulation 7 — a **63× median speedup**.
All three agree on the objective within the 1% MIP gap on every instance, so
this is a pure speed difference, not an accuracy trade-off.

[Code, formulations, and benchmark results on GitHub](https://github.com/Rajas124/misoco-service-system-design)
