# Partitioning the Sky: A Region-based Graph Clustering Approach for UAV Delivery Services

## Overview

This paper addresses the problem of **drone-based package delivery** from a 
service computing perspective. Rather than treating delivery planning as a 
classical vehicle routing problem, it abstracts commercial drones as 
quality-aware, composable services — referred to as **Drone-as-a-Service (DaaS)** 
— and proposes a graph clustering strategy to efficiently orchestrate them over 
large, dynamic aerial networks.

## Problem Statement

Drone delivery involves two tightly coupled challenges:
- **Route planning:** finding an optimal flight path through a skyway network 
  of recharging stations from a source to a destination.
- **Drone selection:** identifying the best combination of drone services 
  that satisfies delivery constraints (cost, time, payload) while respecting 
  station resource availability.

Existing approaches either rely on centralized graph processing of the entire 
skyway network — which is computationally prohibitive at scale — or apply 
classical VRP/TSP formulations that fail to capture the functional heterogeneity 
of aerial infrastructure.

## Proposed Approach

The framework consists of four tightly integrated components:

### 1. Sky Network Modeling
The skyway network is modeled as a weighted graph where:
- **Nodes** represent recharging stations (with attributes such as pad type, 
  charging voltage, and congestion level).
- **Edges** represent skyway segments (with attributes such as distance, 
  wind speed, and flight restrictions).

### 2. Graph Clustering-based Partitioning
A SCAN-inspired structural clustering algorithm partitions the skyway network 
into locally coherent sky regions. Stations are classified into five roles:
- **Core stations** — dominant, heavily connected hubs.
- **Border stations** — peripheral nodes within a cluster.
- **Frontier stations** — inter-cluster gateways enabling path stitching.
- **Bridge stations** — connectors between clusters without cluster membership.
- **Outlier stations** — structurally isolated nodes.

A **hyper-graph abstraction** is then built over frontier and bridge stations 
to support efficient inter-cluster navigation.

### 3. Delivery Path Search
An enhanced **A\* algorithm** searches for the optimal delivery path by:
- Restricting exploration to relevant sky partitions rather than the full network.
- Using a path-level scoring function that minimizes the number of stations, 
  total flight distance, waiting time, and charging cost.
- Handling three delivery scenarios: single-cluster, two-cluster, and 
  multi-cluster path search.

### 4. DaaS Composition
A drone formation selection algorithm identifies the best combination of 
drone services by:
- Filtering drones whose flight range does not cover the delivery path.
- Greedily assigning package items to drones based on payload capacity.
- Ranking candidate formations using a QoS scoring function that balances 
  delivery time, cost, availability, and reputation.

## Key Contributions

- **Graph-based sky network model** incorporating station heterogeneity, 
  skyway segment features, and drone service QoS properties.
- **SCAN-inspired clustering algorithm** adapted to skyway networks, producing 
  non-exhaustive partitions with explicit frontier, bridge, and outlier node roles.
- **Hyper-graph abstraction** for compact and efficient inter-cluster path search.
- **Two-level scoring design** separating path-level optimization (driving A*) 
  from formation-level QoS evaluation (post-processing), preserving A* optimality.
- **Experimental validation** against two graph-based baselines (knowledge graph 
  embedding and single-graph methods) across multiple metrics.

## Experimental Results

Experiments conducted on a hybrid dataset combining real drone specifications 
with a synthetically generated skyway network demonstrate that the proposed 
approach outperforms baselines in terms of:

| Metric | Outcome |
|---|---|
| Composition quality | Higher station compatibility and shorter delivery paths |
| Energy efficiency | Lower energy consumption across drone formations |
| Failure rate | Reduced failure probability under varying payloads |
| Computation time | Faster path search due to reduced search space |

## Baselines

| Method | Description |
|---|---|
| **DaaS-GC** | Proposed graph clustering variant |
| **DaaS-HGC** | Proposed hyper-graph clustering variant |
| **DaaS-KGE** | Knowledge graph embedding baseline |
| **DaaS-SG** | Single-graph (full network) baseline |

## Future Work

- Swarm-based and multi-visit delivery scenarios.
- Dynamic station contention and real-time congestion handling.
- Uncertainty-aware composition using probabilistic graph clustering.
- Parallel processing using distributed frameworks (e.g., Apache Spark, GraphX).

## Implementation

The implementation is available on 
[Google Colab](https://drive.google.com/drive/folders/1f1-_BIOKb-DZnIa3Ng4EpyQK7XeMz0Fr?usp=sharing), 
using **NetworkX** and **Scikit-learn** for graph clustering and hyper-graph 
construction.

## Citation

If you use this work, please cite the corresponding paper (details to be added 
upon publication).
