# CHOP Trio De Novo Variant Calling — Take-Home Assignment

This repository contains my end-to-end solution for identifying **strictly de novo variants** in a parent–child trio and then filtering/scoring them to produce a **high-confidence candidate set** and a **Top-10 prioritized list**. :contentReference[oaicite:0]{index=0}

---

## What’s included

### Task 1 — Strict de novo discovery
- Script/Notebook: `CHOP_test.ipynb`
- Output: `CDL-068-99.strict_denovo.tsv`
- Result: **343** strictly de novo candidates (proband het; parents hom-ref). :contentReference[oaicite:1]{index=1}

### Task 2 — Quality filtering + rationale
Filtering strategy aims to remove sequencing artifacts, mapping errors, and genotype misclassifications while retaining biologically plausible heterozygous de novo events in the proband. :contentReference[oaicite:2]{index=2}

Key filtering rules (high level):
- **Genotypes**: proband `0/1`; both parents `0/0`
- **Variant-level**: `QUAL ≥ 30`, `FILTER = PASS`
- **VQSR (when present)**: retain `VQSLOD > 0`
- **INFO metrics**: `QD ≥ 2.0`, `FS ≤ 60`, `SOR ≤ 3`, `MQ ≥ 40`, `MQRankSum ≥ −12.5`, `ReadPosRankSum ≥ −8`
- **Trio genotype confidence**: `DP ≥ 15`, `GQ ≥ 30` for all trio members
- **Proband balance**: `ALT reads ≥ 8`, `VAF 0.35–0.65`
- **Parents absence**: `ALT reads ≤ 0`, `VAF ≤ 0.01` :contentReference[oaicite:3]{index=3}

- Filtered output: `CDL-068-99.filtered_denovo.tsv`
- Result: **88** candidates (**80 SNVs**, **8 INDELs**). :contentReference[oaicite:4]{index=4}

### Task 3 — Expected true positives
- Expectation: **~80–85 true positives out of 88**
- Rationale: aligns with typical per-child de novo SNV rates in trio WGS studies; residual false positives likely among INDELs or difficult genomic contexts. :contentReference[oaicite:5]{index=5}

### Task 3 (extra) — Top-10 “true positive likelihood” scoring
- Notebook: `Top10 true positives.ipynb`
- Approach: composite score integrating:
  - Variant confidence: `QUAL`, `VQSLOD`, `QD`
  - Mapping integrity: `MQ`, `FS`, `SOR`, `MQRankSum`, `ReadPosRankSum`
  - Trio genotype / balance: `VAF`, `DP`, `GQ` across trio
- Output: Top-10 table (CHROM, POS, REF, ALT, TYPE, PROBAND_VAF, TP_SCORE). :contentReference[oaicite:6]{index=6}

---

## Repository structure (suggested)

```text
.
├── notebooks/
│   ├── CHOP_test.ipynb
│   └── Top10 true positives.ipynb
├── outputs/
│   ├── CDL-068-99.strict_denovo.tsv
│   └── CDL-068-99.filtered_denovo.tsv
├── docs/
│   └── CHOP_ans_sheet.pdf
└── README.md
