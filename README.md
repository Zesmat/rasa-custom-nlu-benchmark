# Rasa Custom NLU & Pipeline Benchmark Harness

[![Python 3.10](https://img.shields.io/badge/python-3.10-blue.svg)](https://www.python.org/downloads/release/python-3100/)
[![Rasa Open Source](https://img.shields.io/badge/rasa-3.6+-purple.svg)](https://rasa.com/)

An empirical benchmarking project comparing Rasa NLU pipeline architectures: a lightweight, CPU-efficient standard DIET pipeline vs. a pre-trained Transformer (DistilBERT) and spaCy hybrid pipeline.

---

## 🎯 Project Goal

To build an evaluation harness that tests and benchmarks different Rasa NLU architectures on the same domain dataset, measuring:
- **Intent classification accuracy & F1-scores**
- **Entity extraction precision, recall, and F1-scores**
- **Training latency, model size, and inference speed**

---

## 🛠️ Tech Stack

- **Framework:** Rasa Open Source (v3.6+)
- **NLP / Embeddings:** spaCy (`en_core_web_md`), Hugging Face Transformers (`distilbert-base-uncased`)
- **Language:** Python 3.10
- **Package Management:** `uv`

---

## 📂 Project Structure

```text
├── components/     # Custom Python NLU components (Day 4)
├── configs/        # NLU pipeline configurations (Day 3)
│   ├── config-standard.yml       # Bag-of-words + Subword N-grams + DIET
│   └── config-transformer.yml    # spaCy + DistilBERT + Subword N-grams + DIET
├── data/           # Training & test data
│   └── nlu.yml                   # Banking domain dataset (10 intents, 6 entities)
├── models/         # Trained model artifacts
├── results/        # Evaluation reports & confusion matrices (Day 5)
├── scripts/        # Benchmarking & comparison scripts (Day 6)
├── tests/          # Test suites
├── domain.yml      # Assistant domain specification
└── config.yml      # Root configuration baseline
```

---

## 🔬 Benchmark Pipeline Architectures

| Component Stage | Baseline Pipeline (`config-standard.yml`) | Hybrid Transformer Pipeline (`config-transformer.yml`) |
| :--- | :--- | :--- |
| **Language Pipeline** | — | `SpacyNLP` (`en_core_web_md`) |
| **Tokenizer** | `WhitespaceTokenizer` | `SpacyTokenizer` |
| **Dense Featurizers** | — | `SpacyFeaturizer` (300-d GloVe), `LanguageModelFeaturizer` (`distilbert-base-uncased`) |
| **Sparse Featurizers** | `RegexFeaturizer`, `LexicalSyntacticFeaturizer`, `CountVectorsFeaturizer` (word), `CountVectorsFeaturizer` (char_wb 1-4) | `RegexFeaturizer`, `LexicalSyntacticFeaturizer`, `CountVectorsFeaturizer` (char_wb 1-4) |
| **Intent & Entity Classifier** | `DIETClassifier` (100 epochs, softmax confidence) | `DIETClassifier` (100 epochs, combined dense + sparse features) |
| **Entity Extractors** | `DIETClassifier` | `DIETClassifier` + `SpacyEntityExtractor` (`PERSON`, `DATE`, `MONEY`) |
| **Entity Normalization** | `EntitySynonymMapper` | `EntitySynonymMapper` |
| **Fallback Handling** | `FallbackClassifier` (threshold: 0.3) | `FallbackClassifier` (threshold: 0.3) |

---

## 🚀 Getting Started

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
# Install all pinned dependencies and spaCy language model
uv pip install -r requirements.txt
```

### 4. Validate Dataset & Pipelines

```powershell
# Validate data with the baseline standard pipeline
rasa data validate --config configs/config-standard.yml

# Validate data with the hybrid transformer pipeline
rasa data validate --config configs/config-transformer.yml
```

---

## 📅 Daily Progress Log

- [x] **Day 1:** Project initialized, Python 3.10 virtual environment configured with `uv`, core dependencies (`rasa`, `spacy`, `transformers`) installed, and directory structure scaffolded.
- [x] **Day 2:** Domain dataset engineered (`data/nlu.yml`) with 10 intents, 6 entities, synonyms, regex patterns, and validated with `domain.yml`.
- [x] **Day 3:** Configured baseline (`configs/config-standard.yml`) and pre-trained transformer (`configs/config-transformer.yml`) NLU pipelines.
- [ ] **Day 4:** Implement custom Python NLU graph component.
- [ ] **Day 5:** Run cross-validation evaluations (`rasa test nlu`).
- [ ] **Day 6:** Write benchmark comparison script and generate Markdown tables.
- [ ] **Day 7:** Final review and GitHub release tag (`v1.0.0`).
