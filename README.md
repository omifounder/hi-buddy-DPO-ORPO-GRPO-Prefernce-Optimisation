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

# 2. Run the Evolution Benchmark
python 42_ultimate_benchmark_v2.py
