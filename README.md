# REVEALER-Reinforcement-Guided-Visual-Reasoning-for-Element-Level-Text-Image-Alignment-Evaluation# REVEALER: Reinforcement-Guided Visual Reasoning for Element-Level Text-to-Image Alignment Evaluation


## Overview

**REVEALER** is a reinforcement-guided visual reasoning framework for **element-level text-to-image alignment evaluation**. Unlike existing coarse-grained metrics (e.g., CLIPScore) or static QA pipelines (e.g., TIFA), REVEALER performs fine-grained, interpretable evaluation by following a structured **"grounding–reasoning–conclusion"** paradigm.

<p align="center">
  <img src="pipeline.png" width="95%">
</p>

### Key Features

- **Element-Level Evaluation**: Decomposes prompts into fine-grained semantic elements (objects, attributes, spatial relations, etc.) and evaluates each independently.
- **Structured Visual Reasoning**: Follows a three-stage pipeline — *Grounding* (localizing elements via bounding boxes), *Reasoning* (free-form natural language analysis), and *Conclusion* (alignment scoring).
- **GRPO Optimization**: Leverages Group Relative Policy Optimization with a multi-dimensional reward function targeting format compliance, localization precision, and alignment accuracy.
- **Category-Aware Penalty**: Implements an adaptive penalty mechanism that suppresses over-grounding for abstract elements and encourages global reasoning.
- **State-of-the-Art Performance**: Surpasses strong proprietary models (Gemini 3 Pro) and training-based baselines across four benchmarks.

## Main Results

We evaluate REVEALER on four fine-grained benchmarks: **EvalMuse-40K**, **RichHF**, **MHaluBench**, and **GenAI-Bench**.

| Method | Model | EvalMuse-40K ACC | RichHF ACC | MHaluBench ACC | GenAI-Bench ACC |
|--------|-------|:---:|:---:|:---:|:---:|
| TIFA | Gemini 3 Pro | 81.3 | 80.8 | 81.0 | 83.9 |
| VQ² | GPT-4o | 81.7 | 77.9 | 80.7 | 84.1 |
| VIEScore | GPT-4o | 80.2 | 79.1 | 81.7 | 82.9 |
| SFT | Qwen3-VL-8B | 72.2 | 76.7 | 79.3 | 80.3 |
| **REVEALER** | **Qwen3-VL-8B** | **85.5** | **86.5** | **86.4** | **87.4** |

**Key Highlights:**
- **+13.3%** ACC over SFT baseline on EvalMuse-40K
- **+4.2%** ACC over Gemini 3 Pro on EvalMuse-40K
- **+19.6%** cumulative improvement over the base model in ablation studies

## Method

REVEALER consists of three main components:

### 1. Visual Reasoning Trajectory Curation
An automated pipeline that combines **Grounding DINO** (for visual localization) and **GPT-4o** (for reasoning generation) to construct structured training trajectories from the EvalMuse-40K dataset.

### 2. Two-Stage Training
- **Stage 1 — Cold-Start SFT**: Fine-tunes the MLLM on curated visual reasoning trajectories to learn the structured output format.
- **Stage 2 — GRPO**: Applies reinforcement learning with a composite reward function:
  - **Format Reward**: Ensures correct structured output format.
  - **Box Reward**: Evaluates localization accuracy with a category-aware penalty for abstract elements.
  - **Element Reward**: Assesses fine-grained alignment score accuracy.

