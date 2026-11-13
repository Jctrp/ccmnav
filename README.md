<h1 align="center">CCM-Nav</h1>

<p align="center"><strong>Cell-Inspired Cognitive Map Modeling for Navigation and Exploration</strong></p>

<p align="center">Turning partial observations and spatial experience into knowledge for action.</p>

<p align="center">
  <a href="#overview">Overview</a> ·
  <a href="#navigation-demos">Navigation Demos</a> ·
  <a href="#real-world-demos">Real-World Demos</a> ·
  <a href="#results">Results</a>
</p>

<p align="center">
  <img src="github/framework.png" alt="CCM-Nav: cognitive map maintenance, differentiable cell interaction and memory readout, and waypoint selection" width="100%">
</p>

## Overview

**CCM-Nav is a cell-inspired cognitive map modeling method for goal-directed navigation and autonomous exploration in unknown environments.** It combines local spatial encoding with persistent place and route memory, and learns how spatial cues interact and how stored knowledge is retrieved for action evaluation.

The cognitive map evolves as the robot observes and moves. A fixed navigation policy uses this maintained spatial knowledge to select successive waypoints; downstream planning and control execute the selected targets.

- **Construct spatial knowledge online.** A local graph organizes observed traversable space, while persistent memory retains places and routes beyond the current local window.
- **Learn how spatial cues work together.** Boundary and motion information modulate local spatial representations through independent cell-interaction pathways.
- **Retrieve knowledge relevant to actions.** Route-aware memory attention relates candidate directions to stored route entry, length, and availability.
- **Support navigation and exploration.** Point-goal navigation and goal-free exploration share the cognitive-map design, with task-specific observations, objectives, and checkpoints.

### Cognitive Map Maintenance

<p align="center">
  <img src="github/cognitive_map.png" alt="Successive local graph windows with places and routes retained in persistent memory" width="100%">
</p>

As the local window advances, earlier places and traversed connections remain available in memory. Local observations and retained experience jointly inform later decisions.

<details>
<summary><strong>Cell interaction and route-aware memory readout</strong></summary>

<p align="center">
  <img src="github/interaction_readout.png" alt="Independent boundary and head-direction gates, graph encoding, route-aware attention, and residual memory fusion" width="100%">
</p>

Grid-cell geometry, boundary information, movement direction, and place history are encoded into local graph features. These features query selected memory records through route-aware attention. The fused representation supports waypoint scoring and relative travel-cost prediction.

</details>

## Navigation Demos

<p align="center">
  <img src="github/navigation_process.png" alt="Three illustrative navigation sequences from initial observation through intermediate navigation to goal arrival" width="100%">
</p>

**Observed space expands, the local graph follows the robot, and the executed trajectory connects start to goal.** These illustrative sequences come from V2 training rollouts; benchmark results are reported separately below.

<details>
<summary><strong>Animated training rollouts with cell and memory diagnostics</strong></summary>

<table>
  <tr>
    <td align="center"><a href="github/navigation_01.gif"><img src="github/navigation_01.gif" alt="m6.3 navigation training rollout at episode 3000" width="320"></a></td>
    <td align="center"><a href="github/navigation_02.gif"><img src="github/navigation_02.gif" alt="m6.3 navigation training rollout at episode 5000" width="320"></a></td>
    <td align="center"><a href="github/navigation_03.gif"><img src="github/navigation_03.gif" alt="m6.3 navigation training rollout at episode 7000" width="320"></a></td>
  </tr>
  <tr>
    <td align="center">Episode 3000</td>
    <td align="center">Episode 5000</td>
    <td align="center">Episode 7000</td>
  </tr>
</table>

The animations show m6.3 training trajectories and their spatial diagnostics. Click an animation to inspect it at full resolution. Media provenance is recorded in [assets.json](github/assets.json).

</details>

## Real-World Demos

The same simulation-trained CCM-Nav navigation policy is deployed in previously unseen indoor and outdoor environments **without additional training or fine-tuning**. CCM-Nav maintains its cognitive map and selects successive waypoints online, while CMU-Exploration handles local planning and tracking.

<table>
  <tr>
    <th align="center">Indoor Navigation</th>
    <th align="center">Outdoor Navigation</th>
  </tr>
  <tr>
    <td align="center"><a href="github/real_world_indoor.gif"><img src="github/real_world_indoor.gif" alt="Real-world indoor RViz recording showing observed-map updates and the executed navigation trajectory" width="480"></a></td>
    <td align="center"><a href="github/real_world_outdoor.gif"><img src="github/real_world_outdoor.gif" alt="Real-world outdoor RViz recording showing observed-map updates and the executed navigation trajectory" width="480"></a></td>
  </tr>
  <tr>
    <td align="center">Corridor junctions and a narrow passage</td>
    <td align="center">Building-side roadway</td>
  </tr>
</table>


## Results

### Goal-Directed Navigation

Reported travel-distance comparison on the same **100 unseen test maps** and start–goal pairs:

| Method | Mean travel distance ± standard deviation (pixels) ↓ |
| --- | ---: |
| BIT\* | 275.2 ± 130.4 |
| D\* Lite | 224.6 ± 85.9 |
| CA | 253.9 ± 112.4 |
| Inpaint+A\* | 231.1 ± 125.8 |
| CogniPlan | 222.1 ± 64.7 |
| **CCM-Nav** | **207.7 ± 63.9** |

CCM-Nav achieves a **100% navigation success rate**, with **6.48% shorter mean travel distance** than CogniPlan. The reported overall inference comparison gives a **2.4× speedup**.

Baseline travel-distance statistics follow the published [CogniPlan comparison](https://proceedings.mlr.press/v305/wang25d.html). 
