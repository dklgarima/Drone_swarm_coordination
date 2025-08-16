# Formal Modeling and Verification of a Decentralized Drone Swarm

**Author:** Garima Dhakal (Technische Universität Dresden)

This repository contains the model, properties, data, and final report for the formal verification of a decentralized drone swarm coordination protocol.

The system models a swarm of drones navigating a 2D grid with obstacles. The goal is for each drone to reach its target while avoiding collisions. The model is a Markov Decision Process (MDP) that captures both the non-deterministic choices of drone movements and the probabilistic outcomes of those movements.

## Repository Contents
* `report.pdf`: The full research report.
* `model.txt`: The source code for the PRISM/ProFeat model.
* `property.txt`: The PCTL properties used for verification.
* `data.txt`: The raw output data from the PRISM model checker.

## Model Parameters
The core experiments were run with the following configuration:
- **Grid Size**: `5x5`
- **Number of Drones**: 3
- **Start/Goal Positions**:
  - Drone 0: (0,0) -> (4,4)
  - Drone 1: (0,3) -> (4,0)
  - Drone 2: (4,0) -> (0,4)
- **Obstacles**: (2,1), (1,3), (3,4)
- **Model Size (for 3 drones)**:
  - **States**: 19,996,372
  - **Transitions**: 103,284,216

## How to Run
1.  Ensure you have [PRISM](https://www.prismmodelchecker.org/) installed.
2.  Since the model uses features, you may need the [ProFeat](https://wwwtcs.inf.tu-dresden.de/ALGI/PUB/ProFeat/) extension to compose the model first.
3.  Load the generated `.prism` file into PRISM.
4.  Load the properties file (`.txt`) or enter the properties manually to verify them. The properties used in the experiments are listed in the tables below.

## Experiments and Results

We conducted several experiments to analyze the swarm's performance and safety. The full raw output can be found in `data.txt`.

### Experiment 1: Impact of Movement Predictability

This experiment analyzes how a drone's movement predictability (`P_STRAIGHT`) affects mission success and systemic failure rates. As predictability decreases, the best-case probability of mission success (`Pmax (all goals)`) drops sharply. Conversely, the unavoidable probability of total swarm failure (`Pmin (all drones collide)`) increases, and the swarm's vulnerability to a worst-case policy forcing a crash on obstacles (`Pmax (all crash on obstacle)`) remains high.

| P_STRAIGHT | P_DEVIATION | Pmax (all goals) | Pmin (all drones collide) | Pmax (all crash on obstacle) |
| :---: | :---: | :---: | :---: | :---: |
| **0.99** | 0.005 | 0.907 | 0.092 | 0.985 |
| **0.95** | 0.025 | 0.575 | 0.425 | 0.929 |
| **0.90** | 0.050 | 0.288 | 0.711 | 0.870 |
| **0.85** | 0.075 | 0.130 | 0.869 | 0.804 |
| **0.75** | 0.125 | 0.030 | 0.969 | 0.698 |
| **0.65** | 0.175 | 0.008 | 0.992 | 0.604 |
| **0.55** | 0.225 | 0.002 | 0.998 | 0.522 |

*Note: `Pmin (all drones collide)` is the minimum probability that **all** drones will collide with each other, even with an optimal policy. `Pmax (all crash on obstacle)` is the maximum probability that **all** drones can be forced to crash on obstacles by a worst-case policy.*

### Experiment 2: Scalability with Number of Drones

This experiment measures the change in success and failure probabilities as the number of drones increases on a `5x5` grid, with `P_STRAIGHT` held constant at `0.99`.

| Drones | Pmax (all goals) | Pmin (all drones collide) | Pmax (all crash on obstacle) |
| :---: | :---: | :---: | :---: |
| 1 | 0.980 | 0.019 | 0.995 |
| 2 | 0.950 | 0.049 | 0.990 |
| 3 | 0.907 | 0.092 | 0.985 |

*Note: The results show that as agent density increases, the best-case success probability decreases, while the unavoidable chance of total swarm failure increases.*
