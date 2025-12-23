# Surrogate-Guided Constrained Optimization of Ultra-Scaled MoS₂ FET Designs

This repository provides a fully reproducible end-to-end pipeline for **constrained MoS₂ FET device design optimization**, combining a physics-inspired compact oracle, leak-free surrogate modeling, and surrogate-guided search.

The code accompanies the paper:

> **Surrogate-Guided Constrained Optimization of Ultra-Scaled MoS₂ FET Designs under Strict Manufacturability Constraints**  
> Youla Yang, Indiana University Bloomington (2025)

---

## 🔍 Overview

Designing ultra-scaled MoS₂ field-effect transistors (FETs) involves navigating a high-dimensional design space under strict manufacturability constraints on:

- On-current: \( I_{\mathrm{on}} \)  
- Leakage ratio: \( I_{\mathrm{off}}/I_{\mathrm{on}} \)  
- Subthreshold swing (SS)  
- Drain-induced barrier lowering (DIBL)

This repository implements a **Route-2 pipeline** that:

1. Defines a **physics-inspired compact oracle** for device metrics.
2. Trains **leak-free surrogate models** using group-based splits.
3. Performs **surrogate-guided constrained optimization**.
4. Benchmarks against:
   - Oracle random search,
   - Bayesian optimization (BO) baseline,
   - Optional LLM self-refinement heuristic.
5. Exports all artifacts for full reproducibility.

The goal is not to replace TCAD, but to provide a **controlled testbed** for comparing optimization strategies under identical budgets and constraints.

---

## ✨ Key Features

- ✅ Physics-inspired compact oracle for MoS₂ FET metrics  
- ✅ High-dimensional continuous + categorical design space  
- ✅ Group-based split to prevent surrogate data leakage  
- ✅ Surrogate-guided propose–validate optimization loop  
- ✅ Relaxed vs. strict manufacturability tiers  
- ✅ Multi-random-seed evaluation with mean ± std  
- ✅ Bayesian Optimization baseline  
- ✅ Optional LLM self-refinement baseline  
- ✅ One-cell Colab pipeline, fully reproducible  
- ✅ Automatic export of CSV/JSON logs and best designs  
- ✅ Pareto visualization of \( I_{\mathrm{on}} \) vs. leakage trade-offs  

---

## 📁 Repository Structure

```text
.
├── route2_pipeline.ipynb        # One-cell Colab notebook (main pipeline)
├── data/                        # Generated CSV/JSON artifacts (after running)
│   ├── oracle_dataset_*.csv
│   ├── optimization_runs_*.csv
│   ├── summary_*.csv
│   ├── best_designs_*.csv
│   └── surrogate_r2_*.json
├── figures/                     # Pareto plots
├── README.md
└── LICENSE
All files in data/ and figures/ are generated automatically by the pipeline.

⚙️ Requirements
The pipeline runs in standard Python/Colab environments.

Main dependencies:

Python ≥ 3.9

numpy

pandas

scikit-learn

matplotlib

tqdm

(Optional) openai for LLM self-refinement

Install via:

bash
Copy code
pip install numpy pandas scikit-learn matplotlib tqdm openai
🚀 Quick Start
Option 1: Run in Google Colab (Recommended)
Open route2_pipeline.ipynb in Colab.

Run the single cell.

All results will be generated and saved automatically.

Option 2: Run locally
bash
Copy code
git clone https://github.com/yourusername/mos2-surrogate-opt.git
cd mos2-surrogate-opt
python route2_pipeline.py
🧪 What the Pipeline Does
For each random seed:

Samples an initial oracle dataset.

Trains surrogate models for:

log
𝐼
o
n
I 
on
​
 ,

log
𝐼
o
f
f
/
𝐼
o
n
I 
off
​
 /I 
on
​
 ,

SS,

DIBL.

Runs optimization under:

Relaxed tier:
𝑟
≤
10
−
4
,
  
𝑆
𝑆
≤
130
r≤10 
−4
 ,SS≤130 mV/dec, 
𝐷
𝐼
𝐵
𝐿
≤
120
DIBL≤120 mV/V

Strict tier:
𝑟
≤
10
−
6
,
  
𝑆
𝑆
≤
90
r≤10 
−6
 ,SS≤90 mV/dec, 
𝐷
𝐼
𝐵
𝐿
≤
60
DIBL≤60 mV/V

Benchmarks:

random_oracle

surrogate_guided

bayesopt_oracle

(Optional) llm_self_refine

Aggregates multi-seed mean ± std results.

Exports all logs and best designs.

📊 Outputs
After running, you will find:

Oracle dataset:
oracle_dataset_*.csv

Surrogate accuracy:
surrogate_r2_*.json

Full optimization logs:
optimization_runs_*.csv

Summary tables:
summary_*.csv, summary_*_meanstd.csv

Best designs per method/tier:
best_designs_*.csv

Pareto plots:
log
⁡
10
(
𝑟
)
log 
10
​
 (r) vs. 
log
⁡
10
(
𝐼
o
n
)
log 
10
​
 (I 
on
​
 )

These files enable full post-hoc analysis and reproduction of all paper results.

🧠 Methods Implemented
Oracle random search

Surrogate-guided local search (propose–validate)

Bayesian optimization (oracle baseline, relaxed tier)

LLM self-refinement (optional, OpenAI-compatible API)

The surrogate uses HistGradientBoostingRegressor with:

median imputation,

one-hot encoding for contacts,

group-based train/test split by device family.

🔁 Reproducibility
All experiments are controlled by random seeds.

Multi-seed runs (e.g., seeds = 0, 1, 2) are aggregated automatically.

Every row in the logs records:

tier name,

tier thresholds,

feasibility flag,

oracle and surrogate metrics.

This ensures paper-grade auditability.

📖 Citation
If you use this code, please cite:

bibtex
Copy code
@article{yang2025mos2surrogate,
  title   = {Surrogate-Guided Constrained Optimization of Ultra-Scaled MoS2 FET Designs under Strict Manufacturability Constraints},
  author  = {Yang, Youla},
  journal = {arXiv preprint / under review},
  year    = {2025}
}
⚠️ Disclaimer
The oracle implemented here is a compact, physics-inspired model designed to reflect qualitative device trends.
It is not a substitute for high-fidelity TCAD simulation or experimental validation.
All reported results should be interpreted as method comparisons within a controlled testbed.

👤 Author
Youla Yang
Luddy School of Informatics, Computing, and Engineering
Indiana University Bloomington
📧 yangyoul@iu.edu

📄 License
This project is released under the MIT License. See LICENSE for details.

yaml
Copy code

---

如果你愿意，我可以再帮你做两件事之一：

- 🔗 根据你真实 repo 名和结构，替你**微调 README**（比如文件名、路径、Colab badge）；  
- 🏷️ 给你加上 **GitHub badges**（Python, license, Colab, arXiv, DOI），让主页更专业。

你打算仓库叫什么名字？比如：`mos2-surrogate-opt` / `Route2-FET-Design`？
