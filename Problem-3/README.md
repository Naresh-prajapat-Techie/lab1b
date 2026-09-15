# Problem 3 — Regularized Regression: Ridge and Lasso

## ECM-402: AI for Signal Processing — Lab 1B

---

## Overview

This notebook investigates **regularized regression methods for ECG baseline-wander estimation**, focusing on Ridge and Lasso regression.

Polynomial regression can provide a flexible model for estimating ECG baseline wander; however, higher-order polynomial models may become numerically unstable and can exhibit undesirable coefficient behavior.

Problem 3 addresses this issue by introducing regularization and evaluating the effect of the regularization parameter on baseline estimation and ECG reconstruction.

The notebook is designed as a **standalone implementation of Problem 3**. Only the prerequisite code required to construct the ECG signal, extract baseline-related anchor points, evaluate the models, and provide the unregularized polynomial reference model is retained.

---

## Objectives

The main objectives of Problem 3 are to:

- Introduce Ridge regularization for polynomial baseline estimation.
- Introduce Lasso regularization for polynomial baseline estimation.
- Investigate the effect of the regularization parameter \( \lambda \).
- Compare Ridge and Lasso coefficient behavior.
- Evaluate the quality of the corrected ECG.
- Compare regularized models with the unregularized polynomial reference.
- Identify the regularization parameter that provides the best output SNR.

---

## Problem Formulation

The observed ECG signal is modeled as:

\[
x[n] = s[n] + b[n]
\]

where:

- \(s[n]\) is the clean ECG signal.
- \(b[n]\) is the baseline wander.
- \(x[n]\) is the corrupted ECG.

The objective is to estimate:

\[
\hat{b}[n]
\]

and recover the corrected ECG:

\[
\hat{s}[n] = x[n] - \hat{b}[n]
\]

A polynomial basis of degree \(p=25\) is used for the regularized regression experiments.

---

## Why Regularization?

High-degree polynomial models can provide considerable flexibility, but this flexibility can result in:

- Large regression coefficients.
- Numerical instability.
- Sensitivity to noise.
- Poor generalization.
- Unwanted oscillations in the estimated baseline.

Regularization introduces a penalty on the model coefficients and helps control model complexity.

---

# Ridge Regression

Ridge regression minimizes an objective of the form:

\[
\min_{\mathbf{w}}
\left\{
\|\mathbf{y}-\Phi\mathbf{w}\|_2^2
+
\lambda
\|\mathbf{w}\|_2^2
\right\}
\]

where:

- \(\Phi\) is the polynomial design matrix.
- \(\mathbf{w}\) is the coefficient vector.
- \(\lambda\) controls the regularization strength.

The implementation does not regularize the intercept term.

The Ridge solution is obtained using:

\[
\mathbf{w}
=
(\Phi^T\Phi+\lambda D)^{-1}
\Phi^T\mathbf{y}
\]

where \(D\) contains the regularization terms.

---

# Lasso Regression

Lasso regression introduces an \(L_1\) penalty:

\[
\min_{\mathbf{w}}
\left\{
\|\mathbf{y}-\Phi\mathbf{w}\|_2^2
+
\lambda
\|\mathbf{w}\|_1
\right\}
\]

Unlike Ridge regression, the \(L_1\) penalty can drive some coefficients toward exactly zero.

This produces a sparse coefficient representation and provides an additional form of model complexity control.

The implementation uses:

```python
sklearn.linear_model.Lasso


