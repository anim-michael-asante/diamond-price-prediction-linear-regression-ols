<div align="center">

# Diamond Price Prediction — Linear Regression (OLS)

**Quantifying the relationship between carat weight and diamond market price using Ordinary Least Squares regression.**

<br/>

<!-- Core Stack Badges -->
![Python](https://img.shields.io/badge/Python-3.13+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![JupyterLab](https://img.shields.io/badge/JupyterLab-4.5.7-F37626?style=for-the-badge&logo=jupyter&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-3.0.2-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-2.4.4-4DABCF?style=for-the-badge&logo=numpy&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-1.17.1-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-3.10.8-71D291?style=for-the-badge&logo=python&logoColor=white)

<br/>

<!-- Project Meta Badges -->
![statsmodels](https://img.shields.io/badge/statsmodels-0.14.6-blue?style=flat-square&logo=python&logoColor=white)
![Model](https://img.shields.io/badge/Model-OLS_Regression-orange?style=flat-square)
![R²](https://img.shields.io/badge/R%C2%B2-0.893-2e7d32?style=flat-square)
![Samples](https://img.shields.io/badge/Samples-308-3776AB?style=flat-square)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)
![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen?style=flat-square)

</div>

---

## Table of Contents

- [Overview](#overview)
- [Why This Project](#why-this-project)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Model Results](#model-results)
- [Diagnostic Notes](#diagnostic-notes)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [Tech Stack](#tech-stack)
- [Roadmap](#roadmap)
- [Citation](#citation)
- [Contributing](#contributing)
- [License](#license)
- [Author](#author)

---

## Overview

This project builds and evaluates a **Simple Linear Regression model** to predict the price of certified diamonds based on their carat weight. Using the Ordinary Least Squares (OLS) method via `statsmodels`, the model quantifies the linear relationship between a diamond's carat weight and its market price — producing a full econometric summary with coefficients, p-values, confidence intervals, and residual diagnostics.

The dataset contains **308 GIA-certified diamonds** with attributes including carat, colour, clarity, certification body, and price. The model explains **89.3% of price variability** with carat alone, confirming carat weight as the dominant pricing driver in the diamond market.

**Target audience:**
- Students learning supervised regression from scratch
- Data science practitioners seeking a clean OLS workflow with full statistical output
- Portfolio builders demonstrating regression fundamentals with real-world economic data

---

## Why This Project

Pricing in the diamond industry is notoriously opaque. Buyers, jewellers, and appraisers need reliable, data-driven methods to estimate fair market value — yet pricing is often informal or experience-dependent. This project addresses that gap in four ways:

1. **Establishes a quantitative pricing baseline** — A statistically rigorous model provides a defensible estimate of price purely from carat weight, the most universally agreed-upon diamond attribute.

2. **Demonstrates regression fundamentals in a real economic context** — Unlike toy datasets, diamond pricing carries genuine market implications, making the model both pedagogically valuable and practically meaningful.

3. **Produces interpretable coefficients** — The OLS model yields a coefficient of **~$11,600 per carat**, meaning every additional carat adds approximately $11,600 to the predicted price. This is a business-readable insight, not just a metric.

4. **Provides a statistical foundation for more complex models** — This simple linear baseline serves as a reproducible benchmark before introducing multiple regression with colour, clarity, and certification as additional predictors.

---

## Dataset

| Attribute | Type | Description |
|---|---|---|
| `carat` | `float` | Diamond weight in carats — primary predictor |
| `colour` | `str` | Colour grade on D–Z scale (D = colourless, best) |
| `clarity` | `str` | Clarity grade: FL, VVS1, VVS2, VS1, VS2, SI1, SI2, I1 |
| `certification` | `str` | Certifying body: GIA, IGI, or HRD |
| `price` | `int` | Market price in USD — target variable |

- **Observations:** 308 diamonds
- **Source:** `diamond.csv` — GIA-certified diamond pricing dataset
- **Preprocessing status:** Clean — no missing values, no outliers requiring treatment
- **Split used:** Full dataset for OLS estimation (baseline model; train/test split recommended for predictive evaluation)

---

## Methodology

```
diamond.csv
    └── Load & Inspect (pandas)
         └── Define Y = price,  X = carat
              └── Add OLS Constant → sm.add_constant(X)
                   └── Fit OLS → sm.OLS(endog=Y, exog=X).fit()
                        └── Evaluate → model.summary()
```

The workflow follows the standard econometric approach:

1. Load and inspect the raw dataset with `pandas`
2. Define the dependent variable (`price`) and independent variable (`carat`)
3. Add a constant term via `sm.add_constant()` to estimate the model intercept
4. Fit the OLS model using `statsmodels.api.OLS`
5. Interpret the full statistical summary — coefficients, p-values, R², F-statistic, and residual diagnostics
6. Validate model assumptions using Durbin-Watson, Omnibus, Jarque-Bera, and Kurtosis tests

---

## Model Results

```
                            OLS Regression Results
==============================================================================
Dep. Variable:                  price   R-squared:                       0.893
Model:                            OLS   Adj. R-squared:                  0.892
Method:                 Least Squares   F-statistic:                     2541.
No. Observations:                 308   Prob (F-statistic):          3.04e-150
Df Residuals:                     306   AIC:                             5200.
Df Model:                           1   BIC:                             5207.
==============================================================================
                 coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------
const      -2298.3576    158.531    -14.498      0.000   -2610.306   -1986.410
carat        1.16e+04    230.111     50.406      0.000    1.11e+04    1.21e+04
==============================================================================
```

**Fitted equation:**

```
price = -2,298.36 + 11,600 × carat
```

### Key Findings

| Metric | Value | Business Interpretation |
|---|---|---|
| **R²** | 0.893 | Carat weight explains **89.3%** of diamond price variability |
| **Adj. R²** | 0.892 | Adjusted for degrees of freedom — model is not overfitted |
| **Carat Coefficient** | ~$11,600 | Each additional carat increases expected price by ~$11,600 |
| **Intercept** | -$2,298 | Theoretical base (not meaningful in isolation for diamonds < ~0.2ct) |
| **F-statistic** | 2541 | Overall model is overwhelmingly significant |
| **F p-value** | 3.04e-150 | Probability the model has no predictive power is effectively zero |
| **Carat p-value** | < 0.001 | Carat weight is a highly significant predictor of price |
| **95% CI (carat)** | [$11,100 – $12,100] | True price increase per carat lies in this range with 95% confidence |

> For every **0.10 carat increase** in diamond weight, the model predicts an average price increase of approximately **$1,160**.

---

## Diagnostic Notes

| Diagnostic | Value | Assessment |
|---|---|---|
| **Durbin-Watson** | 1.216 | Mild positive autocorrelation — acceptable for cross-sectional data |
| **Omnibus p-value** | 0.000 | Residuals deviate significantly from normality |
| **Skewness** | 2.168 | Right-skewed residuals; price distribution has a long upper tail |
| **Kurtosis** | 12.187 | Heavy-tailed residuals — potential high-leverage outliers at upper carat range |
| **Jarque-Bera p-value** | 2.56e-288 | Residuals are **not** normally distributed |

> **Recommended next step:** Apply a log-transformation to the target (`log(price)`) to address skewness and improve residual normality. This is a standard refinement for price modelling tasks.

---

## Getting Started

### Prerequisites

- Python 3.10 or higher
- `pip` package manager

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/anim-michael-asante/diamond-price-regression
cd diamond-price-regression

# 2. Create and activate a virtual environment
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt
```

### Run the Notebook

```bash
jupyter lab notebooks/Linear_Regression.ipynb
```

### Quick Usage

```python
import pandas as pd
import statsmodels.api as sm

df = pd.read_csv('data/diamond.csv')

Y = df['price']
X = sm.add_constant(df['carat'])

model = sm.OLS(endog=Y, exog=X).fit()
print(model.summary())
```

---

## Project Structure

```
diamond-price-regression/
├── data/
│   └── diamond.csv                  ← Dataset (308 GIA-certified diamonds)
├── notebooks/
│   └── Linear_Regression.ipynb      ← Main modelling notebook
├── reports/
│   └── figures/                     ← Saved plots and visualisations
├── requirements.txt                 ← Pinned dependencies
├── .gitignore
└── README.md
```

---

## Tech Stack

| Tool | Version | Purpose |
|---|---|---|
| Python | 3.13+ | Core language |
| pandas | 3.0.2 | Data loading and manipulation |
| NumPy | 2.4.4 | Numerical computing |
| statsmodels | 0.14.6 | OLS regression with full statistical output |
| patsy | 1.0.2 | Formula interface for statsmodels |
| SciPy | 1.17.1 | Statistical functions (statsmodels dependency) |
| Matplotlib | 3.10.8 | Visualisation |
| JupyterLab | 4.5.7 | Interactive notebook environment |

---

## Roadmap

- [x] Simple linear regression — carat vs. price (OLS)
- [ ] Log-linear transformation — `log(price)` to correct residual skewness
- [ ] Multiple regression — add `colour`, `clarity`, `certification` as predictors
- [ ] Residual diagnostics plots — Q-Q plot, residuals vs. fitted, scale-location
- [ ] Train/test split evaluation — MAE, RMSE, cross-validated R²
- [ ] Streamlit demo — interactive price estimator by carat input
- [ ] Model export with `joblib` for lightweight deployment

---

## Citation

If you reference this project in academic work or course reports, please cite as:

```bibtex
@misc{animasante2026diamond,
  author       = {Asante Anim, Michael},
  title        = {Diamond Price Prediction using Ordinary Least Squares Regression},
  year         = {2026},
  publisher    = {GitHub},
  journal      = {GitHub Repository},
  howpublished = {\url{https://github.com/anim-michael-asante/diamond-price-regression}},
  note         = {BSc Cyber Security, University of Mines and Technology (UMaT), Tarkwa, Ghana}
}
```

---

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change. Ensure all notebooks run cleanly end-to-end before submitting.

---

## License

Distributed under the [MIT License](LICENSE). See `LICENSE` for full terms.

---

## Author

**Michael Asante Anim** | `0x1aerixis`
BSc Cyber Security — University of Mines and Technology (UMaT), Tarkwa, Ghana


[![GitHub](https://img.shields.io/badge/GitHub-anim--michael--asante-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/anim-michael-asante)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://linkedin.com/in/michael-asante-anim)
[![X](https://img.shields.io/badge/X-0x1aerixis-000000?style=flat-square&logo=x&logoColor=white)](https://x.com/0x1aerixis)
[![TryHackMe](https://img.shields.io/badge/TryHackMe-0x1aerixis-212C42?style=flat-square&logo=tryhackme&logoColor=white)](https://tryhackme.com/p/0x1aerixis)
[![Discord](https://img.shields.io/badge/Discord-0x1aerixis-5865F2?style=flat-square&logo=discord&logoColor=white)](https://discord.com/users/0x1aerixis)

---

<div align="center">
<sub>Built in the lab. Documented for the field.</sub>
</div>
