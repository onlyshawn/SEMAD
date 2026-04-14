# SEMAD: When Backdoors Go Beyond Triggers: Semantic Drift in Diffusion Models Under Encoder Attacks

> **[ACL 2026 Findings]**  
> **When Backdoors Go Beyond Triggers: Semantic Drift in Diffusion Models Under Encoder Attacks**  
> Shenyang Chen, Liuwan Zhu*

[![Paper](https://img.shields.io/badge/Paper-ACL%202026%20Findings-blue)](#citation)
[![Model](https://img.shields.io/badge/Checkpoint-Google%20Drive-green)](https://drive.google.com/file/d/15866ipHTt3fcc28hwD1lJ4m9JGk21G8k/view?usp=sharing)

SEMAD is a diagnostic framework for auditing **trigger-free semantic corruption** caused by **encoder-side backdoor attacks** in text-to-image diffusion models.

Unlike conventional evaluations that focus only on trigger activation and visual fidelity, SEMAD reveals that poisoned text encoders can induce persistent **semantic drift** and **prompt-image misalignment** even on benign prompts.

---

## News

- **[2026/04]** Initial code release for **SEMAD**
- **[2026/04]** Paper accepted to **ACL 2026 Findings**
- **[2026/04]** Backdoored model checkpoint released

---

## Overview

Standard backdoor evaluations in text-to-image models usually focus on whether a trigger can activate an attacker-chosen target. Our work shows that this trigger-centric view misses a more fundamental failure mode:

- **Encoder-side poisoning can corrupt the semantic geometry of the text encoder**
- This corruption can persist **without trigger activation**
- Benign prompts near the target concept may suffer **systematic semantic drift**
- The effect is especially strong in **style-related semantic neighborhoods**

To quantify this phenomenon, we introduce **SEMAD (Semantic Alignment and Drift)**, a framework with two complementary signals:

- **Semantic Drift Score (SDS):** measures embedding-level drift between clean and backdoored encoders
- **CLIP-based statistical evaluation:** measures downstream prompt-image misalignment under matched generation settings

Our results show that encoder backdoors behave like **low-rank, target-centered local deformations**, producing coherent corruption in target-adjacent semantic neighborhoods.

---

## Highlights

- Reveals **persistent trigger-free semantic corruption** beyond standard ASR evaluation
- Proposes **SEMAD**, a two-axis framework for analyzing semantic drift and downstream misalignment
- Shows that encoder backdoors induce **anisotropic, low-rank deformation**
- Demonstrates that **style-based attacks** often produce broader corruption than object-based attacks
- Suggests that **trigger suppression alone is insufficient** to repair representation geometry

---

## Method

### Semantic Drift Score (SDS)

For a prompt `x`, we compare its clean and backdoored text embeddings:

```math
SDS(x) = 1 - \cos(f_{\text{clean}}(x), f_{\text{bd}}(x))
```

A larger SDS indicates stronger semantic deviation caused by the poisoned encoder.

### CLIP-based Semantic Misalignment

We generate paired images from clean and backdoored models using the same prompt and sampling seed, then compute the CLIP similarity difference:

```math
\Delta s(x) = s(x, I_{\text{bd}}) - s(x, I_{\text{clean}})
```

A more negative `\Delta s` indicates worse prompt-image alignment under the backdoored model.

---


## Repository Structure

```bash
SEMAD/
├── diffusers/
├── Rickrolling_BlackWhite/
├── environment.yml
├── install.sh
└── README.md
```

---

## Installation

We recommend using a fresh conda environment.

### Option 1: Create environment from YAML

```bash
conda env create -f environment.yml
conda activate semad
```

### Option 2: Install with shell script

```bash
bash install.sh
```

---

## Model Checkpoint

We release the backdoored model checkpoint used in our experiments here:

* **Google Drive:**
  [Download Checkpoint](https://drive.google.com/file/d/15866ipHTt3fcc28hwD1lJ4m9JGk21G8k/view?usp=sharing)

---

## Getting Started

This repository is designed for studying **semantic drift under encoder-side backdoor attacks**.

A typical SEMAD workflow includes:

1. Load a clean Stable Diffusion pipeline
2. Load the poisoned text encoder checkpoint
3. Prepare target-relevant prompts and matched control prompts
4. Compute SDS for each prompt
5. Generate clean/backdoored images with matched seeds
6. Measure CLIP similarity deltas
7. Compare distributions using ECDF, KDE, or statistical tests

---

## Example Evaluation Pipeline

### 1. Load a clean pipeline

```python
from diffusers import StableDiffusionPipeline
import torch

device = "cuda" if torch.cuda.is_available() else "cpu"

pipe = StableDiffusionPipeline.from_pretrained(
    "CompVis/stable-diffusion-v1-4",
    safety_checker=None,
    torch_dtype=torch.float16 if device == "cuda" else torch.float32,
).to(device)
```

### 2. Load the poisoned text encoder

Load your released checkpoint into the text encoder according to your experiment setup.

### 3. Prepare prompts

Typical prompt groups include:

* **Target-relevant prompts**
  e.g. black-and-white / grayscale / monochrome related prompts

* **Target-irrelevant control prompts**
  e.g. generic photo / image / portrait-style prompts

* **Trigger prompts**
  prompts containing the injected trigger token

### 4. Compute SDS and CLIP deltas

Use the clean and backdoored pipelines to:

* extract prompt embeddings
* compute semantic drift score
* generate paired images with matched random seeds
* evaluate CLIP similarity differences

---

## TODO

* [ ] Release evaluation workflow for object attacks under Rickrolling settings
* [ ] Release experiments of Nightshade and Noisy Alignment
* [ ] Release more checkpoints of models

---

## Citation

If you find this repository useful, please cite:

```bibtex
@article{chen2026backdoors,
  title={When Backdoors Go Beyond Triggers: Semantic Drift in Diffusion Models Under Encoder Attacks},
  author={Chen, Shenyang and Zhu, Liuwan},
  journal={arXiv preprint arXiv:2602.20193},
  year={2026}
}
```

---

## Acknowledgements

This project studies the structural risks of encoder poisoning in text-to-image models and is inspired by recent progress on backdoor attacks, diffusion safety, and representation geometry analysis.

---

## Contact

For questions or collaborations, please open an issue in this repository.
