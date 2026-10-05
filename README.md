# Learning Generalizable REST API Test Oracles

This repository contains the dataset, fine-tuning workflows, model adapter weights, and benchmark evaluation code for my MS thesis on **Learning Generalizable REST API Test Oracles**.

The research investigates whether a compact Large Language Model (Llama-3-8B fine-tuned via QLoRA) can learn generalizable semantic compliance rules to detect REST API bugs across unseen domains, both with and without OpenAPI specifications.

---

## Overview

Automated API testing often struggles with the **test oracle problem**: deciding whether an API response is actually valid or buggy without relying on fragile hard-coded assertions.

This project formulates API oracle evaluation as a sequence classification and reasoning task. We evaluate two experimental tracks:
1. **`specs/` (Schema-Guided)**: Oracle receives the API request, response, and relevant OpenAPI schema snippet.
2. **`nospecs/` (Pure Semantic)**: Oracle receives only the API request and response, inferring compliance semantically.

---

## Dataset & Bug Taxonomy

The dataset consists of real API interactions and systematically mutated responses covering **7 bug classes**:
* `correct` (Valid HTTP 200 / PASS)
* `missing_required_field`
* `wrong_data_type`
* `wrong_status_code`
* `enum_violation`
* `constraint_violation`
* `undeclared_field`

### Split & Generalization Setup
To test true cross-domain generalization, the test set strictly evaluates on **held-out APIs never seen during training**:

* **Training Set (1,508 records)** & **Validation Set (377 records)**:
  * APIs: GitHub, JSONPlaceholder, Petstore, Rick and Morty, Open-Meteo, OpenTDB, ReqRes, TaskAPI.
* **Test Set (Held-Out APIs)**:
  * Unseen APIs: SpaceX, Frankfurter, OpenLibrary, TVMaze.

---

## Repository Structure

```text
├── specs/                                # Schema-guided track (with OpenAPI snippets)
│   ├── train_with_spec.jsonl             # Training set with relevant schema snippets
│   ├── test_with_spec.jsonl              # Test set on unseen APIs with spec snippets
│   ├── qlora_finetune_with_spec.ipynb    # QLoRA fine-tuning notebook (Unsloth + Llama 3 8B)
│   ├── phase_c_with_spec.ipynb           # Benchmark evaluation notebook
│   ├── api-oracle-lora-adapter.zip       # Saved LoRA adapter weights
│   ├── evaluation_results_with_spec.json # Quantitative evaluation output
│   └── *.png                             # Loss curves, confusion matrices, and F1 plots
│
└── nospecs/                              # Spec-free semantic track
    ├── train.jsonl                       # Training set without schemas
    ├── val.jsonl                         # Validation set
    ├── test.jsonl                        # Test set on unseen APIs
    ├── qlora_finetune_002.ipynb          # Fine-tuning pipeline without specs
    ├── phase_c_fixed - 002.ipynb         # Evaluation pipeline without specs
    ├── api-oracle-lora-adapter.zip       # Saved LoRA adapter weights
    └── evaluation_results.json           # Quantitative evaluation output
```

---

## Key Experimental Results

Models compared across held-out unseen APIs:
* **Model A (Baseline)**: Zero-shot `Meta-Llama-3-8B-Instruct`
* **Model B (Fine-Tuned)**: `Llama-3-8B-Instruct` + QLoRA adapter
* **Model C (Frontier Reference)**: Claude 3.5 Sonnet

### Summary Performance

| Track | Model | Precision | Recall | F1 Score | Accuracy |
|---|---|:---:|:---:|:---:|:---:|
| **With Specs** | Baseline (Llama-3-8B) | 0.938 | 0.395 | 0.556 | 46.9% |
| | **Fine-Tuned (Ours)** | **0.965** | **0.579** | **0.724** (+0.168) | **62.8%** |
| | Reference (Claude) | 0.864 | 1.000 | 0.927 | 86.7% |
| **Without Specs** | Baseline (Llama-3-8B) | 0.926 | 0.263 | 0.410 | 36.3% |
| | **Fine-Tuned (Ours)** | **0.932** | **0.289** | **0.442** (+0.032) | **38.5%** |
| | Reference (Claude) | 0.844 | 1.000 | 0.916 | 84.5% |

### Core Takeaways
1. **Fine-Tuning Efficacy**: Fine-tuning substantially increases detection capability over the base model, yielding a **+0.168 F1 improvement** in schema-guided compliance.
2. **Cross-Domain Generalization**: High precision across all unseen APIs confirms that the model learns general API contract principles rather than memorizing domain-specific endpoints.
3. **Spec Value**: Providing even small, relevant schema snippets drastically improves both recall and overall F1 (+0.282 F1 over the spec-free model).

---

## Quick Start / Reproducing

### 1. Fine-Tuning
Open `specs/qlora_finetune_with_spec.ipynb` or `nospecs/qlora_finetune_002.ipynb` in Google Colab (runs comfortably on a free T4 GPU):
1. Set runtime to **GPU (T4)**.
2. Upload the corresponding `train*.jsonl` and `val.jsonl` files.
3. Run through the notebook to produce the LoRA adapter weights.

### 2. Evaluation
Open `phase_c_*.ipynb` in Colab or Jupyter:
1. Ensure the fine-tuned adapter zip and test JSONL files are available in your working directory.
2. Run the evaluation script to benchmark against the baseline and generate confusion matrices and per-bug F1 figures.

---

## Citation & Author

* **Author**: Haris Umar
* **Degree**: MS Computer Science / Software Engineering Thesis
* **Repository**: [Learning-Generalizable-REST-API-Oracles](https://github.com/hukdeveloper/Learning-Generalizable-REST-API-Oracles)
