# Problem 4 — Local Polynomial Regression and Comparison with Classical Methods

## ECM-402: AI for Signal Processing — Lab 1B

---

## Overview

This notebook investigates **Local Polynomial Regression for ECG baseline-wander estimation** and compares its performance with several previously introduced baseline-correction methods.

A global polynomial model uses a single polynomial over the complete ECG segment. While this approach is simple and interpretable, its ability to represent a changing baseline can become limited when the baseline drift varies over time.

Problem 4 addresses this limitation by fitting low-degree polynomial models over local time windows and combining their estimates using overlapping triangular blending.

The notebook is implemented as a **standalone Problem 4 solution**. Only the prerequisite code required from Problems 1–3 is retained.

---

## Objectives

The main objectives of Problem 4 are to:

- Implement local polynomial regression for ECG baseline estimation.
- Estimate the baseline using overlapping local windows.
- Use triangular blending to combine neighboring local estimates.
- Compare local polynomial regression with:
  - Moving-average estimation.
  - Global polynomial regression.
  - Ridge regression.
- Evaluate the corrected ECG using quantitative metrics.
- Investigate performance under a longer segment with faster baseline drift.

---

## Problem Formulation

The observed ECG is modeled as:

\[
x[n] = s[n] + b[n]
\]

where:

- \(s[n]\) is the clean ECG.
- \(b[n]\) is the baseline wander.
- \(x[n]\) is the corrupted ECG.

The objective is to estimate:

\[
\hat{b}[n]
\]

and recover the corrected ECG:

\[
\hat{s}[n]
=
x[n]-\hat{b}[n]
\]

---

# Local Polynomial Regression

Instead of fitting a single polynomial over the complete ECG segment, local polynomial regression divides the time axis into overlapping windows.

For each local window, a polynomial of degree 3 is fitted:

\[
\hat{b}_j(t)
=
\sum_{k=0}^{3}
w_{j,k}t^k
\]

where \(j\) identifies the local window.

The individual local estimates are then combined using triangular cross-fade weights.

---

## Local Window Configuration

The implementation uses:

| Parameter | Value |
|---|---:|
| Window duration | 2.0 s |
| Window overlap | 50% |
| Local polynomial degree | 3 |
| Blending | Triangular weighting |

The 50% overlap allows neighboring local models to contribute to the final baseline estimate.

---

## Why Local Polynomial Regression?

A single global polynomial assumes that one polynomial can adequately describe the baseline over the complete signal.

This assumption can become restrictive when the baseline:

- Changes its local curvature.
- Drifts at different rates.
- Contains multiple low-frequency components.
- Varies over longer recording intervals.

Local polynomial regression provides greater flexibility by allowing the estimated baseline shape to change from one local window to another.

---

# Prerequisite Code Included

Problem 4 depends on several components introduced in earlier problems. To make this notebook independently executable, only the necessary dependencies are included.

| Dependency | Origin | Purpose |
|---|---|---|
| ECG generation/loading | Earlier setup | Provides ECG signal |
| Baseline generation | Earlier setup | Creates reference baseline |
| R-peak detection | Earlier setup | Identifies cardiac cycles |
| TP anchor extraction | Earlier setup | Provides baseline-related observations |
| MSE/SNR | Earlier setup | Evaluates estimation quality |
| Moving average | Problem 1 | Classical comparison |
| Polynomial regression | Problem 2 | Global polynomial comparison |
| `p_star` selection | Problem 2 | Selects global polynomial degree |
| Ridge regression | Problem 3 | Regularized comparison |
| `lambda*_ridge` | Problem 3 | Selects best Ridge model |
| Local polynomial regression | Problem 4 | Main method |

No unnecessary code from the earlier problems is included.

---

# Problem 1 Dependency

The moving-average estimator from Problem 1 is retained only because Problem 4 compares the local polynomial approach with the classical moving-average method.

The moving-average baseline uses a window duration of:

\[
0.6\text{ s}
\]

The corrected signal is calculated as:

\[
\hat{s}_{MA}[n]
=
x[n]-\hat{b}_{MA}[n]
\]

---

# Problem 2 Dependency

Problem 4 uses the global polynomial model selected through the validation procedure from Problem 2.

The candidate polynomial degrees are:

```text
1, 3, 5, 9, 15, 25