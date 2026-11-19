<p align="center">
  <img src="assets/mark.svg" width="64" alt="LoFT logo">
</p>

<h1 align="center">LoFT</h1>
<h3 align="center">Looped Foundation Transformer for Video Geometry</h3>

<p align="center">
  <a href="https://horizonrobotics.github.io/robot_lab/loft/">Project Page</a>
  &nbsp;·&nbsp;
  <a href="https://horizonrobotics.github.io/robot_lab/loft/#method">Method</a>
  &nbsp;·&nbsp;
  <a href="https://horizonrobotics.github.io/robot_lab/loft/#results">Results</a>
</p>

<p align="center"><strong>Code coming soon.</strong></p>

## Overview

**LoFT** reconstructs 3D scenes from RGB video by combining temporal attention with iterative camera refinement. It estimates camera poses, field of view, and per-frame depth and confidence, supporting 3D reconstruction from continuous video.

LoFT uses two loops: one aggregates visual features across frames, and the other refines camera predictions by feeding the updated camera state back into the model. Each loop shares its own weights across iterations, so a single model can run at different refinement depths.

### Highlights

- **Video continuity:** progressively wider temporal attention windows alternate with attention over the full clip, connecting nearby frames and distant views of the same scene.
- **Explicit camera feedback:** each refinement step uses the updated camera state to guide the next feature update.
- **Shared weights across iterations:** the number of refinement steps can vary without changing the model's parameters.
- **Broad training data:** training spans 80 public datasets, with evaluation on SpatialBench, ClassicBench, and VRBench.

## Method

![LoFT architecture: RGB frames pass through visual aggregation and camera refinement loops to predict cameras, depth, and confidence for 3D reconstruction.](assets/method.webp)

### Visual aggregation loop (LB1)

Visual features are extracted with DINOv3 and aggregated through alternating local and global attention. Local temporal windows expand from **3 → 5 → 9 → 17 frames**. Global attention connects every frame to the full clip, including distant observations of the same place.

### Camera refinement loop (LB2)

Each iteration predicts camera pose and field of view, then feeds the updated camera state into the next feature update. A dense prediction head decodes depth and confidence once per frame. Camera estimates and depth maps are combined to reconstruct the scene.

## Training scale

| Model parameters | Public training datasets | Sequences | Images |
| :---: | :---: | :---: | :---: |
| 514.5M | 80 | 2.97M | 961.8M |

## Benchmark results

LoFT achieves competitive depth and camera pose estimation results across three reconstruction benchmarks:

- **SpatialBench Medium:** 129 sequences in the Full split; the Continuous subset contains 98 sequences with uninterrupted observations.
- **ClassicBench:** seven established datasets: 7-Scenes, Neural RGB-D, Bonn, TUM Dynamic, Sintel, DyCheck, and KITTI.
- **VRBench:** 12 public datasets and two collected datasets, covering indoor and outdoor scenes, autonomous driving, and robotic manipulation.

The table below summarizes LoFT's results reported on the [project page](https://horizonrobotics.github.io/robot_lab/loft/#results).

| Benchmark | AbsRel ↓ | δ₁.₀₃ ↑ | AUC@3 ↑ | AUC@30 ↑ | ATE ↓ |
| :--- | ---: | ---: | ---: | ---: | ---: |
| SpatialBench Medium — Full | 0.0637 | 0.6959 | 0.4767 | 0.8341 | 0.5041 |
| SpatialBench Medium — Continuous | 0.0734 | 0.6765 | 0.4757 | 0.8342 | 0.2131 |
| ClassicBench | 0.1316 | 0.5464 | 0.3506 | 0.7623 | 0.5015 |
| VRBench | 0.0852 | 0.5751 | 0.3879 | 0.8271 | 0.2973 |

AbsRel and δ₁.₀₃ measure depth accuracy, AUC measures relative camera pose accuracy, and ATE measures trajectory error. Arrows indicate the better direction for each metric. See the project page for full comparisons and evaluation notes.

## Release status

| Resource | Status |
| :--- | :--- |
| [Project page](https://horizonrobotics.github.io/robot_lab/loft/) | Available |
| Source code | Coming soon |
| Paper | Coming soon |
| Hugging Face release | Coming soon |

Watch this repository for the code release. Installation and usage instructions will be added with the release, and citation information will be added when the paper is published.
