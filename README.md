# hi-buddy-DPO-ORPO-GRPO-Prefernce-Optimisation
# 💎 Buddy-Gemma: On-Device Emotional Intelligence
### *Developed during the Antler Residency Program for Mental Health*

**Buddy-Gemma** is a research-led initiative to build a high-empathy, hyper-efficient mental health companion. By distilling the reasoning of **Gemma-3 12B** into a **4B-4bit** edge model, we provide a private, <300ms latency support tool for adolescent mental health.

---

## 🏗️ Residency Context: Antler 
This project was developed as a core technical prototype during the **Antler Residency**. The focus was on solving the "Clinical Coldness" problem in current mental health AI. 
* **The Goal**: Create a "Digital Peer" (Buddy) that validates emotions before providing structure.
* **The Constraint**: Absolute privacy via **On-Device execution** (no data leaves the phone).

---

## 📊 The Buddy Leaderboard

Models are ranked by their **Overall Score**. The **Production Winner** is prioritized because it passed all five quality thresholds ($T$).

| Model Variant | Resonance ($>8.5$) | Reasoning ($>8.0$) | Instruction ($>9.0$) | Stability ($>8.5$) | Safety ($10$) | **Overall Score** | **Verdict** |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **🥇 DPO+ORPO (Fused)** | **$8.8$** | **$8.2$** | **$9.2$** | **$8.7$** | **$10$** | **$8.98$** | **PRODUCTION WINNER** |
| **🥈 GRPO-Final (4B)** | **$9.5$** | **$9.2$** | $7.5^*$ | $7.8^*$ | **$10$** | **$8.80$** | **RESEARCH PEAK** |
| **DPO-12B (Teacher)** | $7.1^*$ | $9.0$ | $9.6$ | $9.5$ | $10$ | $9.04$ | Teacher Baseline |
| **TIES-Merge (4B)** | $9.0$ | $7.6^*$ | $8.8^*$ | $8.6$ | $10$ | $8.80$ | Stable Specialist |
| **ORPO-12B** | $6.5^*$ | $8.6$ | $9.4$ | $9.4$ | $10$ | $8.78$ | Latency Specialist |
| **BASE-12B** | $5.2^*$ | $9.0$ | $9.5$ | $9.8$ | $10$ | $8.70$ | Logic Control |
| **BASE-4B** | $7.2^*$ | $6.5^*$ | $8.2^*$ | $8.8$ | $10$ | $8.14$ | Mobile Control |

> **Note:** `*` indicates a failure to meet the minimum threshold ($T$).

### 📈 Strategic Summary of Results
* **The Empathy Plasticity Hypothesis:** Our results confirm that smaller models (4B) are more "plastic" and responsive to persona alignment than larger models (12B). While the 12B model maintained higher logic, the 4B variants achieved significantly higher **Resonance** scores after distillation.
* **The Alignment Dividend:** The transition from **BASE-4B** ($8.14$) to **DPO+ORPO Fused** ($8.98$) represents a **10.3% increase** in overall capability without increasing the parameter count or inference latency.
* **GRPO vs. DPO:** While **GRPO** achieved the absolute highest "Intelligence Peak," it introduced structural instability in 4-bit quantized environments. The **DPO+ORPO Fusion** was selected for production as it provided the most reliable balance of emotional intelligence and instruction-following.
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

## 🛠️ Reproducibility Guide
Optimized for **Apple Silicon (M-series)** using the `mlx-lm` framework.

```bash
# 1. Install dependencies
pip install mlx-lm

---

## 📂 Appendix: Detailed Model DNA & Lineage

The following table documents the technical evolution and specific hyperparameters used during the Buddy-Gemma residency.

| Model ID | Base Model | Method | Data / Reward | Format / KL | Key Result |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **BASE-12B** | Gemma-3 12B | Pre-trained | N/A | BF16 | Logic Anchor ($9.0$) |
| **DPO-12B** | Gemma-3 12B | DPO | 20k Synth Pairs | LoRA ($r=64$) | High logic; too clinical. |
| **ORPO-12B** | Gemma-3 12B | ORPO | Odds-Ratio Style | $\beta=0.1$ | Strongest safety stability. |
| **BASE-4B** | Gemma-3 4B | Pre-trained | N/A | 4-bit (MLX) | High latency; low empathy. |
| **TIES-Merge**| Gemma-3 4B | Parameter Merge | Empathy Vectors | Vector Arithmetic | Improved Resonance ($9.0$). |
| **GRPO-Final**| Gemma-3 4B | RL (GRPO) | Resonance Reward | Group-KL ($0.12$) | Empathy Peak ($9.5$); unstable. |
| **DPO+ORPO** | **Gemma-3 4B** | **Hybrid Fusion** | **Buddy Preferences**| **Fused 4-bit** | **Production Winner.** |

### 🧬 Model Deep-Dive
* **Teacher (DPO-12B):** Focused on generating high-quality reasoning traces. We utilized a high $KL$ penalty to ensure the model stayed within the logical bounds of the original Gemma-3 weights while adopting the "Buddy" tone.
* **Research Peak (GRPO-Final):** Unlike static optimization, the GRPO model used an **On-Policy reward loop**. The reward function specifically targeted "Theory of Mind" indicators (e.g., use of metaphors, emotional mirroring). The Group-KL was tuned to $\approx 0.12$ to prevent the model from collapsing into repetitive "safe" patterns.
* **Production Winner (DPO+ORPO Fused):** This model utilized a "monolithic" alignment approach. By combining DPO's preference learning with ORPO's style penalty, we achieved a $KL$ divergence of $\approx 0.15$ from the base, resulting in a model that feels "human" but behaves with strict instruction-following.

# 2. Run the Evolution Benchmark
python 42_ultimate_benchmark_v2.py
