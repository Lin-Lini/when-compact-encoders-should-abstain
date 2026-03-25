# When Compact Encoders Should Abstain

> Controlled empirical study of a compact encoder-only Transformer versus BERT for natural language inference, focusing on efficiency, calibration, selective prediction, and robustness under distribution shift.

## Overview

This repository contains the final research package for the project **“When Compact Encoders Should Abstain: Selective Prediction and Calibration under Distribution Shift in NLI.”**

The project asks a practical question that goes beyond simple leaderboard comparison:

> **Can a compact encoder remain operationally useful in NLI when we explicitly account for calibration, selective prediction, and distribution shift?**

Instead of treating compact models as either “good enough” or “obviously weaker,” this study examines where they remain useful, where they fail, and when they should defer to a stronger system or a human reviewer.

---

## Research focus

The study compares:

- a **compact encoder-only Transformer trained from scratch**
- a **fine-tuned `bert-base-uncased` baseline**

under a shared evaluation framework designed to measure not only predictive quality, but also:

- computational efficiency
- probabilistic calibration
- abstention behavior
- robustness under out-of-domain shift
- operational usefulness under constrained coverage

---

## What this repository contains

This repo is organized as a **research artifact**, not as a polished Python package.

It includes:

- an **EEML-style extended abstract**
- a **full report** with methodology, results, limitations, and discussion
- a **single main Kaggle notebook** implementing the full pipeline
- **experiment configuration**
- **exported result tables**
- **figures and reporting artifacts**
- **model checkpoints layout** for both compared systems

---

## Experimental setup

### Models
- **Compact encoder-only Transformer** implemented in PyTorch
- **BERT baseline**: `bert-base-uncased`

### Datasets
- **SNLI** for training and in-domain evaluation
- **MNLI matched** for near-shift evaluation
- **MNLI mismatched** for genre shift evaluation
- **HANS** for diagnostic stress testing

### Evaluation dimensions
- **Predictive quality**: accuracy, macro-F1, per-class F1
- **Probabilistic quality**: NLL, Brier score, ECE, MCE
- **Selective prediction**: coverage-based evaluation, AURC, quality at fixed coverage
- **Efficiency**: trainable parameters, throughput, batch latency
- **Slice analysis**: heuristic and structural subsets

### Calibration
- Post-hoc **temperature scaling**

### Multi-run protocol
- shared tokenizer family
- repeated runs across multiple seeds
- exported summary tables with mean/std statistics

---

## Key results at a glance

### Efficiency

| Model | Trainable parameters | Throughput (items/sec) | Mean batch latency |
|---|---:|---:|---:|
| Compact encoder | 11.0M | 7085.5 | 0.0090 s |
| BERT-base | 109.5M | 309.3 | 0.2070 s |

The compact encoder is roughly **10× smaller** and **22.9× faster** in measured inference throughput.

### Predictive performance snapshot

| Split | Compact encoder | BERT-base |
|---|---:|---:|
| SNLI test accuracy | 75.1% | 90.5% |
| MNLI matched accuracy | 44.0% | 74.4% |
| MNLI mismatched accuracy | 44.7% | 74.2% |
| HANS accuracy | 49.4% | 58.7% |

### Calibration snapshot on SNLI

After temperature scaling:

- compact encoder ECE improves from **0.0577 → 0.0274**
- BERT ECE improves from **0.0437 → 0.0251**

This makes the compact model more usable **in-domain**, but calibration does **not** remove the broader robustness gap under distribution shift.

---

## Main takeaways

1. **The compact encoder is meaningfully efficient, but not robust enough to replace BERT as a standalone decision-maker under shift.**

2. **In-domain, the compact model remains usable**, especially when calibration and abstention are part of the decision pipeline.

3. **Out-of-domain, the compact model degrades much more severely**, especially on MNLI and HANS.

4. **Abstention helps more in-domain than under shift.**
   Confidence filtering improves practical quality, but it does not fully rescue the compact encoder once the distribution changes.

5. **The most realistic operational role for a compact encoder is not “small BERT,” but a front-stage component**:
   - lightweight first-pass filter
   - routing component in a cascade
   - triage model that escalates uncertain cases

---

## Repository layout

```text
abstract/
  When_Compact_Encoders_Should_Abstain_EEML_Extended_Abstract.pdf
  When_Compact_Encoders_Should_Abstain_EEML_Extended_Abstract.docx

checkpoints/
  bert/
  compact/

configs/
  experiment_config.json

figures/

notebooks/
  when-compact-encoders-should-abstain.ipynb

report/
  final_report_manifest.json
  Отчет_Compact_Encoders_NLI.docx

tables/
  benchmarks.csv
  dataset_sizes.csv
  metrics_all.csv
  ood_deltas.csv
  practical_quality_table.csv
  quality_at_fixed_coverage.csv
  slice_metrics.csv
  summary_bench_mean_std.csv
  summary_hans_mean_std.csv
  summary_multiclass_mean_std.csv
  summary_quality_at_coverage_mean_std.csv
  training_registry.csv
