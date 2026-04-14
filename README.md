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
