# Marketing Mix Modeling (MMM): Adstock & Saturation Optimization

## Project Overview

In modern marketing, assigning a linear ROI to advertising spend is highly inaccurate due to delayed consumer memory (carryover) and audience fatigue (diminishing returns).

The objective of this project is to build a custom **Marketing Mix Model (MMM)** from scratch in Python to accurately quantify the incremental sales driven by TV, Radio, and In-Store campaigns. By mathematically transforming raw spend data, this model isolates the true Return on Ad Spend (ROAS) to guide data-driven budget allocation.

## Data Description

* **Dataset:** 104 weeks (2 years) of chronological retail sales and marketing spend data.
* **Target Variable:** `NewVolSales` (Weekly sales volume).
* **Independent Variables:** Marketing spend (`TV`, `Radio`, `InStore`), Pricing metrics (`Base_Price`, `Discount`), and Campaign flags (`Newspaper`, `Website`).

## Mathematical Methodology & Feature Engineering

Instead of using automated black-box frameworks, the feature engineering was custom-coded using `numpy` to replicate real-world consumer behavior:

1. **Adstock Transformation (Carryover Effect):** Programmed a decaying lag function to model how consumer memory of an advertisement fades over time ($\lambda$ decay weight).
2. **Exponential Saturation (Diminishing Returns):** Applied a nonlinear transformation to model the ceiling of advertising effectiveness, preventing the model from overestimating the impact of infinite spend ($\alpha$ saturation parameter).
3. **Data Cleaning:** Handled missing values via mean imputation and converted text-based campaign schedules into binary variables for regression ingestion using `pandas`.

## Model Building

A Multivariate Ordinary Least Squares (OLS) regression was built using `statsmodels` to evaluate the transformed variables against baseline sales.

**Model Health:**

* **R-Squared:** 0.683 (The model successfully explains 68.3% of the variance in weekly sales).
* **F-Statistic:** 29.55 (p-value: 2.32e-21), indicating strong overall model significance.

## Business Results & Attribution

The model's P-values (P>|t|) were evaluated at a strict 0.05 significance level to determine channel viability:

* ✅ **Base_Price (0.000) & Discount (0.006):** Highly significant. Core pricing strategy is a primary driver of volume.
* ✅ **TV Advertising (0.001):** Highly significant. TV is the only paid media channel driving mathematically proven incremental sales.
* ❌ **Radio (0.360), In-Store (0.426), Newspaper (0.729):** Statistically insignificant. The model proves these channels are not driving measurable volume and budget should be reallocated.

### Final ROI / ROAS Calculation

Based on the exact coefficient weights assigned by the model, TV advertising performance was extracted:

* **Total Raw TV Spend:** $14,665.02
* **Total Incremental Revenue:** $4,778,333.00
* **Calculated ROAS:** $325.83

*(Note: The exceptionally high ROAS suggests the underlying dataset variables are scaled. However, the foundational attribution pipeline remains structurally sound for real-world scaling).*

## Tech Stack

* **Python**
* **Pandas** (Data manipulation & cleaning)
* **NumPy** (Mathematical feature engineering)
* **Statsmodels** (Multivariate OLS Regression & Statistical summaries)
