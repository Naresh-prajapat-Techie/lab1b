# Problem 5 — Generalization to Real ECG Recordings

## ECM-402: AI for Signal Processing — Lab 1B

---

## Overview

This notebook investigates the **generalization of Ridge-regression-based ECG baseline estimation to a new real-like ECG recording**.

The preceding experiments establish polynomial regression and regularized regression methods on a controlled synthetic ECG signal. Problem 5 examines whether a model and regularization parameter selected from the original experiment can be transferred to a different ECG recording whose baseline characteristics are no longer represented by the original simple sinusoidal model.

The notebook therefore focuses on:

- Applying the previously selected Ridge model to a new ECG recording.
- Evaluating the result using indirect validation criteria.
- Investigating whether the original regularization parameter transfers successfully.
- Re-tuning the regularization parameter using a signal-based criterion.

The notebook is designed as a **standalone implementation of Problem 5** and contains only the prerequisite code required for this experiment.

---

# Objectives

The main objectives of Problem 5 are to:

- Test the generalization of the Ridge baseline-estimation model.
- Apply a previously selected regularization parameter to a new recording.
- Estimate baseline wander when the true baseline is hidden.
- Evaluate baseline correction using indirect signal-based criteria.
- Analyze TP-segment flatness before and after correction.
- Analyze R-peak amplitude stability.
- Determine whether the original regularization parameter transfers to the new recording.
- Re-tune the regularization parameter when necessary.

---

# Problem Formulation

The observed ECG is represented as:

\[
x[n] = s[n] + b[n]
\]

where:

- \(s[n]\) is the underlying ECG signal.
- \(b[n]\) is the unknown baseline wander.
- \(x[n]\) is the observed ECG.

The objective is to estimate:

\[
\hat{b}[n]
\]

and obtain the corrected ECG:

\[
\hat{s}[n]
=
x[n]-\hat{b}[n]
\]

Unlike the controlled experiments, the baseline in the new recording is treated as hidden from the estimation procedure.

---

# Why Generalization Is Important

A model that performs well on a controlled synthetic signal may not necessarily perform equally well on another ECG recording.

Real ECG recordings can contain baseline variations caused by factors such as:

- Slow trends.
- Patient movement.
- Respiration.
- Electrode movement.
- Motion artifacts.
- Changes in signal morphology.

Therefore, Problem 5 evaluates whether the previously selected Ridge model can generalize beyond the original synthetic baseline configuration.

---

# New Real-Like Recording

A new synthetic ECG recording is generated to represent a real-like test signal.

The ECG uses:

- Approximately 80 beats per minute.
- A different random seed from the original experiment.
- Measurement noise.
- A different baseline configuration.

The hidden baseline contains a combination of:

1. A linear trend.
2. A slow sinusoidal component.
3. A localized envelope producing irregular motion-artifact-like behavior.

The baseline is defined as:

\[
b(t)
=
0.05t
+
0.20
\sin(2\pi(0.15)t)
\exp
\left[
-\frac{(t-6)^2}{2(2)^2}
\right]
\]

The observed signal is:

\[
x(t)
=
s(t)+b(t)
\]

---

# Required Dependencies

Problem 5 uses only selected functionality from the earlier experiments.

| Dependency | Purpose |
|---|---|
| ECG generation/loading | Provides ECG data |
| R-peak detection | Identifies cardiac cycles |
| TP-segment extraction | Provides baseline-related samples |
| Time normalization | Normalizes polynomial input |
| Vandermonde matrix | Constructs polynomial features |
| Ridge regression | Estimates the baseline |
| Original \(\lambda^\star\) | Tests parameter transfer |

No unrelated algorithms from the previous problems are included.

---

# Problem 3 Dependency

Problem 5 uses the Ridge regression model established in Problem 3.

The polynomial degree is fixed at:

\[
p=25
\]

The regularization parameter is selected from the original experiment using the output SNR criterion.

The selected parameter is:

\[
\lambda^\star_{\text{original}}
\]

This parameter is then applied directly to the new recording.

---

# Problem 5(a) — Applying the Original Model

The new ECG recording is processed using the same basic workflow:

```text
New ECG Recording
        │
        ▼
R-Peak Detection
        │
        ▼
TP-Segment Extraction
        │
        ▼
Polynomial Design Matrix
        │
        ▼
Ridge Regression
        │
        ▼
Original λ*
        │
        ▼
Estimated Baseline
        │
        ▼
Baseline Removal
        │
        ▼
Corrected ECG