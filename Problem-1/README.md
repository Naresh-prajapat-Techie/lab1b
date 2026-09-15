# Problem 1 — Classical Baseline Estimation for ECG

## ECM-402: AI for Signal Processing — Lab 1B

---

## Overview

This solution presents a classical signal-processing approach for **ECG baseline wander estimation and removal**.

The objective of Problem 1 is to introduce the baseline-wander removal problem and establish a classical moving-average method as a reference baseline for comparison with the regression-based approaches developed in subsequent problems.

The notebook is implemented as a **standalone Problem 1 solution** and contains only the prerequisite code and experiments required for this problem.

---

## Objectives

The main objectives of this problem are to:

- Generate or load an ECG signal.
- Introduce synthetic baseline wander into the ECG.
- Detect R-peaks from the corrupted ECG.
- Extract TP-segment anchor points.
- Estimate baseline wander using a moving-average filter.
- Remove the estimated baseline from the corrupted ECG.
- Quantitatively evaluate the baseline estimation and corrected ECG.

---

## Problem Formulation

The observed ECG signal is modeled as

\[
x[n] = s[n] + b[n]
\]

where:

- \(s[n]\) represents the clean ECG signal.
- \(b[n]\) represents the baseline wander.
- \(x[n]\) represents the observed corrupted ECG.

The objective is to estimate the unknown baseline:

\[
\hat{b}[n] \approx b[n]
\]

and recover the corrected ECG:

\[
\hat{s}[n] = x[n] - \hat{b}[n]
\]

---

## Synthetic Baseline Wander

For controlled evaluation, the baseline wander is generated using two low-frequency sinusoidal components:

\[
b(t)
=
A_1\sin(2\pi f_1t+\phi_1)
+
A_2\sin(2\pi f_2t+\phi_2)
\]

The parameters used in the experiment are:

| Parameter | Value |
|---|---:|
| \(A_1\) | 0.15 |
| \(A_2\) | 0.10 |
| \(f_1\) | 0.2 Hz |
| \(f_2\) | 0.4 Hz |
| \(\phi_1\) | 0 |
| \(\phi_2\) | \(\pi/3\) |

The generated baseline is added to the clean ECG to create the corrupted signal.

---

## ECG Data

The notebook attempts to load **record 100 from the MIT-BIH Arrhythmia Database** using the `wfdb` package.

If the real record cannot be accessed, the notebook automatically generates a synthetic ECG signal.

The synthetic ECG consists of:

- P wave
- Q wave
- R wave
- S wave
- T wave
- Small measurement noise
- Small heart-rate variability

This fallback mechanism ensures that the notebook remains executable even when the external ECG database is unavailable.

---

## Methodology

### 1. ECG Generation / Loading

The notebook first obtains a clean ECG signal either from:

- The MIT-BIH Arrhythmia Database, or
- The built-in synthetic ECG generator.

---

### 2. Baseline Wander Addition

A low-frequency baseline is generated and added to the clean ECG:

\[
x[n] = s[n] + b[n]
\]

The clean ECG, baseline wander, and corrupted ECG are plotted for visualization.

---

### 3. R-Peak Detection

R-peaks are detected using `scipy.signal.find_peaks`.

The detection uses:

- An adaptive amplitude threshold based on the 85th percentile.
- A minimum peak distance of 0.4 seconds.

The minimum distance prevents unrealistically close R-peaks and approximately limits the detectable heart rate to 150 beats per minute.

---

### 4. TP-Segment Anchor Extraction

TP-segment samples are extracted between consecutive R-peaks.

The extraction uses the interval:

\[
0.45RR \rightarrow 0.85RR
\]

where \(RR\) is the number of samples between consecutive R-peaks.

The extracted points serve as approximate baseline-related observations.

---

### 5. Moving-Average Baseline Estimation

The classical baseline estimator is a moving-average filter.

For a window of length \(W\):

\[
\hat{b}[n]
=
\frac{1}{W}
\sum_{k}x[n-k]
\]

A window duration of **0.6 seconds** is used in the experiment.

The estimated baseline is then removed:

\[
\hat{s}[n]
=
x[n]-\hat{b}[n]
\]

---

## Evaluation Metrics

The performance of the baseline estimator is evaluated using the following metrics.

### Mean Squared Error

\[
MSE
=
\frac{1}{N}
\sum_{n=1}^{N}
(b[n]-\hat{b}[n])^2
\]

A lower MSE indicates more accurate baseline estimation.

---

### Baseline SNR

The baseline signal-to-noise ratio is calculated as:

\[
BSNR
=
10\log_{10}
\left(
\frac{\sum b[n]^2}
{\sum(b[n]-\hat{b}[n])^2}
\right)
\]

---

### Output SNR

The quality of the corrected ECG is evaluated using:

\[
SNR_{out}
=
10\log_{10}
\left(
\frac{\sum s[n]^2}
{\sum(s[n]-\hat{s}[n])^2}
\right)
\]

---

### SNR Improvement

The improvement obtained after baseline correction is calculated as:

\[
SNR_{imp}
=
SNR_{out}-SNR_{in}
\]

where \(SNR_{in}\) is the SNR before baseline correction.

---

## Notebook Workflow

The notebook follows the workflow below:

```text
Clean ECG
    │
    ▼
Generate Baseline Wander
    │
    ▼
Corrupted ECG
x[n] = s[n] + b[n]
    │
    ├───────────────┐
    ▼               ▼
R-Peak Detection   Moving Average
    │               │
    ▼               ▼
TP Anchors       Estimated Baseline
                    │
                    ▼
             Baseline Removal
                    │
                    ▼
             Corrected ECG
                    │
                    ▼
              Performance
                Evaluation



