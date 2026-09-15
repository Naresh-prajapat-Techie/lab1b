# Problem 2 — Polynomial Regression for ECG Baseline Estimation

---

## 📌 Overview

**Polynomial Regression for ECG Baseline Wander Estimation and Removal**.

The objective is to model the low-frequency baseline wander present in an ECG signal using polynomial regression and evaluate how the polynomial degree affects estimation accuracy, numerical stability, and the quality of the corrected ECG signal.

The implementation includes the minimum prerequisite code required to make **Problem 2 independently executable**, without depending on the notebooks for the other problems.

---

## 🎯 Objective

The main objectives of Problem 2 are to:

- Construct polynomial regression models for ECG baseline estimation.
- Investigate different polynomial degrees.
- Analyze numerical conditioning of the polynomial design matrix.
- Estimate and remove baseline wander from the corrupted ECG.
- Study the bias–variance behavior of polynomial regression.
- Select an appropriate polynomial degree using a validation split.

---

## 🧠 Methodology

The ECG signal is represented as

\[
x(t) = s(t) + b(t)
\]

where:

- \(s(t)\) is the clean ECG signal.
- \(b(t)\) is the baseline wander.
- \(x(t)\) is the observed corrupted ECG signal.

The baseline is estimated using a polynomial model:

\[
\hat{b}(t) = \sum_{k=0}^{p} w_k t^k
\]

where:

- \(p\) is the polynomial degree.
- \(w_k\) are the regression coefficients.
- \(t\) is normalized to the interval \([-1,1]\).

The corrected ECG is then obtained as:

\[
\hat{s}(t) = x(t) - \hat{b}(t)
\]

---

## 🔬 What Is Implemented

### 1. ECG Signal Generation / Loading

The notebook provides functions for:

- Generating a synthetic ECG signal.
- Generating individual ECG beats.
- Loading a MIT-BIH record when available.
- Falling back to a synthetic ECG signal if the real dataset cannot be loaded.

This makes the notebook executable even when the MIT-BIH database is unavailable.

---

### 2. Baseline Wander Generation

A low-frequency baseline wander is generated using sinusoidal components:

\[
b(t) = A_1\sin(2\pi f_1t+\phi_1)
      + A_2\sin(2\pi f_2t+\phi_2)
\]

The baseline is added to the clean ECG to obtain the corrupted signal.

---

### 3. TP Segment Anchor Extraction

R-peaks are detected from the ECG signal, and TP-segment samples are extracted around the detected cardiac cycles.

These TP-segment points are used as approximate observations of the baseline because they are relatively free from the major ECG waves.

---

### 4. Polynomial Regression

Polynomial regression is evaluated using the following degrees:

```text
1, 3, 5, 9, 15, 25


