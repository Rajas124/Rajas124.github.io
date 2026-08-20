---
layout: page
title: airline crew pairing
description: Column generation for crew scheduling — solving a set-partitioning problem with billions of columns by never enumerating them.
img: assets/img/projects/crew_pairing.png
importance: 1
category: optimization
---

**Julia · JuMP · Gurobi · Mixed-Integer Linear Programming**

An airline publishes next week's schedule — a few hundred flights between a hub
and a dozen cities — and somebody has to decide which crew flies which legs. A
crew signs in at their home base, flies a sequence of legs that connect, and has
to end up back home. There are limits on how long they can work, and if the
schedule strands them in the wrong city the airline buys them a passenger seat,
which costs money and pays them to sit still.

A sequence like that is a **pairing**. The job is to pick a set of pairings that
covers every flight exactly once, for as little money as possible. Written out
in full it is a set-partitioning problem with far more columns than can be
enumerated — so the model never enumerates them.

### Approach

Column generation splits the problem in two and runs a loop between the halves:

- a **master problem** picks the best combination of the pairings found so far;
- a **pricing problem** searches the flight network for a pairing that would
  improve that combination, guided by the master's dual prices.

The loop stops when the pricing problem can no longer find one. The relaxed
problem is solved exactly, then rounded to an integer schedule using the
pairings generated along the way.

### Results

Three synthetic hub-and-spoke instances — 50, 100, and 150 flights across 12
airports — solve to a covering schedule under FAA duty and rest limits. A
heuristic built on Gurobi's Solution Pool harvests many improving columns per
pricing call, cutting master-problem re-solves by **96%** and bringing solve
time down from hours to seconds.

Instances are seeded, so results reproduce exactly across runs.

### Where it could go next

The rounding step at the end is a heuristic; closing it properly means
**branch-and-price** — continuing to generate columns at every node of the
branch-and-bound tree. Other directions: overnight rest and multi-day duty
structure, multiple crew bases, delay costs, integrated crew-and-aircraft
scheduling, and dual stabilization to tame the price oscillation that slows
convergence on larger instances.

[Code, paper, and full write-up on GitHub](https://github.com/Rajas124/Airline-Crew-Pairing-Optimization)
