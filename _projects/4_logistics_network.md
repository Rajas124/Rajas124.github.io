---
layout: page
title: multi-commodity logistics network
description: A multi-commodity MIP routing four commodities across a two-stage network — cheapest plan vs. fastest plan, and the five scenarios in between.
img: assets/img/projects/logistics_network.png
importance: 4
category: optimization
---

**Python · GAMSPy · CPLEX / Xpress · Mixed-Integer & Stochastic Programming**

A mixed-integer program that routes four commodities — fuel, troops, ammunition
and weapons — from three supply depots, through four forward staging stations,
to four delivery points on an island, using a mixed fleet of boats, planes,
trains and trailers.

The model answers two questions that pull against each other — _what is the
cheapest plan?_ and _what is the fastest plan?_ — and then five operational
follow-ups about deadlines, trade-offs, uncertain demand and worst cases.

> **Cheapest plan: \$872,899. Fastest plan: 13.05 hours, at \$9,103,212.**
> Everything interesting in this project lives between those two numbers.

### The network

Cargo moves along a directed, two-stage network — supply → forward staging →
delivery — with no tier-skipping and no backflow. Vehicles serve one leg only;
cargo changes vehicle at the forward station. Distances are great-circle
distances via the Haversine formula, and delivery coordinates and demand
quantities are user inputs — change them and the whole study re-runs.

### The fleet

Each vehicle is modelled individually so a binary variable can switch a single
one on or off.

| Connector | Cost (\$/ton-mile) | Fixed (\$/trip) | Speed (mph) | Payload (tons) | Restricted to            |
| :-------- | -----------------: | --------------: | ----------: | -------------: | :----------------------- |
| Boat      |               3.00 |          75,000 |          21 |             50 | Sea legs                 |
| Plane     |               2.37 |          50,000 |         336 |             19 | Anywhere, but never fuel |
| Train     |               0.05 |           1,000 |          70 |             70 | Inland legs              |
| Trailer   |               0.16 |             100 |          35 |            4.5 | Overland only            |

That cost column is the whole tension in one line: a plane is roughly **47×** the
per-ton-mile cost of a train and **15×** that of a trailer, but moves 10–16×
faster than anything else in the fleet.

### Scenarios

Five follow-up studies extend the baseline: maximising deliverable tonnage
inside a 15-hour window, tracing the cost–time Pareto frontier, sizing the
fleet, and handling uncertain demand through a stochastic programming model with
scenario analysis.

[Notebook, formulation, and results on GitHub](https://github.com/Rajas124/Logistics-Network-Optimization)
