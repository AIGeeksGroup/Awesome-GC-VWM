# Awesome Geometry-Consistent Video World Models

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub stars](https://img.shields.io/github/stars/AIGeeksGroup/Awesome-GC-VWM?style=social)](https://github.com/AIGeeksGroup/Awesome-GC-VWM)

A curated and continuously updated list of papers, datasets, and benchmarks for **Geometry-Consistent Video World Models (GC-VWM)** — generative video models that preserve stable scene geometry across viewpoints, time horizons, and camera trajectories.

> **Geometry-Consistent Video World Models: A Survey**
>
> [Zida Yang](https://scholar.google.com/citations?user=0XE_GW4AAAAJ&hl=en)\*, Yihao Lu\*, Liyang Wang\*, [Zeyu Zhang](https://steve-zeyu-zhang.github.io/)\*<sup>†</sup>, [Ling Shao](https://ling-shao.github.io/), [Hao Tang](https://ha0tang.github.io/)<sup>‡</sup>
>
> \*Equal contribution. <sup>†</sup>Project lead. <sup>‡</sup>Corresponding author.
>
> ### [Paper]() | [SSRN]()

This survey will be regularly updated here. If you find this useful, please consider giving us a ⭐!

<img width="6716" height="2971" alt="1" src="https://github.com/user-attachments/assets/082afe2a-5c03-492b-bf13-9eaea108ca6f" />

---

## Table of Contents

- [Overview](#overview)
- [Taxonomy](#taxonomy)
- [Comprehensive Method Comparison](#comprehensive-method-comparison)
- [Methods by Category](#methods-by-category)
  - [Without 3D Cache](#without-3d-cache)
  - [With 3D Cache](#with-3d-cache)
- [Datasets](#datasets)
- [Benchmarks](#benchmarks)
- [Related Surveys](#related-surveys)
- [Citation](#citation)
- [Star History](#star-history)

---

## Overview

Recent generative video models produce photorealistic clips but struggle to maintain consistent spatial structure under large camera motion or extended horizons. **GC-VWM** addresses this by integrating geometric reasoning — reprojection constraints, depth-aware warping, and spatial memory — into the generative process, enabling interactive exploration of generated worlds through precise camera control, reliable scene revisiting, and out-of-sight dynamics reasoning.

We organize the field into **two primary paradigms**:

| Paradigm | State Representation | Key Mechanism | Main Trade-off |
|---|---|---|---|
| **Without 3D Cache** | Latent state + 2D memory | Camera conditioning, geometric PE, RL/SFT alignment | Lightweight but limited geometric guarantees |
| **With 3D Cache** | Latent + 2D + 3D memory + world state | Explicit point cloud / surfel / 3DGS / hybrid patch | Strong consistency but higher compute & memory |

---

## Taxonomy

```
GC-VWM
├── Without 3D Cache
│   ├── Training-based Alignment (Geometry Forcing, Memory Forcing, LIVE)
│   ├── Camera-control Conditioning (CameraCtrl/II, SEVA, CamI2V, RealCam-I2V, EPiC)
│   ├── Geometry-aware Embeddings (ViewRope, UCPE, ReRoPE)
│   └── Latent Geometry Priors (Context as Memory, Diffusion as Shader)
└── With 3D Cache
    ├── Point Cloud / Surfel Memory (VMem, WorldMem, EvoWorld, Spatia)
    ├── Joint 4D Generation (WorldReel, VerseCrafter)
    ├── 3.5D Geometry Memory (GEN3C, NeoVerse)
    ├── Hierarchical / Compressed Memory (Infinite-World, RELIC)
    ├── Unified Camera–Memory (UCM, WorldStereo)
    ├── Hybrid Patch Memory (MosaicMem)
    └── Dynamic 3D Cache / World State (LiveWorld, PERSIST)
```

---

## Comprehensive Method Comparison

*44 methods surveyed, covering 2024–2026. Venues listed where confirmed.*

| Method | Year | Venue | Paradigm | Geo. Rep. | Temp. Model | Cam. Anno. | Training Strategy |
|---|---|---|---|---|---|---|---|
| MotionCtrl | 2024 | SIGGRAPH | w/o 3D | Camera cond. | Diffusion | Yes | End-to-end |
| DynamiCrafter | 2024 | ECCV | w/o 3D | Image cond. | Diffusion | No | Temporal prior |
| GenWarp | 2024 | NeurIPS | w/o 3D | Latent warp | Diffusion | Yes | Cross-view attn. |
| CameraCtrl | 2024 | ICLR'25 | w/o 3D | Pose cond. | Diffusion | Yes | Plug-and-play |
| CamI2V | 2024 | arXiv | w/o 3D | Epipolar attn. | Diffusion | Yes | Fine-tuning |
| CameraCtrl-II | 2025 | ICCV | w/o 3D | Pose cond. | Diffusion | Yes | Plug-and-play |
| SEVA | 2025 | ICCV | w/o 3D | Implicit | Diffusion | Yes | End-to-end |
| RealCam-I2V | 2025 | ICCV | w/o 3D | Metric depth | Diffusion | Yes | Metric-scale align. |
| EPiC | 2025 | arXiv | w/o 3D | Anchor video | Diffusion | No | Anchor-ControlNet |
| Geo. Forcing | 2025 | ICLR'26 | w/o 3D | Latent align. | Diffusion | Yes | Geo. feature align. |
| Mem. Forcing | 2025 | arXiv | w/o 3D | Latent memory | Diffusion | Yes | Memory supervision |
| DaS | 2025 | SIGGRAPH | w/o 3D | 3D tracking | Diffusion | Yes | Multi-task unified |
| Context as Mem. | 2025 | SIG Asia | w/o 3D | Latent retrieval | Diffusion | Partial | Memory retrieval |
| PanoWAN | 2025 | arXiv | w/o 3D | Spherical | Diffusion | Yes | Pano. alignment |
| CamCloneMaster | 2025 | SIG Asia | w/o 3D | Ref. video | Diffusion | No | Motion transfer |
| WorldPack | 2025 | arXiv | w/o 3D | Traj. memory | Diffusion | Yes | Memory comp. |
| ViewRope | 2026 | arXiv | w/o 3D | Ray-based PE | Transformer | Yes | PE injection |
| UCPE | 2025 | arXiv | w/o 3D | Ray encoding | Diffusion | Yes | PE injection |
| LIVE | 2026 | arXiv | w/o 3D | Cycle cons. | Diffusion | No | Cycle-consistency |
| AnchorWeave | 2026 | arXiv | w/o 3D | Local mem. | Diffusion | Yes | Anchor retrieval |
| ViewCrafter | 2024 | arXiv | w/ 3D | Point cloud | Diffusion | Yes | View cond. |
| GEN3C | 2025 | CVPR | w/ 3D | Depth→PC | Diffusion | Yes | 3D cache cond. |
| VMem | 2025 | ICCV | w/ 3D | Surfel memory | AR+retrieval | Yes | Plug-and-play mem. |
| WorldMem | 2025 | NeurIPS'25 | w/ 3D | State memory | Diffusion | Yes | State-aware attn. |
| EvoWorld | 2025 | arXiv | w/ 3D | Point cloud | Diffusion | Yes | 3D reproj. guide |
| Spatia | 2025 | CVPR'26 | w/ 3D | SLAM PC | Diffusion | Yes | SLAM update loop |
| WorldReel | 2025 | arXiv | w/ 3D | Pointmap+flow | Diffusion | Yes | Joint 4D gen. |
| WVD | 2025 | CVPR | w/ 3D | XYZ images | Diffusion | Yes | Co-generation |
| Geo4D | 2025 | ICCV | w/ 3D | Point/Ray map | Diffusion | Yes | Multi-modal align. |
| UniFuture | 2025 | arXiv | w/ 3D | Hier. scene | Diffusion | Yes | Hierarchical |
| TrajectoryCrafter | 2025 | ICCV | w/ 3D | Temp. PC | Diffusion | Yes | PC-guided |
| DeepVerse | 2025 | arXiv | w/ 3D | 4D Auto-reg. | AR | Yes | Geo. conditioning |
| Aether | 2025 | arXiv | w/ 3D | 4D Recon. | Diffusion | Yes | Action-conditioned |
| VerseCrafter | 2026 | arXiv | w/ 3D | 3DGS traj. | Diffusion | Yes | 4D control render. |
| NeoVerse | 2026 | arXiv | w/ 3D | 4D world | Recon. | No | Pose-free 4D |
| ReCapture | 2025 | CVPR | w/ 3D | Depth recon. | Diffusion | Yes | Masked fine-tuning |
| ReCamMaster | 2025 | ICCV | w/ 3D | Video cond. | Diffusion | Yes | Video conditioning |
| RELIC | 2025 | arXiv | w/ 3D | Compressed KV | AR diffusion | Yes | Self-forcing distill. |
| Infinite-World | 2026 | arXiv | w/ 3D | Hier. memory | AR diffusion | No | Pose-free HPMC |
| LiveWorld | 2026 | arXiv | w/ 3D | BG+entities | State evol. | Yes | Monitor mechanism |
| PERSIST | 2026 | arXiv | w/ 3D | Latent 3D scene | Scene evol. | Yes | 3D scene sim. |
| UCM | 2026 | arXiv | w/ 3D | PE warping | Dual-stream DiT | Yes | Time-aware PE warp |
| WorldStereo | 2026 | arXiv | w/ 3D | Dual geo. mem. | Diffusion | Yes | Global+spatial mem. |
| MosaicMem | 2026 | arXiv | w/ 3D | Hybrid patch 3D | Diffusion | Yes | Patch lift+compose |

---

## Methods by Category

### Without 3D Cache

#### Training-based Geometric Alignment

- **Geometry Forcing** — Aligning Geometric Representations for Video Diffusion Models (ICLR 2026) [[paper]](https://arxiv.org/abs/2502.09674)
- **Memory Forcing** — Memory Forcing for Geometry-Consistent Video World Models [[paper]](https://arxiv.org/abs/2504.04483)
- **LIVE** — Long-horizon Video Generation via Cycle-Consistency Objective [[paper]](https://arxiv.org/abs/2603.07145)
- **Self Forcing** — Self-Forcing: Bridging the Train-Test Gap in Autoregressive Video Generation [[paper]](https://arxiv.org/abs/2412.15089)
- **Rolling Forcing** — Rolling Diffusion for Real-Time Long Video Generation [[paper]](https://arxiv.org/abs/2503.04244)
- **Causal Forcing** — Causal Autoregressive Diffusion Distillation [[paper]](https://arxiv.org/abs/2603.03482)

#### Camera-control Conditioning

- **CameraCtrl** — Camera Motion Conditioned Video Diffusion (ICLR 2025) [[paper]](https://arxiv.org/abs/2404.02101) [[code]](https://github.com/hehao13/CameraCtrl)
- **CameraCtrl-II** — Dynamic Scene Exploration (ICCV 2025) [[paper]](https://arxiv.org/abs/2503.10592) [[project]](https://hehao13.github.io/Projects-CameraCtrl-II/)
- **CamI2V** — Epipolar-Geometry-Aware I2V [[paper]](https://arxiv.org/abs/2405.13048)
- **SEVA** — Stable Virtual Camera (ICCV 2025) [[paper]](https://arxiv.org/abs/2503.12263)
- **RealCam-I2V** — Metric-Scale Camera Control (ICCV 2025) [[paper]](https://arxiv.org/abs/2502.10218)
- **EPiC** — Efficient Camera-Control Learning with Anchor Videos [[paper]](https://arxiv.org/abs/2503.16081)
- **MotionCtrl** — Unified Camera and Object Motion Control (SIGGRAPH 2024) [[paper]](https://arxiv.org/abs/2312.03641)
- **CamCloneMaster** — Reference-Video-Driven Camera Control (SIG Asia 2025) [[paper]](https://arxiv.org/abs/2503.16973)

#### Geometry-aware Position Embeddings

- **ViewRope** — Geometry-Aware Rotary Position Encoding [[paper]](https://arxiv.org/abs/2603.13215)
- **UCPE** — Unified Camera Positional Encoding [[paper]](https://arxiv.org/abs/2503.02489)
- **ReRoPE** — Repurposing Rotary Embeddings for Camera Pose [[paper]](https://arxiv.org/abs/2603.17117)

#### Latent Geometry Priors

- **Context as Memory** — Memory Retrieval for Spatial Coherence (SIG Asia 2025) [[paper]](https://arxiv.org/abs/2501.14537)
- **Diffusion as Shader** — 3D-Tracking-Guided Video Diffusion (SIGGRAPH 2025) [[paper]](https://arxiv.org/abs/2501.03847)

#### Reward-based Alignment

- **Epipolar-DPO** — Epipolar-Geometry Reward for Diffusion [[paper]](https://arxiv.org/abs/2503.15408)
- **VGGRPO** — Latent-Space Geometry Alignment via GRPO [[paper]](https://arxiv.org/abs/2603.22275)

---

### With 3D Cache

#### Point Cloud / Surfel Memory

- **VMem** — Surfel-Indexed View Memory (ICCV 2025 Highlight) [[paper]](https://arxiv.org/abs/2506.18903) [[code]](https://github.com/runjiali-rl/vmem) [[project]](https://v-mem.github.io/)
- **WorldMem** — Long-term Consistent World Simulation with Memory (NeurIPS 2025) [[paper]](https://arxiv.org/abs/2504.12369)
- **EvoWorld** — Evolving 3D Memory for Panoramic Video Generation [[paper]](https://arxiv.org/abs/2504.07038)
- **Spatia** — SLAM-Updated Point Cloud Memory (CVPR 2026) [[paper]](https://arxiv.org/abs/2503.04606)
- **Voyager** — World-Consistent 3D Point-Cloud Generation [[paper]](https://arxiv.org/abs/2505.18762)

#### Joint 4D Generation

- **WorldReel** — Joint 4D RGB + Geometry Generation [[paper]](https://arxiv.org/abs/2504.07043)
- **VerseCrafter** — 4D Geometric Control via 3DGS Trajectories [[paper]](https://arxiv.org/abs/2603.02049)
- **WVD** — World Video Diffusion with Co-Generated XYZ (CVPR 2025) [[paper]](https://arxiv.org/abs/2501.09154)
- **Geo4D** — Monocular 4D Reconstruction via Video Diffusion (ICCV 2025) [[paper]](https://arxiv.org/abs/2504.07596)
- **GeometryCrafter** — Multi-Modal Geometry from Video Priors [[paper]](https://arxiv.org/abs/2504.01016)
- **TesserAct** — Joint RGB-DN 4D World Representation [[paper]](https://arxiv.org/abs/2504.14875)

#### 3.5D Geometry Memory

- **GEN3C** — 3D-Informed Video Generation (CVPR 2025 Highlight) [[paper]](https://arxiv.org/abs/2503.03751) [[code]](https://github.com/nv-tlabs/GEN3C) [[project]](https://research.nvidia.com/labs/toronto-ai/GEN3C/)
- **NeoVerse** — Pose-Free 4D World Models from Monocular Video [[paper]](https://arxiv.org/abs/2601.00393)
- **TrajectoryCrafter** — Camera Trajectory Guided Video Generation (ICCV 2025) [[paper]](https://arxiv.org/abs/2505.12810)

#### Hierarchical / Compressed Memory

- **Infinite-World** — Pose-Free Hierarchical Memory Compressor [[paper]](https://arxiv.org/abs/2603.12513)
- **RELIC** — Compressed KV Memory for Real-Time Generation [[paper]](https://arxiv.org/abs/2503.07849)
- **MemRoPE** — Dual-Stream Memory Compression for Infinite Video [[paper]](https://arxiv.org/abs/2603.26599)

#### Unified Camera–Memory Architectures

- **UCM** — Unified Camera-Memory via Time-Aware PE Warping [[paper]](https://arxiv.org/abs/2603.03482)
- **WorldStereo** — Dual Geometric Memories for Camera-Guided Generation [[paper]](https://arxiv.org/abs/2603.22212)

#### Hybrid Patch Memory

- **MosaicMem** — Lifted 3D Patch Memory for Diffusion-Native Conditioning [[paper]](https://arxiv.org/abs/2604.02677)

#### Dynamic 3D Cache / Evolving World State

- **LiveWorld** — Out-of-Sight Dynamics with Monitor-Based Evolution [[paper]](https://arxiv.org/abs/2603.13215)
- **PERSIST** — Persistent Latent 3D Scene Simulation [[paper]](https://arxiv.org/abs/2603.22275)
- **DeepVerse** — 4D Autoregressive World Model [[paper]](https://arxiv.org/abs/2504.10000)

---

## Datasets

| Dataset | Type | Scale | Pose | Depth | Metric | Primary Usage |
|---|---|---|---|---|---|---|
| [RealEstate10K](https://google.github.io/realestate10k/) | Posed video | ~80K clips | ✓ | — | — | Camera-controlled generation, NVS |
| [ACID](https://infinite-nature.github.io/) | Aerial video | Thousands | ✓ | — | — | Outdoor camera control |
| [RealCam-Vid](https://arxiv.org/abs/2502.10218) | Posed video | ~105K | ✓ | — | ✓ | Metric-scale camera control |
| [Princeton365](https://arxiv.org/abs/2505.01953) | Posed video | 365 videos | ✓ | — | ✓ | High-precision pose evaluation |
| [MultiCamVideo](https://arxiv.org/abs/2501.10706) | Synthetic multi-cam | 136K videos | ✓ | ✓ | ✓ | Multi-view consistency |
| [Virtual KITTI 2](https://arxiv.org/abs/2001.10773) | Synthetic driving | 5 sequences | ✓ | ✓ | ✓ | Domain transfer |
| [KITTI](http://www.cvlibs.net/datasets/kitti/) | Driving | 22 sequences | ✓ | ✓ | ✓ | Trajectory estimation |
| [Waymo Open](https://waymo.com/open/) | Driving | 2,030 segments | ✓ | ✓ | ✓ | Driving world models |
| [Argoverse 2](https://www.argoverse.org/av2.html) | Driving | 1,000 sequences | ✓ | ✓ | ✓ | Driving generation |
| [CO3D](https://github.com/facebookresearch/co3d) | Object-centric | 18,619 videos | ✓ | ✓ | — | Object-level NVS |
| [Ego4D](https://ego4d-data.org/) | Egocentric | ~3.7K hrs | Partial | Partial | — | Long-horizon ego tasks |

---

## Benchmarks

| Benchmark | Year | Target | Metrics | Focus |
|---|---|---|---|---|
| [Tanks & Temples](https://www.tanksandtemples.org/) | 2017 | 3D Recon. | Precision, Recall, F-score | Reconstruction accuracy |
| [VBench](https://arxiv.org/abs/2311.17982) | 2024 | Video Gen. | 16 dims: subject/bg consist., temporal | Disentangled video quality |
| [WorldModelBench](https://arxiv.org/abs/2504.07038) | 2025 | Video Gen. | Instruction, common sense, physics | Instruction & physics adherence |
| [VBench-2.0](https://arxiv.org/abs/2503.21755) | 2025 | Video Gen. | 18 dims: 3D spatial, camera, physics | Prompt faithfulness |
| [WorldScore](https://arxiv.org/abs/2504.00477) | 2025 | 3D/4D Gen. | Ctrl., quality, dynamics | Unified 3D/4D/video eval |
| [ViewBench](https://arxiv.org/abs/2603.13215) | 2026 | Video Gen. | PSNR, SSIM, LPIPS, LCE | View consistency & loop-closure |
| [StEvo-Bench](https://arxiv.org/abs/2603.03482) | 2026 | Video Gen. | State progress, physics | State evolution ≠ observation |
| [LiveBench](https://arxiv.org/abs/2603.13215) | 2026 | Video Gen. | PSNR, SSIM, VQA-Acc | OOS dynamics & identity |
| [Omni-WorldBench](https://arxiv.org/abs/2604.02677) | 2026 | Video Gen. | AgenticScore | Interactive camera & loop |

---

## Related Surveys

| Survey | Year | Scope |
|---|---|---|
| Tewari *et al.* — Advances in Neural Rendering | 2022 | Neural Rendering |
| Wang *et al.* — World Models for Autonomous Driving | 2024 | General WM |
| Ding *et al.* — Understanding World Models | 2024 | General WM |
| Kong *et al.* — World Modeling for 3D/4D | 2025 | 3D/4D WM |
| Cho *et al.* — Simulating the Visual World | 2025 | Visual AI |
| **Ours** | **2026** | **GC-VWM** |

---

## Citation

If you find this repository or our survey useful, please consider citing:

```bibtex
@article{yang2026gcvwm,
  title={Geometry-Consistent Video World Models: A Survey},
  author={Yang, Zida and Lu, Yihao and Wang, Liyang and Zhang, Zeyu and Tang, Hao},
  journal={IEEE Transactions on Pattern Analysis and Machine Intelligence},
  year={2026},
  note={Under Review}
}
```

---

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=AIGeeksGroup/Awesome-GC-VWM&type=Date)](https://star-history.com/#AIGeeksGroup/Awesome-GC-VWM&Date)

---

## Contributing

We welcome contributions! Please feel free to submit a Pull Request to add new papers, correct errors, or suggest improvements.

---

**Maintained by:** [Zida Yang](https://github.com/yzd2002) and co-authors.

**Last updated:** April 2026
