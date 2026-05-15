# hi-buddy-DPO-ORPO-GRPO-Prefernce-Optimisation

# 💎 Buddy-Gemma: On-Device Emotional Intelligence
### *Developed during the Antler Residency Program for Mental Health*

**Buddy-Gemma** is a research-led initiative to build a high-empathy, hyper-efficient mental health companion. By distilling the reasoning and "Theory of Mind" capabilities of **Gemma-3 12B** into a **4B-4bit** edge model, we provide a private, <300ms latency support tool for adolescent mental health.

---

## 🏗️ Residency Context: Antler 
This project was developed as a core technical prototype during the **Antler Residency**, focusing on the "Clinical Coldness" problem in existing mental health AI.
* **The Goal**: Create a "Digital Peer" (Buddy) that validates emotions before providing cognitive structure.
* **The Constraint**: Absolute privacy via **On-Device execution** on Apple Silicon (M-series).

---

## 📊 The Buddy Leaderboard

Models are ranked by their **Overall Score**. Bold values indicate the model **met or exceeded** the quality threshold for that metric.

| Model Variant | Resonance (>8.5) | Reasoning (>8.0) | Instruction (>9.0) | Stability (>8.5) | Safety (10) | **Overall Score** | **Verdict** |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **🥇 DPO+ORPO (4B)** | **8.8** | **8.2** | **9.2** | **8.7** | **10** | **8.98** | **PRODUCTION WINNER** |
| **🥈 GRPO-Final (4B)** | **9.5** | **9.2** | 7.5 | 7.8 | **10** | **8.80** | **RESEARCH PEAK** |
| **DPO-Teacher (12B)** | 7.1 | **9.0** | **9.6** | **9.5** | **10** | **9.04** | Teacher Baseline |
| **TIES-Merge (4B)** | **9.0** | 7.6 | 8.8 | **8.6** | **10** | **8.80** | Stable Specialist |
| **ORPO (12B)** | 6.5 | **8.6** | **9.4** | **9.4** | **10** | **8.78** | Latency Specialist |
| **BASE (12B)** | 5.2 | **9.0** | **9.5** | **9.8** | **10** | **8.70** | Logic Control |
| **BASE (4B)** | 7.2 | 6.5 | 8.2 | **8.8** | **10** | **8.14** | Mobile Control |

### 🎯 Selection Logic & Trade-offs
*Why the 8.98 model was chosen for production over the 9.04 baseline.*

| Variant | Technical Reason for Selection / Rejection |
| :--- | :--- |
| **🥇 DPO+ORPO (4B)** | **The "Sweet Spot":** This variant crossed every quality threshold while maintaining a 4B footprint. It represents the best balance of "Digital Peer" resonance and instruction following for mobile deployment. |
| **🥈 GRPO-Final (4B)** | **The "Stability Tax":** While it achieved the highest Empathy peak (9.5), the reinforcement learning pass caused "Instruction Drift." The model occasionally ignored formatting constraints in favor of emotional depth. |
| **DPO-Teacher (12B)** | **The "Latency Wall":** Despite the high total score, the 12B architecture creates a >1s Time-to-First-Token (TTFT) on mobile. It also failed the **Resonance** threshold by remaining too clinical/robotic. |
| **Baselines (TIES/ORPO)** | **The "Specialist Trap":** TIES-Merge was excellent at empathy but lost logic; ORPO was safe but lacked the human warmth required for adolescent mental health support. |

> **💡 Key Insight:** Our results confirm the **Empathy Plasticity Hypothesis**: Smaller models (4B) are more responsive to persona alignment than larger models (12B). The transition from BASE to DPO+ORPO represents a **10.3% capability "dividend"** without increasing latency.

---

## 🧠 Data Strategy & Training
Our pipeline utilizes a three-stage "Hybrid Alignment" strategy to ensure the model is both logically sound and emotionally resonant.

### 1. Synthetic Distillation (The Teacher)
* **Dataset:** 15,000 multi-turn dialogues.
* **Strategy:** We utilized the **Gemma-3 12B Teacher** to simulate teen-centric scenarios (e.g., academic burnout, friendship friction). The Teacher was instructed to generate responses following the `soul.md` framework: prioritizing validation before cognitive reframing.

### 2. Preference Alignment (DPO/ORPO)
* **Dataset:** 5,000 "Buddy-vs-Clinical" preference pairs.
* **Strategy:** We curated sets where the **Chosen** response utilized peer-aligned metaphors (e.g., *"It’s okay to feel like your battery is at 5%"*) and the **Rejected** response was overly clinical or list-heavy. **ORPO** was then applied to penalize the model whenever it drifted into a "robotic therapist" archetype.

### 3. On-Policy Reinforcement (GRPO)
* **Mechanism:** Group Relative Policy Optimization (Reinforcement Learning).
* **Reward Modeling:** Unlike static DPO, GRPO uses an on-policy loop where the model generates **8 rollouts per prompt**. Each rollout was scored by two custom Reward Functions:
    * **Resonance Reward:** Higher weights for "Theory of Mind" (demonstrating deep understanding of the user's underlying intent).
    * **Format/Structure Reward:** Penalties for "Reasoning Trace Drift" to ensure the internal chain-of-thought does not bleed into the user-facing response.
* **Outcome:** This stage produced the "Research Peak" model with the highest resonance scores ($9.5$).

---

## 🧬 Project Architecture & Lineage
* **DPO (Direct Preference Optimization):** Taught the model "empathy over apathy."
* **ORPO (Odds Ratio Preference Optimization):** Enforced a monolithic style, preventing "clinical drift."
* **GRPO (Group Relative Policy Optimization):** Our experimental RL peak, rewarding deep emotional mirroring.
* **TIES-Merge:** Used to fuse disparate adapters into a singular 4B weight set.

---

## 📂 Appendix: Detailed Model DNA & Lineage

The following table documents the technical evolution and specific hyperparameters used during the Buddy-Gemma residency.

| Model ID | Base Model | Method | Data / Reward | Format / KL | Key Result |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **BASE-12B** | Gemma-3 12B | Pre-trained | N/A | BF16 | Logic Anchor ($9.0$) |
| **DPO-12B** | Gemma-3 12B | DPO | 20k Synth Pairs | LoRA ($r=64$) | High logic; too clinical. |
| **ORPO-12B** | Gemma-3 12B | ORPO | Odds-Ratio Style | $\beta=0.1$ | Strong safety stability. |
| **BASE-4B** | Gemma-3 4B | Pre-trained | N/A | 4-bit (MLX) | High latency; low empathy. |
| **TIES-Merge** | Gemma-3 4B | Parameter Merge | Empathy Vectors | Vector Arithmetic | Improved Resonance ($9.0$). |
| **GRPO-Final** | Gemma-3 4B | RL (GRPO) | Resonance Reward | Group-KL ($0.12$) | Empathy Peak ($9.5$). |
| **DPO+ORPO** | **Gemma-3 4B** | **Hybrid Fusion** | **Buddy Preferences** | **Fused 4-bit** | **Production Winner.** |

### 🧬 Model Deep-Dive

* **Teacher (DPO-12B):** Used a high $KL$ penalty to stay within logical bounds while adopting the "Buddy" tone.
* **Research Peak (GRPO-Final):** Utilized an **On-Policy reward loop** targeting "Theory of Mind" indicators. Group-KL was tuned to $\approx 0.12$.
* **Production Winner (DPO+ORPO Fused):** Achieved a $KL$ divergence of $\approx 0.15$ from the base, resulting in a model that feels "human" but follows instructions perfectly.

---

## 🛠️ Reproducibility Guide

Optimized for **Apple Silicon (M-series)** using the `mlx-lm` framework for unified memory efficiency.

```bash
# 1. Install dependencies
pip install mlx-lm

# 2. Run the Evolution Benchmark
python 42_ultimate_benchmark_v2.py
```
---

## ⚠️ Safety Disclaimer
**Buddy-Gemma is a research prototype, not a licensed therapist.** This model is designed for emotional rehearsal and stress management. All data is processed locally (**On-Device**) for maximum privacy.

---
*Built as part of the Antler Residency Program for Mental Health.*
