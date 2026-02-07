<div align="center">

# Trial-Nexus

<div align="center">

![Python Version](https://img.shields.io/badge/python-3.9+-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Status](https://img.shields.io/badge/status-experimental-orange.svg)
[![GitHub](https://img.shields.io/badge/github-ajeshraj402/Trial--Nexus-black.svg?logo=github)](https://github.com/ajeshraj402/Trial-Nexus)

**An experimental research codebase for matching patient queries to clinical trials**

[Key Features](#key-features) • [Installation](#installation) • [Quick Start](#quickstart) • [Documentation](#documentation) • [Contributing](#contributing)

</div>

---

## 🔬 Overview

Trial-Nexus combines classical information retrieval (BM25) with neural/LLM-based reranking and matching logic to produce TREC-style runs and evaluate retrieval + matching performance on the TREC Clinical Trials dataset.

This repository contains Jupyter notebooks that build indexes, run retrieval, call a local LLM server (Ollama) to produce inclusion/exclusion judgments, and evaluate results with precision/recall metrics and TREC run files.

---

## ✨ Key Features

- 🔍 **BM25 Retrieval Baseline** - Build and evaluate classical information retrieval over the TREC clinical trials corpus
- 🧠 **Neural Reranking** - Optional BioBERT-style reranker evaluation on retrieval outputs
- 🤖 **LLM-Based Matching** - Use local LLM (via Ollama) to perform trial-to-patient matching
  - Generate JSON labels for inclusion/exclusion criteria
  - Score/rerank candidates and write TREC run files
- 📊 **Comprehensive Evaluation** - Save runs and evaluation metrics for reproducible comparisons

---

## 📁 Repository Structure

```
Trial-Nexus/
├── notebooks/
│   ├── trialnexus.ipynb                      # Main TrialGPT-style matching workflow
│   └── non_user_bm25_biobert_eval.ipynb     # BM25 baselines & BioBERT evaluations
├── models/
│   ├── non_user/
│   │   └── bm25/
│   │       ├── index/                        # BM25 index files
│   │       └── runs/                         # BM25 run files
│   └── user/
│       └── trialgpt_local/                   # TrialGPT outputs & match caches
├── tests/
│   └── outputs/                              # Evaluation outputs & metrics CSVs
└── README.md
```

---

## 🛠️ Installation

### Prerequisites

- Python 3.9 or higher
- [Ollama](https://ollama.com/) (for LLM-based matching)

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/ajeshraj402/Trial-Nexus.git
   cd Trial-Nexus
   ```

2. **Create and activate virtual environment**
   ```bash
   python -m venv .venv
   source .venv/bin/activate   # On Windows: .venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install jupyterlab ir-datasets pandas numpy tqdm requests scikit-learn
   ```

4. **Optional: Install neural reranking dependencies**
   ```bash
   pip install transformers sentence-transformers
   ```

5. **Set up Ollama (for LLM matching)**
   - Install Ollama from [ollama.com](https://ollama.com/)
   - Pull the required model:
     ```bash
     ollama pull llama3.1:8b
     ```
   - Start Ollama server (runs on `http://localhost:11434` by default)

---

## 🚀 Quickstart

### Reproduce the Core Pipeline

1. **Configure the environment**
   
   Edit the notebook cells to set `PROJECT_ROOT` to your desired path, or use the default (repo root):
   ```python
   PROJECT_ROOT = Path.cwd().parent  # or set to specific path
   OLLAMA_URL = "http://localhost:11434/api/generate"
   OLLAMA_MODEL = "llama3.1:8b"
   ```

2. **Run BM25 baseline**
   
   Open `notebooks/non_user_bm25_biobert_eval.ipynb` and run cells in sequence to:
   - Build BM25 index
   - Generate baseline runs
   - Evaluate performance

3. **Run TrialGPT matching**
   
   Open `notebooks/trialnexus.ipynb` and run cells to:
   - Load BM25 candidates
   - Call LLM for inclusion/exclusion matching
   - Generate and evaluate TREC runs

4. **Inspect results**
   
   Results are saved in:
   - **Run files**: `models/*/*/*.run` (TREC format)
   - **Metrics**: `tests/outputs/*.csv`
   - **LLM outputs**: `models/user/trialgpt_local/`

---

## 📚 Documentation

### Notebooks & Workflows

#### BM25 Indexing & Baseline
- Reads documents from `clinicaltrials/2021/trec-ct-2022` dataset using `ir_datasets`
- Builds and saves BM25 index (`models/non_user/bm25/index/bm25_full.pkl`)
- Writes BM25 TREC runs for evaluation

#### Neural / BioBERT Evaluation
- Reranks BM25 candidates with neural reranker
- Computes Precision@K and Recall@K metrics
- Saves results to CSV

#### TrialGPT LLM Matching
- Constructs prompts with patient text and trial criteria
- Sends prompts to LLM (Ollama) for JSON-formatted inclusion/exclusion labels
- Computes match scores and generates TREC-style runs

### Configuration

Key variables to configure in notebooks:

| Variable | Description | Default |
|----------|-------------|---------|
| `PROJECT_ROOT` | Base path for models/runs | Repository root |
| `OLLAMA_URL` | LLM API endpoint | `http://localhost:11434/api/generate` |
| `OLLAMA_MODEL` | Model identifier | `llama3.1:8b` |
| `TOP_K` | Number of candidates to retrieve | Varies by notebook |
| `TOPIC_ID` | Specific topic/query ID for testing | All topics |
| `RUN_TAG` | Name tag for output runs | Auto-generated |

### Output Files

| Location | Contents |
|----------|----------|
| `models/non_user/bm25/index/` | BM25 index files (`.pkl`) |
| `models/non_user/bm25/runs/` | BM25 baseline runs (`.run`) |
| `models/user/trialgpt_local/` | TrialGPT outputs, matches, runs |
| `tests/outputs/` | Evaluation metrics (`.csv`) |

---

## 💡 Tips & Troubleshooting

### Common Issues

**Disk Space**
- Dataset download and indexing require significant disk space
- Ensure `PROJECT_ROOT` points to a drive with sufficient free space

**LLM Configuration**
- If you don't have Ollama, set `OLLAMA_URL` to a compatible LLM endpoint
- For testing without LLM, stub out the LLM-calling cells

**Path Issues (Windows)**
- Notebooks show paths as `C:\Ajesh_Drive\...`
- Edit paths for Linux/macOS compatibility

**Performance**
- If your LLM is slow, increase timeouts in the notebook
- Adjust `SLEEP_SEC` parameter between calls to avoid rate limits

**Testing Before Full Runs**
- Use test mode with small `TOP_K` values before running full pipeline
- Set `TEST_TOPICS` to a subset for initial validation

---

## 🤝 Contributing

Contributions are welcome! Here are some ways to improve the project:

- [ ] Add `requirements.txt` or `environment.yml` for dependency management
- [ ] Create runnable scripts (non-notebook) for automation/CI
- [ ] Add per-notebook README sections with exact execution order
- [ ] Improve error handling and logging
- [ ] Add unit tests for core functions
- [ ] Expand documentation with more examples

### How to Contribute

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📊 Evaluation Metrics

The pipeline tracks the following metrics:

- **Precision@K** - Precision at various cutoff ranks
- **Recall@K** - Recall at various cutoff ranks
- **TREC Run Files** - Standard format for clinical trials track evaluation
- **Match Cache Statistics** - Saved/skipped/error counts for LLM matching

Example output:
```
Total docs saved: 375580
Precision@10: 0.xxx
Recall@10: 0.xxx
FULL MATCHING DONE: Total saved: 7197
```

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 👤 Author

**Ajesh Raj**

- GitHub: [@ajeshraj402](https://github.com/ajeshraj402)
- Project Link: [https://github.com/ajeshraj402/Trial-Nexus](https://github.com/ajeshraj402/Trial-Nexus)

---

## 🙏 Acknowledgments

- TREC Clinical Trials dataset
- Ollama for local LLM inference
- BioBERT and related biomedical NLP tooling
- `ir_datasets` for standardized IR dataset access

---

<div align="center">

**⭐ Star this repository if you find it helpful!**

Made with ❤️ for clinical trials research

</div>
