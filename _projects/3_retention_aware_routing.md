---
layout: page
title: retention-aware delivery routing
description: Three approximations of the Bellman recursion for a stochastic routing problem where customers remember how well they were served.
img: assets/img/projects/retention_routing.png
importance: 3
category: stochastic
---

**Python · Stochastic Dynamic Programming · Monte Carlo Tree Search**

A single vehicle operates over a finite horizon and must repeatedly decide which
client to serve, how much to deliver, and when to return to a distant depot to
replenish. Demand is uncertain and revealed only on arrival. Service generates
revenue; travel costs money.

The twist is that customers remember. Each client carries a satisfaction level
that evolves with how well it has been served, and moves through **Active →
Suspended → Retry → Lost** as that satisfaction crosses a category-specific
threshold. Clients also carry visit deadlines: miss one and the customer is gone
permanently. Each decision therefore sets both immediate revenue _and_ the
future availability of demand, forcing a trade-off between short-term
exploitation and long-term retention.

The problem admits a Bellman-optimal stochastic DP formulation, but its
high-dimensional, partially continuous state space makes exact solution
infeasible.

### Three controllers

|       | Method        | Approach                                                                                                              | Verdict                                                                     |
| ----- | ------------- | --------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| **1** | Backward DP   | Approximate the global value function via state aggregation and Monte Carlo expectation, then act greedily against it | Principled benchmark; computationally infeasible past small horizons        |
| **2** | Naive MCTS    | Finite-depth tree search, uniform action grid, demand sampled independently per candidate action                      | Fast but unreliable — actions get compared against different random futures |
| **3** | Enhanced MCTS | Adds common random numbers, structured action sets, urgency-aware scoring, rollout tail approximation                 | Best quality per unit compute; the only one stable at scale                 |

### Headline finding

Carefully structured approximation — not deeper lookahead alone — is what makes
stochastic service routing tractable. The naive controller at horizon 3 burns
**~100 minutes** to produce a policy that looks lucrative and is actually
overfit to favourable sample paths. The enhanced controller gets better
retention in **31 seconds**.

[Methods, results, and full paper on GitHub](https://github.com/Rajas124/retention-aware-delivery-routing)
