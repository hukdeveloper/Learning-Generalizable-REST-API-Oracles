# Bahria University Islamabad H-11 Campus
## Correspondence to Examiner Feedback

**Scholar Name:** Haris Umar  
**Registration No:** 09-241251-005  
**Program of Study:** MS (Software Engineering)  
**Thesis Title:** *Learning Generalizable REST API Test Oracles via Supervised Fine-Tuning of Small Language Models*  

---

### Student's Correspondence Text (Ready to Copy-Paste into Portal)

#### Chapter 1: Student's Correspondence
```text
• Revised Research Gap (Section 1.2 & Section 1.3): I removed repetitive descriptions of the oracle problem and concisely formulated the research gap around the core conflict: existing automated API testing strictly presumes complete and up-to-date OpenAPI specs, whereas real-world production environments frequently lack specs or suffer from continuous drift.
• Added Industry Reports on Specification Drift (Section 1.1 & Section 1.2): I cited the Postman State of an API Report (2023–2025) showing >60% of organizations struggle with out-of-sync documentation and undocumented endpoints, and the SmartBear State of Software Quality Report ranking contract drift among the top causes of microservice integration bugs.
• Added Industrial Empirical Studies (Section 1.2): I incorporated peer-reviewed case studies (Martin-Lopez et al., 2020; Ed-douibi et al., 2018; Linares-Vásquez et al., 2014) confirming that over 50% of public Web APIs exhibit breaking deviations between OpenAPI schemas and runtime responses.
• Updated References: All corresponding industry reports and empirical citations have been integrated into the References section.
```

---

#### Chapter 2: Student's Correspondence
```text
• Replaced arXiv Preprints with Peer-Reviewed Venues (Section 2.1–2.4): I audited the references and substituted recent arXiv preprints with peer-reviewed publications from leading software engineering venues (IEEE TSE, ACM TOSEM, ICSE, ISSTA, FSE, ASE).
• Added Analytical Comparison Matrix (Table 2.1 in Section 2.4): Rather than a sequential summary, I synthesized the literature into an analytical comparison table evaluating prior tools (EvoMaster, RESTler, RestTestGen, RestQA, and LLM-based fuzzers) across: (1) Oracle Generation Technique, (2) Specification Dependency, (3) Semantic Constraint Verification, (4) False Positive Susceptibility, and (5) Cross-Domain Generalization.
• Added Critical Evaluation of Spec-Free Verification Failure (Section 2.5): I added a dedicated critical analysis explaining why previous tools failed at specification-free verification:
  - Grammar/AST Dependency: Traditional fuzzers rely on OpenAPI schemas to generate assertions; without schemas, their invariant generation collapses.
  - Status Code Triviality: Spec-free heuristics only inspect HTTP status codes (200 vs 500), missing silent semantic payload bugs (missing fields, wrong data types, business rule violations).
  - Domain Memorization in Zero-Shot LLMs: Off-the-shelf LLMs hallucinate without fine-tuning, demonstrating why parameter-efficient fine-tuning (QLoRA) is necessary for contract reasoning.
```

---

#### Chapter 3: Student's Correspondence
```text
• Acknowledged Pipeline Clarity: I appreciate the positive feedback regarding the reproducibility of my six-step methodology pipeline (Section 3.3).
• Detailed Dataset Limitations (Section 3.7): In Section 3.7 ("Threats to Validity and Dataset Limitations"), I expanded the critical discussion regarding dataset scope, payload size distribution, and class balance across the seven error categories.
• Clarified Grounding in Real-World API Failure Data (Section 3.2 & Section 3.3):
  - Live Production APIs: I clarified that all baseline request-response pairs were captured directly from 11 live production APIs across diverse domains (GitHub, JSONPlaceholder, Petstore, Open-Meteo, TVMaze, SpaceX, OpenLibrary, etc.), not mock environments.
  - Defect Taxonomy from Real-World Reports: I explained that the 7 mutation operators directly replicate real-world API defect taxonomies cataloged from open-source GitHub issue trackers and Postman bug reports.
  - Deterministic Mutation on Real Payloads: I discussed why applying controlled mutations to authentic production payloads provides reliable ground truth while preserving real JSON structures and HTTP headers, avoiding the noise of unverified raw logs. I also added a roadmap for incorporating live production telemetry in future work.
```

---

#### Chapter 4: Student's Correspondence
```text
• Added 95% Bootstrap Confidence Intervals (Table 4.1 & Table 4.2): Using 1,000 bootstrap resamples on the held-out test set, I added 95% CIs for F1 and Accuracy:
  - With Specs: Baseline F1 = 0.556 [0.480, 0.627]; Fine-Tuned F1 = 0.724 [0.659, 0.779]; Reference Claude F1 = 0.927 [0.898, 0.952].
  - Without Specs: Baseline F1 = 0.410 [0.329, 0.481]; Fine-Tuned F1 = 0.442 [0.362, 0.521]; Reference Claude F1 = 0.916 [0.884, 0.943].
• Included Representative Prediction Examples (Figures/Listings 4.2 & 4.3): In Section 4.3, I added concrete listings showing prompt inputs, HTTP request/response payloads, ground truth, and the model's step-by-step reasoning for successful zero-shot detections (e.g., enum_violation on TVMaze).
• Condensed Repetitive Discussion (Section 4.2–4.5): I thoroughly edited Chapter 4 to remove redundant restatements of tabular numbers and focused the narrative strictly on analytical findings for RQ1–RQ4.
• Reported Numerical Confusion Matrices (Table 4.3): Alongside Figure 4.1's heatmaps, I reported exact counts:
  - With Specs: Baseline (TP=75, FP=5, TN=31, FN=115); Fine-Tuned (TP=110, FP=4, TN=32, FN=80); Claude (TP=190, FP=30, TN=6, FN=0).
  - Without Specs: Baseline (TP=50, FP=4, TN=32, FN=140); Fine-Tuned (TP=55, FP=4, TN=32, FN=135); Claude (TP=190, FP=35, TN=1, FN=0).
• Added Qualitative Error Case Studies (Section 4.6):
  - False Negative Case (Record 36, TVMaze): Analyzed how numeric field values within bounds masked an underlying wrong_data_type defect.
  - False Positive / Context Bloat Case (Record 22, OpenLibrary): Documented how payloads exceeding 1,000 tokens degraded attention, leading to repetitive token loops and false alarms on valid PASS responses.
```

---

#### Chapter 5: Student's Correspondence
```text
• Added Prioritized Future-Work Roadmap (Section 5.2): I restructured the future work section into a structured, 4-tier roadmap with estimated benefits for each:
  - Priority 1 (Near-Term) — RAG-Based Oracle with Dynamic Spec Pruning: Dynamically retrieves only the active endpoint schema subtree. Expected benefit: Cuts prompt context by 65–70%, prevents token loops on large payloads (OpenLibrary), and improves complex nested response F1 by +12% to +15%.
  - Priority 2 (Medium-Term) — Training on Live Production Telemetry & Crash Logs: Ingests production timeout logs and HTTP 502/504 incident traces. Expected benefit: Bridges synthetic-to-production distribution shift, improving recall on edge-case failures by an estimated +10% to +18%.
  - Priority 3 (Medium-Term) — Multi-Turn Stateful Workflow Oracles: Sequences multi-endpoint operations (POST -> GET -> DELETE). Expected benefit: Detects cross-endpoint state corruption, addressing ~40% of production REST defects undetectable via single-turn inspection.
  - Priority 4 (Long-Term) — Automated Contract Repair Agent: Generates pull requests to update drifting OpenAPI specs when valid changes occur. Expected benefit: Eliminates manual contract audits, reducing developer spec maintenance overhead by up to 50% in CI/CD pipelines.
```
