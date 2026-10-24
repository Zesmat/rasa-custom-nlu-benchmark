# Rasa Custom NLU & Pipeline Benchmark Harness

[![Python 3.10](https://img.shields.io/badge/python-3.10-blue.svg)](https://www.python.org/downloads/release/python-3100/)
[![Rasa Open Source](https://img.shields.io/badge/rasa-3.6+-purple.svg)](https://rasa.com/)

An empirical benchmarking project comparing Rasa NLU pipeline configurations (lightweight DIET vs. pre-trained Transformers & spaCy).


---

## Project Goal

To build an evaluation harness that tests and benchmarks different Rasa NLU architectures on the same domain dataset, measuring:
- Intent classification accuracy & F1-scores
- Entity extraction precision & recall
- Training and inference latency

---

## Tech Stack

- **Framework:** Rasa Open Source (v3.6+)
- **NLP / Embeddings:** spaCy (`en_core_web_md`), Hugging Face Transformers
- **Language:** Python 3.10
- **Package Management:** `uv`

---

## Project Structure

```text
├── components/     # Custom Python NLU components
├── configs/        # NLU pipeline configurations
├── data/           # Training data (nlu.yml)
├── models/         # Trained model artifacts
├── results/        # Evaluation reports & confusion matrices
├── scripts/        # Benchmarking scripts
└── tests/          # Test suites
```

---

## Getting Started

### 1. Prerequisites

- **Python 3.10**
- [**uv**](https://github.com/astral-sh/uv) (recommended) or standard `pip`

### 2. Setup Virtual Environment

```powershell
# Create virtual environment with Python 3.10
uv venv .venv --python 3.10

# Activate virtual environment (Windows PowerShell)
.venv\Scripts\Activate.ps1
```

> **Note (Linux / macOS):** Activate using `source .venv/bin/activate`.

### 3. Install Dependencies

```powershell
# Install core NLP and Rasa libraries
uv pip install rasa spacy transformers

# Download spaCy medium English language model
python -m spacy download en_core_web_md
```