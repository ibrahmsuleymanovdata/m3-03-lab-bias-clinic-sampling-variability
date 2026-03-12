![logo_ironhack_blue 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# Lab | Bias Clinic and Sampling Variability

## Overview

Every dataset you'll encounter as a data scientist was collected by someone, using some method, for some purpose — and each of those choices can introduce bias. Selection bias, survivorship bias, measurement bias, and data leakage are not just textbook concepts; they routinely distort real analyses and lead to wrong conclusions. The first skill of a responsible data scientist is recognizing when data *can't* be taken at face value.

In this lab you'll work through two complementary tracks. First, you'll diagnose bias in realistic case studies — reading scenarios and identifying what went wrong in the data collection process. Second, you'll move to code: simulating biased and unbiased sampling from a known population so you can *see* how bias shifts estimates and how sampling variability behaves. You'll finish with a preview of bootstrapping — a resampling technique that lets you quantify uncertainty without relying on distributional assumptions — and a hands-on demonstration of data leakage in a prediction pipeline.

This lab connects to the lesson on sampling, bias, and data quality. Where the lesson introduced types of bias and the concept of a sampling distribution, here you'll build intuition through simulation and critical analysis.

## Learning Goals

By the end of this lab, you should be able to:

- Identify selection bias, survivorship bias, and measurement bias in real-world data scenarios.
- Simulate random, stratified, and biased sampling from a population with known parameters.
- Compare sample statistics to true population parameters across many repeated samples.
- Construct a basic bootstrap distribution and compute a bootstrap confidence interval.
- Demonstrate how data leakage inflates model performance and explain why it's dangerous.
- Articulate best practices for collecting representative, unbiased data.

## Setup and Context

You'll work inside a Jupyter Notebook for this lab. Part of the lab involves written analysis (the bias case studies), and part involves simulation code. Both belong in the same notebook, using markdown cells for analysis and code cells for simulation.

A synthetic population dataset is generated at the start so that you always know the "ground truth" — this lets you measure exactly how far sample estimates deviate from reality.

## Requirements

### Fork and clone

1. Fork this repository to your own GitHub account.
2. Clone the fork to your local machine.
3. Navigate into the project directory.

### Python environment

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

- **Python** ≥ 3.9
- **pandas** — data manipulation
- **numpy** — numerical operations and random sampling
- **matplotlib** — plotting
- **seaborn** — statistical visualization
- **scikit-learn** — used in the data leakage demonstration (Task 5)

## Getting Started

1. Create a new Jupyter Notebook called **`m3-03-bias-clinic-sampling-variability.ipynb`**.
2. Import cell:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score

np.random.seed(42)
sns.set_style("whitegrid")
```

3. Work through the tasks in order. Tasks 1–2 are analytical; Tasks 3–5 are code-heavy.
4. Use markdown cells liberally to explain your reasoning and observations.

## Tasks

### Task 1: Bias Diagnosis — Case Studies

For each of the following scenarios, identify the **type of bias** present (selection bias, survivorship bias, measurement bias, response bias, or another relevant type), explain **how** it distorts the conclusions, and suggest **one concrete fix**.

**Scenario A — App Review Analysis:**
A product team analyzes their app's reviews on the App Store to understand user satisfaction. The average rating is 4.3/5, so they conclude that most users are happy. They plan to reduce investment in customer support.

**Scenario B — Startup Success Study:**
A business school studies 200 successful tech startups (all founded in the last 10 years and still operating) to identify common traits that predict startup success. They find that 80% had a pivot in their first two years and conclude that pivoting is a key success strategy.

**Scenario C — Health Survey:**
A health organization sends a voluntary online survey to 50,000 email subscribers asking about exercise habits and health outcomes. The survey receives 5,000 responses (10% response rate). Results show that respondents exercise an average of 5 hours per week and have excellent self-reported health.

**Scenario D — Salary Benchmarking:**
A recruiting platform publishes average salaries by job title based on user-submitted salary data. The platform is popular among tech workers in large cities. A company in a small town uses this data to set their salary bands.

Write your analysis in markdown cells — no code required for this task. Aim for 3–5 sentences per scenario.

### Task 2: Create the Population

Before you can study sampling, you need a known population to sample from.

1. Generate a synthetic population of **100,000** individuals with the following columns:
   - `age`: integers drawn from a realistic distribution (e.g., a clipped normal centered at 40 with SD of 15, range 18–85).
   - `income`: correlated with age — use a linear relationship with noise (e.g., `income = 1500 * age + normal_noise`). Clip to a realistic range (e.g., 15,000–250,000).
   - `satisfaction`: a score from 1–10 that depends on income (higher income → slightly higher satisfaction, with plenty of noise).
   - `region`: categorical — randomly assign "Urban" (60%), "Suburban" (25%), "Rural" (15%).
2. Compute and store the **true population parameters**: mean age, mean income, mean satisfaction, and the proportion in each region.
3. Display the population summary and a grid of histograms (one per numerical variable).

These population parameters are your ground truth — everything in the following tasks is measured against them.

### Task 3: Biased vs. Unbiased Sampling

Draw samples from your population using three different strategies and compare how well each recovers the true population parameters.

1. **Simple random sample** (n = 200): every individual has an equal chance of being selected.
2. **Biased sample — Urban only** (n = 200): only sample from individuals in the "Urban" region.
3. **Biased sample — High-income filter** (n = 200): only sample from individuals with income above the population median.

For each sample:
- Compute mean age, mean income, and mean satisfaction.
- Display the results alongside the true population parameters in a comparison table.
- Create overlapping KDE plots (sample vs. population) for income and satisfaction.

Now, repeat each sampling strategy **1,000 times** and collect the sample means:
- Plot the **sampling distribution of the mean income** for each strategy (three histograms, side by side).
- Mark the true population mean on each histogram.

**Guiding question:** Which sampling strategies produce biased estimates? How can you tell from the sampling distributions?

## Submission

### What to submit

- `m3-03-bias-clinic-sampling-variability.ipynb` — your completed notebook with all code, outputs, and markdown explanations.

### Definition of done (checklist)

- [ ] All four bias case studies are analyzed with type, explanation, and fix.
- [ ] Population is generated with 100,000 rows and true parameters are stored.
- [ ] Three sampling strategies are compared across 1,000 repeated samples.
- [ ] The notebook runs top-to-bottom without errors (`Kernel → Restart & Run All`).

### How to submit (Git workflow)

```bash
git add .
git commit -m "lab: complete bias clinic and sampling variability"
git push origin main
```

Then open a **Pull Request** on the original repository with a brief description of your work.
