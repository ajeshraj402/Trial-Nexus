<div align="center">

# 🔬 Trial-Nexus

![Python Version](https://img.shields.io/badge/python-3.9+-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Status](https://img.shields.io/badge/status-experimental-orange.svg)
[![GitHub](https://img.shields.io/badge/github-ajeshraj402/Trial--Nexus-black.svg?logo=github)](https://github.com/ajeshraj402/Trial-Nexus)

**An experimental research codebase for matching patient queries to clinical trials**

[✨ Key Features](#-key-features) •
[🛠 Installation](#-installation) •
[🚀 Quick Start](#-quick-start) •
[📚 Documentation](#-documentation) •
[🤝 Contributing](#-contributing)

</div>

---

# 🔬 Overview

**Trial-Nexus** combines classical information retrieval (BM25) with neural / LLM-based reranking and matching logic to produce **TREC-style runs** and evaluate retrieval + matching performance on the **TREC Clinical Trials dataset**.

The repository provides Jupyter notebooks that:

* Build BM25 indexes
* Run retrieval baselines
* Call a local LLM server (Ollama) for inclusion/exclusion matching
* Generate TREC run files
* Evaluate results with precision / recall metrics

---

# ✨ Key Features

### 🔍 BM25 Retrieval Baseline

* Build and evaluate classical IR pipelines over the TREC clinical trials corpus
* Generate reproducible TREC run files

### 🧠 Neural Reranking

* Optional BioBERT / sentence-transformer reranking
* Evaluate improvements over BM25 baseline

### 🤖 LLM-Based Matching

* Local LLM inference using **Ollama**
* JSON inclusion/exclusion labeling
* Trial-to-patient matching logic
* Reranking + scoring with cached outputs

### 📊 Comprehensive Evaluation

* Precision@K and Recall@K metrics
* TREC-formatted runs for reproducibility
* CSV outputs for easy comparison

---

# 📁 Repository Structure

```text
Trial-Nexus/
│
├── notebooks/
│   ├── trialnexus.ipynb
│   └── non_user_bm25_biobert_eval.ipynb
│
├── models/
│   ├── non_user/
│   │   └── bm25/
│   │       ├── index/
│   │       └── runs/
│   │
│   └── user/
│       └── trialgpt_local/
│
├── tests/
│   └── outputs/
│
└── README.md
```

---

# 🛠 Installation

## Prerequisites

* Python **3.9+**
* [Ollama](https://ollama.com/) (for LLM-based matching)

---

## Setup

### 1️⃣ Clone the repository

```bash
git clone https://github.com/ajeshraj402/Trial-Nexus.git
cd Trial-Nexus
```

### 2️⃣ Create virtual environment

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
```

### 3️⃣ Install dependencies

```bash
pip install jupyterlab ir-datasets pandas numpy tqdm requests scikit-learn
```

### 4️⃣ Optional neural reranking dependencies

```bash
pip install transformers sentence-transformers
```

### 5️⃣ Setup Ollama

```bash
ollama pull llama3.1:8b
```

Ollama runs at:

```
http://localhost:11434
```

---

# 🚀 Quick Start

## 1️⃣ Configure environment

In notebooks:

```python
PROJECT_ROOT = Path.cwd().parent
OLLAMA_URL = "http://localhost:11434/api/generate"
OLLAMA_MODEL = "llama3.1:8b"
```

---

## 2️⃣ Run BM25 baseline

Open:

```
notebooks/non_user_bm25_biobert_eval.ipynb
```

Steps:

* Build BM25 index
* Generate baseline runs
* Evaluate performance

---

## 3️⃣ Run TrialGPT matching

Open:

```
notebooks/trialnexus.ipynb
```

Steps:

* Load BM25 candidates
* Call LLM for matching
* Generate + evaluate TREC runs

---

## 4️⃣ Inspect results

| Output               | Location                      |
| -------------------- | ----------------------------- |
| BM25 run files       | `models/non_user/bm25/runs/`  |
| LLM matching outputs | `models/user/trialgpt_local/` |
| Metrics CSVs         | `tests/outputs/`              |

---

# 📚 Documentation

## 🔹 BM25 Indexing & Baseline

* Uses `ir_datasets` to load TREC Clinical Trials data
* Builds BM25 index → `bm25_full.pkl`
* Writes TREC run files

## 🔹 Neural / BioBERT Evaluation

* Reranks BM25 candidates
* Computes Precision@K / Recall@K
* Saves evaluation CSVs

## 🔹 TrialGPT LLM Matching

* Builds prompts from patient text + trial criteria
* Calls Ollama LLM
* Produces JSON inclusion/exclusion labels
* Generates ranked TREC outputs

---

# ⚙️ Configuration

| Variable       | Description               | Default                               |
| -------------- | ------------------------- | ------------------------------------- |
| `PROJECT_ROOT` | Base path for models/runs | Repo root                             |
| `OLLAMA_URL`   | LLM endpoint              | `http://localhost:11434/api/generate` |
| `OLLAMA_MODEL` | LLM model name            | `llama3.1:8b`                         |
| `TOP_K`        | Retrieval candidates      | Notebook-specific                     |
| `RUN_TAG`      | Output run tag            | Auto                                  |

---

# 💡 Tips & Troubleshooting

### Disk Space

* Dataset + index may require **several GB**
* Set `PROJECT_ROOT` to a disk with sufficient space

### LLM Issues

* If Ollama is not available, stub LLM calls
* Increase timeout if local inference is slow

### Windows Path Fix

Update notebook paths if they reference:

```
C:\Ajesh_Drive\...
```

### Test Before Full Runs

Use:

```python
TOP_K = 10
TEST_TOPICS = ["T1", "T2"]
```

before running the full dataset.

---

# 🤝 Contributing

Contributions are welcome!

### Suggested Improvements

* Add `requirements.txt` or `environment.yml`
* Convert notebooks → runnable CLI scripts
* Add logging + error handling
* Add unit tests
* Improve evaluation dashboards

---

## How to Contribute

```bash
git checkout -b feature/my-feature
git commit -m "Add new feature"
git push origin feature/my-feature
```

Then open a Pull Request 🚀

---

# 📄 License

This project is licensed under the **MIT License**.

---

# 👤 Author

**Ajesh Raj**

* GitHub: https://github.com/ajeshraj402
* LinkedIn: https://www.linkedin.com/in/ajesh-nadar/
* SubStack: https://ajeshnadar.substack.com/

---

# 🙏 Acknowledgments

* TREC Clinical Trials dataset
* Ollama for local LLM inference
* BioBERT + biomedical NLP ecosystem
* `ir_datasets` for standardized dataset access

---

<div align="center">

⭐ **Star this repository if you find it useful!**
Made with ❤️ for clinical trials research

</div>
