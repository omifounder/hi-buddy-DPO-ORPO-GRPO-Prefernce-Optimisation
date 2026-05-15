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
To achieve the "Buddy" persona, we moved beyond generic datasets and used a three-stage synthetic-to-real data pipeline.

### 1. Synthetic "Theory of Mind" Data
We used the **Gemma-3 12B Teacher** to generate 15,000 multi-turn dialogues centered on "Teen Stressors" (academic pressure, social anxiety, future uncertainty). 
* **Focus**: The teacher was prompted to prioritize *validation* over *solutions*.

### 2. Preference Pairs (DPO/ORPO)
We curated a preference set of 5,000 pairs where:
* **Chosen**: Responses that used metaphors (e.g., "emotions are like waves") and peer-aligned language.
* **Rejected**: Responses that were overly clinical, preachy, or bullet-pointed list heavy.

### 3. Crisis Redirect Layer
A specialized subset of 1,000 samples was used to train the **Crisis Pivot**. If a trigger word is detected, the model is trained to maintain its warm tone but immediately bridge the user to 988 (US) or 111 (UK) resources.

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
