# When Compact Encoders Should Abstain

Controlled empirical study of a compact encoder-only Transformer versus BERT for natural language inference, with emphasis on efficiency, calibration, selective prediction, and robustness under distribution shift.

## Overview

This repository contains the final materials for the project **"When Compact Encoders Should Abstain: Selective Prediction and Calibration under Distribution Shift in NLI"**.

The study investigates a practical question: **when can a compact encoder still be useful, and when does reliability degrade enough that the model should abstain rather than make a confident prediction?**

Instead of treating model comparison as a simple leaderboard exercise, the project focuses on the trade-off between:

- predictive quality,
- computational efficiency,
- confidence calibration,
- usefulness of abstention-based decision policies,
- robustness under distribution shift.

The experimental setup compares a compact Transformer encoder trained from scratch against a fine-tuned `bert-base-uncased` baseline.

## Research question

Can a compact encoder remain practically useful in NLI if we explicitly account for uncertainty, calibration, and abstention under out-of-domain conditions?

## Main contributions

- Controlled comparison between a compact encoder-only Transformer and BERT.
- Evaluation on **SNLI** (in-domain) and under distribution shift on **MNLI matched**, **MNLI mismatched**, and **HANS**.
- Analysis of **temperature scaling** for post-hoc calibration.
- Study of **selective prediction** and quality at fixed coverage levels.
- Joint analysis of **efficiency vs. reliability**.

## Main findings

### In-domain performance on SNLI

- **BERT** achieves substantially stronger predictive quality.
- **Compact encoder** remains usable in-domain, but with a clear gap in accuracy and macro-F1.
- After temperature scaling, the compact model becomes reasonably calibrated on SNLI.

### Out-of-domain behavior

- Under shift to **MNLI** and **HANS**, the compact encoder degrades much more severely than BERT.
- Calibration improvements do **not** eliminate the robustness gap.
- Confidence-based abstention helps in-domain, but becomes much less useful under distribution shift.

### Efficiency trade-off

- The compact encoder is approximately **10x smaller** in trainable parameters.
- It is also more than **20x faster** in measured inference throughput.
- This makes the model attractive for efficiency-constrained scenarios, but not as a reliable standalone decision-maker under shift.

## Practical conclusion

Compact encoders can be reasonable candidates for **efficiency-first, in-domain pipelines**, especially when combined with calibration and abstention-aware routing. However, under distribution shift, their confidence estimates become substantially less operationally useful. In such settings, compact models are better treated as:

- a lightweight first-stage filter,
- a routing component in a cascade,
- or a model that escalates uncertain cases to a stronger system or a human reviewer.

## Repository structure

```text
abstract/        EEML-style extended abstract
report/          Full detailed report in Russian
notebooks/       Main Kaggle notebook used for training and evaluation
results/         Exported metric tables and analysis artifacts
figures/         Final plots and diagrams used in the report
assets/          Auxiliary images for documentation
```

## Main artifacts

This repository is intended to contain the final research package:

- **Extended abstract** for EEML submission
- **Full report** with methodology, metrics, tables, discussion, and limitations
- **Final Kaggle notebook** with the full experimental pipeline
- **Figures** for performance, calibration, and selective prediction
- **Exported results** for later analysis and reporting

## Experimental setup

### Models

- **Compact encoder-only Transformer** trained from scratch
- **BERT baseline**: `bert-base-uncased`

### Datasets

- **SNLI** for training / validation / in-domain testing
- **MNLI matched** and **MNLI mismatched** for distribution shift evaluation
- **HANS** for stress-testing heuristic sensitivity

### Calibration

- Post-hoc **temperature scaling** fitted on **SNLI validation**

### Metrics

- Accuracy
- Macro-F1
- Negative log-likelihood (NLL)
- Brier score
- Expected calibration error (ECE)
- AURC
- Quality at fixed coverage levels: `0.70 / 0.80 / 0.90 / 0.95`
- Throughput and latency

## Reproducibility

The experiments were developed in a **Kaggle notebook environment** with GPU acceleration and internet access enabled for dataset/model loading.

The repository is designed to provide the **final research artifacts and reproducible pipeline structure**, while avoiding unnecessary large binary files.

## Notes

- Heavy checkpoints and raw datasets are intentionally omitted.
- Results were generated from the final notebook pipeline and exported into tables/figures for reporting.
- The project is framed as a **controlled empirical study**, not as a proposal of a new model architecture.

## Citation-ready summary

If you need a one-sentence summary for forms, applications, or portfolio descriptions:

> Controlled empirical study of compact encoder reliability in NLI, comparing efficiency, calibration, and abstention behavior against BERT under distribution shift.

## License

Add a license file before public release if needed (`MIT`, `Apache-2.0`, or another appropriate choice).
