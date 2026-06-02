# HF-IQR V3 — Pre-Registered Confirmatory Study of GPT-4o Critique Behavior

**Pre-registration:** [osf.io/yhk98](https://osf.io/yhk98)  
**Researcher:** Billy Davis — Hudson Forge IRMB-C, Lenoir, NC  
**Methods contribution:** James Davis (two-axis validation procedure)  
**Status:** Data collection complete; primary analysis: ceiling-effect null on H1 with full audit chain.

## Study summary

V3 is a pre-registered confirmatory test of whether the GPT-4o critique-validity deficit observed in HF-IQR V2 persists under a forced-counterargument protocol designed to structurally prohibit validation behavior.

**Primary finding:** Under the V3 intervention, all five confirmatory-pool models (Claude Sonnet 4.5, GPT-4o, Gemini 2.5 Pro, DeepSeek Chat, Grok 3) produced Round 2 critiques that satisfied the pre-registered binary CVS rubric on all five items across all 80 questions. Composite CVS was 5/5 for every critique scored by both the machine scorer (Mistral Large 2-2512) and the human validator. H1 is **not confirmed** — the binary rubric saturated under the intervention, producing a ceiling effect that eliminated between-model variance.

The intervention, in the operational sense, eliminated the V2-observed GPT-4o deficit.

## Data structure

```
data/
├── condition_A/        # 400 R1 responses (Condition A: R1 only)
├── condition_B/        # 1998 responses (Condition B: R1-R5)
│   ├── r1_*.json       # Round 1 (400 responses)
│   ├── r2_*.json       # Round 2 forced counterargument (398, 2 logged missing)
│   ├── r3_*.json       # Round 3 defend/revise (400)
│   ├── r4_*.json       # Round 4 mirror self-assessment (400)
│   └── r5_*.json       # Round 5 mechanistic trace (400)
├── cvs_scores/         # Mistral Large 2-2512 CVS scoring of all R2 critiques
├── human_validation/   # 50-critique solo human validation + two-axis result
└── dataset/            # V3 N=80 stratified subset of V2 master
```

## Key numbers

- **Total API calls:** 2,398 across 6 providers
- **Total cost:** ~$13 (data collection) + ~$2 (scoring)
- **Malformed responses (final):** 2 out of 2,398 (0.08%)
- **Models confirmed at snapshot level:**
  - claude-sonnet-4-5-20250929
  - gpt-4o-2024-08-06
  - gemini-2.5-pro (alias)
  - deepseek-v4-flash (alias)
  - grok-4.3 (alias)
  - mistral-large-2512 (scorer)
- **Pre-flight gate:** 10/10 passed
- **Two-axis validation:** Validated (NULL cell, Section 6.6)

## Pre-registration deviations

A separate pilot was not conducted before registered scoring. The first 10 critiques of the human validation subset (by selection index) were retrospectively designated as the pilot subset and used to derive equivalence bounds per Section 6.6 procedure; the remaining 40 critiques served as the validation test subset. This deviation from the filed procedure (which envisioned a separate pre-run pilot) is disclosed transparently and does not alter the pre-registered statistical procedure (TOST for delta-mu, Levene's for delta-sigma) or the four-cell decision surface. The deviation did not affect the outcome — both pilot and test subsets produced degenerate ceiling distributions.

## Secondary analyses (pending)

Per pre-registration Section 7, secondary analyses include:
- Condition A vs B causal contrast
- Three CVS sub-dimensions analyzed independently
- Round 5 revision-trigger category distributions
- Per-category effects across 12 reasoning categories
- Round 1 confidence calibration
- Item-level Position Stability / Correctness Rate

## Citation

If citing this study, please reference the pre-registration: osf.io/yhk98

## License

Data released under CC-BY-4.0. Code under MIT.

Full Force Eternal | Romans 8:28