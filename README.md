# ECM-402 — AI for Signal Processing | Lab 1B

## ECG Baseline Wander Estimation and Removal using Regression

This repository separates the five problems from the supplied `ECM402_Lab1B_Solution.ipynb` into clean, independently executable Jupyter notebooks.

## Structure

```text
ECM402_Lab1B_GitHub_Repo/
├── README.md
├── requirements.txt
├── .gitignore
├── Problem-1/
│   ├── problem_1.ipynb
│   └── README.md
├── Problem-2/
│   ├── problem_2.ipynb
│   └── README.md
├── Problem-3/
│   ├── problem_3.ipynb
│   └── README.md
├── Problem-4/
│   ├── problem_4.ipynb
│   └── README.md
└── Problem-5/
    ├── problem_5.ipynb
    └── README.md
```

## Problem Overview

| Problem | Topic |
|---|---|
| 1 | Problem formulation and classical moving-average baseline |
| 2 | Polynomial regression and degree selection |
| 3 | Ridge and Lasso regularization |
| 4 | Local polynomial regression and method comparison |
| 5 | Generalization and indirect validation |

## Standalone-file policy

These are **dependency-aware extractions**, not blind notebook cell copies.

Each problem contains:
1. Its own required imports.
2. Earlier functions/data only when the problem actually depends on them.
3. The complete dependency chain needed to avoid `NameError` and notebook-order issues.
4. No unrelated problem calculations.

For example, Problem 5 includes the Ridge-selection code it needs, but does not include unrelated Lasso computations.

## Installation

Python 3.10+ is recommended.

```bash
pip install -r requirements.txt
```

## Run

```bash
python Problem-1/problem_1.ipynb
python Problem-2/problem_2.ipynb
python Problem-3/problem_3.ipynb
python Problem-4/problem_4.ipynb
python Problem-5/problem_5.ipynb
```

The scripts can also be run individually in VS Code.

## Data

The original solution attempts to load MIT-BIH record 100 with `wfdb`. If PhysioNet is unavailable, the notebook's synthetic ECG fallback is used automatically.

## Source

Prepared from the supplied `ECM402_Lab1B_Solution.ipynb`.

## GitHub

```bash
git init
git add .
git commit -m "Add ECM-402 Lab 1B solutions"
git branch -M main
git remote add origin <YOUR_GITHUB_REPOSITORY_URL>
git push -u origin main
```
