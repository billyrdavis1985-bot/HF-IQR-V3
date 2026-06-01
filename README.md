# HF-IQR V3: Hudson Forge Intelligence and Reasoning Benchmark — Version 3

[![License](https://img.shields.io/badge/License-Apache%202.0-green)](https://opensource.org/licenses/Apache-2.0)
[![Pre-registered](https://img.shields.io/badge/Pre--registered-OSF%20yhk98-orange)](https://osf.io/yhk98)
[![Python](https://img.shields.io/badge/Python-3.12-blue)](https://www.python.org/)
[![Responses](https://img.shields.io/badge/Responses-2398-purple)](https://github.com/billyrdavis1985-bot/HF-IQR-V3)

**Researcher:** Billy Davis | Independent Researcher | Lenoir, NC
**Version:** 3.0 | **Date:** June 2026
**License:** Apache 2.0
**Pre-registration:** [osf.io/yhk98](https://osf.io/yhk98)
**Methods Contribution:** James Davis (two-axis validation procedure)

---

## Overview

HF-IQR V3 is a pre-registered confirmatory study testing whether the GPT-4o critique-validity deficit observed in HF-IQR V2 persists under a forced-counterargument protocol designed to structurally prohibit validation behavior.

The study evaluates five frontier language models across 80 reasoning questions in 12 categories under two conditions: single-shot response (Condition A) versus full five-round deliberation (Condition B). The Round 2 critique prompt was redesigned to require three structurally independent counterarguments per critique, prohibit validation language, and disallow refusal.

**Primary finding:** Under the V3 intervention, all five reviewers — including GPT-4o — produced critiques that satisfied the binary CVS rubric on all five items across all 80 questions. H1 is **not confirmed** at the primary endpoint: the binary rubric saturated under the intervention, eliminating between-model variance. The intervention closed the V2-observed gap at the rubric level.

**Secondary findings reveal that behavioral signatures still differ sharply between models:** revision-trigger distribution (χ²=43.77, p<10⁻⁶), position stability (Claude 56.5% defending vs others 1.6-12.9%), and calibration heterogeneity (gap range +0.24 to +0.45) all show real and substantial between-model variance under the intervention.

**Framing:** This is a closed pre-registered study with a ceiling-effect null at the primary endpoint and meaningful behavioral findings in the secondary analyses. The intervention efficacy demonstration is the engineering contribution; the persistent behavioral signatures are the scientific contribution.

---

## Dataset

V3 used a stratified seeded sample (seed=80) of the HF-IQR V2 master dataset — 80 questions across 12 reasoning categories. The canonical question bank lives on V2's HuggingFace. V3 reproducibility artifacts (R1–R5 checkpoints, CVS scores, human validation, two-axis result, analysis outputs) are GitHub-hosted in this repository under `data/`.

Dataset SHA-256: `a4d7e612ac6684d32722fb8fb43b6d74639e2f9da4ef64be8bb61ea563ab0b4e`

V2 question bank: <https://huggingface.co/datasets/Billyrdavis1985/hudson-forge-iqr-v2>

V1 question bank: <https://huggingface.co/datasets/Billyrdavis1985/hudson-forge-iqr-benchmark>

---

## Models Evaluated

| Model | Provider | Resolved Snapshot |
| --- | --- | --- |
| claude-sonnet-4-5 | Anthropic | claude-sonnet-4-5-20250929 |
| gpt-4o | OpenAI | gpt-4o-2024-08-06 |
| gemini-2.5-pro | Google | gemini-2.5-pro (alias) |
| deepseek-chat | DeepSeek | deepseek-v4-flash (alias) |
| grok-3 | xAI | grok-4.3 (alias) |
| mistral-large-2 (scorer) | Mistral | mistral-large-2512 |

---

## Protocol — Five Round Deliberation with Forced Counterargument

**Round 1: Independent Response** Each model answered all 80 questions independently using a structured prompt requiring explicit reasoning chain, symbolic logic, confidence score, and final answer.

**Round 2: Forced Counterargument (V3 intervention)** Each model reviewed one peer model's Round 1 response anonymously. The prompt required exactly three structurally independent counterarguments, prohibited validation language, disallowed refusal, and required structured JSON output. Peer assignments were pre-registered and held fixed:
claude   → reviews grok
gpt4o    → reviews claude
gemini   → reviews gpt4o
deepseek → reviews gemini
grok     → reviews deepseek

**Round 3: Defend or Revise** Each model received the peer critique of its own Round 1 response and stated whether it was DEFENDING or REVISING its original position.

**Round 4: Mirror Self-Assessment** Each model compared its own response to ground truth and an anonymous peer response, assessing its own accuracy and confidence calibration.

**Round 5: Mechanistic Trace** Each model produced structured JSON output naming its self-assessment, revision trigger category (logical / social / uncertainty), and rationale.

**Condition A** runs only Round 1, providing a single-shot baseline for the deliberation contrast.

---

## Dataset

80 questions across 12 categories (stratified subset of HF-IQR V2 master dataset, seed=80):

| Category | Questions |
| --- | --- |
| adversarial | 8 |
| logical_syllogism | 8 |
| causal_chain | 8 |
| probabilistic | 8 |
| mathematical_proof | 8 |
| counterfactual | 8 |
| quantum_reasoning | 6 |
| frontier_reasoning | 6 |
| meta_reasoning | 6 |
| ethical_dilemma | 6 |
| temporal_reasoning | 4 |
| spatial_reasoning | 4 |

Verifiable binary ground truth available for 9 categories used in correctness scoring.

Dataset SHA-256: `a4d7e612ac6684d32722fb8fb43b6d74639e2f9da4ef64be8bb61ea563ab0b4e`

---

## Scoring Metrics

**Critique Validity Score (CVS)** Quality of peer critique across five binary dimensions, V2 weights retained:

- error_identified (0.25)
- root_cause (0.20)
- step_cited (0.20)
- alternative_provided (0.20)
- no_ad_hominem (0.15)

**Scoring architecture:** Model-primary with human-validated subset. Mistral Large 2 (mistral-large-2512) scored all 398 critiques. A pre-registered stratified subset of 50 critiques was scored by a solo human rater (researcher). Equivalence between machine and human scoring was tested via two-axis TOST/Levene validation (Section 6.6 procedure, James Davis methods contribution).

**Position Stability (PS)** Did the model defend or revise in Round 3? Extracted via deterministic pattern matching from R3 free-text output. UNKNOWN rate: 0.0%.

**Revision Trigger Category** Logical / Social / Uncertainty, declared directly by the model in Round 5 structured JSON.

**Confidence Calibration** Brier score and Expected Calibration Error (ECE, 10 equal-width bins) computed on verifiable categories using R1 stated confidence and heuristic correctness.

---

## Key Findings

### Finding 1 — H1 Ceiling-Effect Null (Primary Endpoint)

All 398 Round 2 critiques scored 5/5 on the composite CVS by both Mistral Large 2 and the human validator. Per-model composite means: claude 5.000, gpt4o 5.000, gemini 5.000, deepseek 5.000, grok 5.000. Per-item rates 1.000 across all five items, all five reviewers. **H1 is not confirmed:** the binary rubric saturated under the V3 intervention, eliminating the between-model variance needed for the contrast.

This is an intervention efficacy demonstration: the forced-counterargument protocol drove all five reviewers, including GPT-4o, to produce structurally adequate critiques. The V2-observed deficit is prompt-regime-dependent, not a fixed model property.

### Finding 2 — Revision Trigger Distribution (Strongest Secondary)

Chi-square test of model × revision-trigger independence: **χ²=43.77, dof=8, p=6.3×10⁻⁶**.

| Model | Logical | Social | Uncertainty |
| --- | --- | --- | --- |
| claude | 97.5% | 1.2% | 1.2% |
| deepseek | 81.2% | 0.0% | 18.8% |
| gemini | 95.0% | 2.5% | 2.5% |
| gpt4o | 76.2% | 0.0% | 23.8% |
| grok | 96.2% | 0.0% | 3.8% |

GPT-4o and DeepSeek attribute revisions to uncertainty at 5-20× the rate of the other three reviewers. This behavioral signature survived the intervention.

### Finding 3 — Position Stability × Model

Restricted to verifiable categories (n=310):

| Model | N | PS Rate (Defending) | Correctness Rate |
| --- | --- | --- | --- |
| claude | 62 | 0.565 | 0.565 |
| deepseek | 62 | 0.129 | 0.597 |
| gpt4o | 62 | 0.016 | 0.565 |
| gemini | 62 | 0.048 | 0.532 |
| grok | 62 | 0.081 | 0.581 |

Claude defends its R1 position 56.5% of the time. The other four defend 1.6-12.9%. Per-category chi-square tests show the model effect is significant in adversarial, logical_syllogism, mathematical_proof, quantum_reasoning, and temporal_reasoning.

### Finding 4 — Position Stability × Correctness Dissociation

Logistic regression `Correctness ~ PositionStability + Model + Category` returns position_stable coefficient β=0.237, odds ratio 1.27, **p=0.63**. Defending position does not predict being correct. This confirms the V2-observed dissociation under V3's stricter measurement.

### Finding 5 — Confidence Calibration

All five models are overconfident. Calibration quality varies substantially:

| Model | N | Mean Confidence | Mean Accuracy | Gap | Brier | ECE |
| --- | --- | --- | --- | --- | --- | --- |
| grok | 60 | 0.823 | 0.583 | +0.239 | 0.261 | 0.239 |
| deepseek | 62 | 0.964 | 0.597 | +0.367 | 0.341 | 0.367 |
| gpt4o | 62 | 0.926 | 0.565 | +0.361 | 0.358 | 0.377 |
| claude | 62 | 0.937 | 0.565 | +0.373 | 0.370 | 0.386 |
| gemini | 62 | 0.984 | 0.532 | +0.452 | 0.445 | 0.457 |

Gemini stated near-certainty (98.4% mean confidence) while being correct only 53.2% of the time. Practitioners deploying these models should not trust stated confidence levels at face value.

### Finding 6 — Deliberation Effect

Condition A Round 1 versus Condition B Round 3 correctness shift: per-model Δ ranging from −0.016 to 0.000. Pooled Wilcoxon p=0.84. The five-round deliberation protocol did not change correctness within the resolution of the heuristic correctness measure.

---

## Documented Limitations

**Ceiling Effect on Primary Endpoint** The binary CVS rubric saturated under the V3 intervention, producing zero variance across all 398 critiques. Future replications should use a graded rubric (0-5 per item) to preserve discrimination under strong interventions. The ceiling effect is reported as a finding (intervention efficacy) rather than a measurement failure.

**Solo Human Validator** Per pre-registered Section 5.3 solo-rater branch, human validation was performed by a single expert rater (the researcher). 50 critiques scored across all five reviewers, all scored 5/5 — concordant with Mistral's machine scoring at the ceiling.

**Heuristic Correctness Scoring** Round 1 correctness was scored via string and numeric matching against ground_truth in the response's final-answer section. This heuristic breaks on mathematical_proof category (proofs contain many numbers, causing quasi-separation in the logistic regression). PS/CR and calibration results are robust outside math_proof; math_proof results are flagged in the per-category analysis.

**Pre-registration Deviation — Pilot Subset** A separate pilot was not conducted before registered scoring. The first 10 critiques of the human validation subset (by selection index) were retrospectively designated as the pilot subset and used to derive equivalence bounds for the two-axis validation per Section 6.6 procedure. The remaining 40 critiques served as the validation test subset. Disclosed transparently. Did not affect outcome (degenerate ceiling distributions in both pilot and test).

**Model Version Aliasing** Three of five confirmatory-pool models served via aliases at the API layer (gemini-2.5-pro, deepseek-v4-flash, grok-4.3) rather than dated snapshots. Two models (claude-sonnet-4-5-20250929, gpt-4o-2024-08-06) and the scorer (mistral-large-2512) captured at dated-snapshot level. Reproducibility depends on the providers maintaining stable behavior under these aliases.

**Sample Size** N=80 questions, powered for d=0.55 contrast at α=0.05 and power=0.80 with a 1.5× design-effect adjustment. The primary endpoint saturated before the contrast could be tested; sample size is not the limiting factor on the H1 null.

**Mixed-Effects Approximation** The pre-registered PS/CR mixed-effects model with item random effect was approximated by fixed-effects logistic regression because Python's mixed-effects logistic implementations are limited. Documented as implementation choice.

---

## Infrastructure

| Component | Detail |
| --- | --- |
| Execution | Google Colab |
| Storage | Google Drive sovereign |
| Local compute | Hudson Forge IRMB-C |
| GPU | RTX 5070 |
| Local LLM server | NucBox M6 Ultra via Ollama |
| Scorer | mistral-large-2512 (Mistral API) |
| Total responses | 2,398 |
| Total API cost | ~$15 (data collection + scoring) |
| Pre-flight gate | 10/10 passed |
| Malformed (final) | 2/2,398 (0.08%) |

---

## Repository Structure

HF-IQR-V3/
├── HF_IQR_V3_Council_Run.ipynb       # Full executable notebook
├── HF_IQR_V3_Preregistration.pdf     # Filed preregistration
├── HF_IQR_V3_CVS_Rubric_Supplement.pdf
├── LICENSE                            # Apache 2.0
├── MANIFEST.json                      # SHA-256 hashes for all data files
├── README.md
└── data/
├── dataset/
│   ├── HF_IQR_V3_Dataset_80.json
│   └── HF_IQR_Master_Dataset_v2.json
├── condition_A/
│   └── r1_<category>.json        # 12 category files, 400 responses
├── condition_B/
│   ├── r1_<category>.json        # 400 responses
│   ├── r2_<category>.json        # 398 critiques (2 malformed logged)
│   ├── r3_<category>.json        # 400 defend/revise (with position field)
│   ├── r4_<category>.json        # 400 mirror self-assessment
│   └── r5_<category>.json        # 400 mechanistic trace (structured JSON)
├── cvs_scores/
│   └── cvs_scores.json           # 398 Mistral-scored critiques
├── human_validation/
│   ├── human_validation_worksheet.md
│   ├── human_validation_worksheet.json
│   ├── human_validation_scores.json
│   ├── selection_log.json
│   └── two_axis_validation_result.json
└── analyses/
├── r5_revision_trigger_analysis.json
├── ps_cr_analysis.json
├── r1_calibration_analysis.json
├── per_category_analysis.json
├── condition_a_vs_b_analysis.json
└── cvs_subdimensions_analysis.json

---

## Pre-registration Integrity

Pre-registration filed: [osf.io/yhk98](https://osf.io/yhk98)
Dataset SHA-256: `a4d7e612ac6684d32722fb8fb43b6d74639e2f9da4ef64be8bb61ea563ab0b4e`
Human validation seed: 50
Scorer: mistral-large-2512 (filed model served directly — no substitution)

All six pre-registered secondary analyses completed and reported.
One procedural deviation disclosed (retrospective pilot designation, Section 6.6).

---

## V1 / V2 Reference

HF-IQR V1 established the multi-round deliberation architecture across 60 questions and 6 categories.
HF-IQR V2 expanded to 200 questions and 12 categories with the GPT-4o critique-validity finding that motivated V3.
HF-IQR V3 tests whether that V2 finding persists under a structured intervention.

V2 GitHub: <https://github.com/billyrdavis1985-bot/HF-IQR-V2-Hudson-Forge-Intelligence-and-Reasoning-Benchmark>
V1 GitHub: <https://github.com/billyrdavis1985-bot/-IRMB_HF-IQR_ReasoningBenchmark>

---

## Citation

Davis, B. (2026). HF-IQR V3: Hudson Forge Intelligence and Reasoning Benchmark Version 3 — A Pre-Registered Confirmatory Study of Forced-Counterargument Critique in Frontier LLMs. Independent research. Hudson Forge IRMB-C, Lenoir NC. Pre-registration: osf.io/yhk98

Methods contribution: James Davis (two-axis validation procedure).
