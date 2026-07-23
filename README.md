# Identifying Risk Factors of Different Types of Chest Pain

Statistical analysis of heart disease risk factors across chest pain categories using feature engineering, correlation analysis, and multiple linear regression in R, evaluating predictive power and feature associations (multiple linear regression $R^2 \approx 0.01$).

Note on data access: The clinical data used in this project are based on a publicly available heart disease dataset from Kaggle (`Dataset Heart Disease.csv`). Users must download and place the raw dataset in the project root directory before running the R Markdown analysis script.

## Table of contents
- [Abstract](#abstract)
- [Research questions](#research-questions)
- [Data & cohort](#data--cohort)
- [Methods (high level)](#methods-high-level)
- [Process (detailed)](#process-detailed)
- [Key results (summary)](#key-results-summary)
- [Figures and tables](#figures-and-tables)
- [Files & artifacts](#files--artifacts)
- [Reproducibility notes](#reproducibility-notes)
- [What I learned](#what-i-learned-detailed)
- [Limitations](#limitations-explicit)
- [Future directions](#future-directions)
- [Broader significance](#broader-significance)
- [Acknowledgements & contact](#acknowledgements--contact)

# Statistical Analysis of Heart Disease Risk Factors
Author: Vanya Murzin  
Date: March 19, 2024

## Abstract
This project uses a public heart disease dataset ($n = 1,048$ patient observations) to evaluate relationships between patient demographics, clinical characteristics, and four distinct chest pain categories. Workflow includes data import, cleaning, exploratory visualization, subtype-specific correlation testing, and multivariable linear modeling predicting serum cholesterol. Results highlight that baseline age and cholesterol share virtually no linear correlation ($r \approx 0.097$, $R^2 \approx 0.0094$), while exercise-induced angina exhibits a significantly stronger association with chest pain etiology and cholesterol level than age or resting blood pressure. Full R scripts and artifacts are provided for complete reproducibility.

---

Note: variable counts and statistical summaries above reflect the canonical dataset run — verify against local data if using a modified file.

---

## Research questions
1. Is there a strong linear correlation between patient age and serum cholesterol levels within this cardiovascular cohort?  
2. How do age-cholesterol trends vary across the four specific classifications of chest pain?  
3. Does exercise-induced angina serve as a stronger multivariable predictor of cholesterol and risk than baseline age?

---

## Data & cohort
- Data source: Public heart disease dataset (Kaggle).  
- Subjects: $n = 1,048$ total clinical patient observations.  
- Data structure: 13 numeric and clinical variables without missing values after cleaning.  
- Key features: `age`, `sex`, `cholesterol`, `chest.pain.type`, `exercise.angina`, `resting.bps`, `max.heart.rate`, `target`.  
- Formatting: CSV source file read directly into R data frames.

### Cohort demographics (summary)
| Chest Pain Type | Clinical Description | Prevalence ($n$) | Exercise Angina Mean |
|---:|:---|---:|---:|
| Type 1 | Typical Angina | 142 | 0.47 |
| Type 2 | Atypical Angina | 168 | 0.18 |
| Type 3 | Non-Anginal Pain | 86 | 0.12 |
| Type 4 | Asymptomatic | 652 | 0.60 |
| Total | Complete Cohort | 1,048 | 0.43 |

---

## Methods (high level)
1. Import & cleaning: Import raw CSV data, perform structure inspection (`glimpse()`, `head()`), and verify/remove missing values (`na.omit()`).  
2. Baseline EDA: Evaluate distributions and produce bivariate scatter plots of `age` vs. `cholesterol` with linear regression lines.  
3. Global correlation & regression: Compute global Pearson correlation $r$ and baseline univariate linear regression ($R^2$).  
4. Chest pain subgroup analysis:  
   - Calculate categorical counts and proportion distributions via interactive pie charts (`plotly`).  
   - Compute mean exercise-induced angina rates across chest pain types using structured bar plots (`ggplot2`).  
5. Subtype-stratified modeling: Segment dataset by `chest.pain.type` (1 to 4), fitting per-group regression models to evaluate conditional correlation and $R^2$.  
6. Multivariable linear modeling: Fit multiple linear regression:  
   $$\text{cholesterol} \sim \text{age} + \text{sex} + \text{resting.bps} + \text{max.heart.rate} + \text{exercise.angina}$$  
7. Model evaluation & visualization: Extract regression coefficients, generate coefficient bar plots, and assess model fit via predicted vs. actual plots and residual summaries.

---

## Process (detailed)
- Data selection & QC
  - Loaded public heart disease data; checked variable types across all 13 features.  
  - Confirmed baseline row count ($n = 1,048$) and verified zero remaining `NA` values post-cleaning.  
- Preprocessing & subgroup categorization
  - Encoded `chest.pain.type` factors (1 = typical angina, 2 = atypical angina, 3 = non-anginal pain, 4 = asymptomatic).  
  - Derived mean summary statistics for binary `exercise.angina` relative to chest pain presentation.  
- Iterative analyses & subgroup comparisons
  - Tested simple linear fits for age vs. cholesterol globally ($r = 0.097$, $R^2 = 0.0094$).  
  - Stratified models across all four chest pain subgroups, discovering heterogeneous slopes (positive trends for Types 1, 2, 4; negative trend for Type 3).  
- Model selection & validation
  - Built multivariable linear regression incorporating demographic and clinical features.  
  - Compared individual predictor magnitudes; identified `exercise.angina` as a primary driver relative to traditional demographic predictors.  
- Documentation & packaging
  - Compiled code into R Markdown (`.Rmd`) and rendered standalone HTML and PDF outputs.

---

## Key results (summary)
- Baseline correlation: Global age vs. cholesterol exhibits almost zero linear relationship ($r \approx 0.097$, $R^2 \approx 0.0094$), showing that age alone explains less than 1% of cholesterol variance in this sample.  
- Subtype differences:  
  - Types 1, 2, and 4 present weak positive age-cholesterol trends.  
  - Type 3 (non-anginal pain) exhibits a weak negative correlation, indicating distinct clinical mechanisms.  
- Exercise angina impact: Exercise-induced angina is highest in Type 4 (mean = 0.60) and Type 1 (mean = 0.47) chest pain, but markedly lower in Types 2 and 3.  
- Multivariable model performance: Multiple linear regression demonstrates low overall $R^2$, indicating high unexplained variance, but highlights `exercise.angina` as holding stronger relative predictive value than age or cholesterol alone.

---

## Figures and tables (to include in report/presentation)
- Figure: Scatter plot — Baseline age vs. serum cholesterol with linear fit line.  
- Figure: Interactive pie chart — Prevalence distribution across chest pain types 1–4.  
- Figure: Bar plot — Mean exercise-induced angina by chest pain category.  
- Figure: Multi-panel scatter plot — Age vs. cholesterol stratified by chest pain type with per-group regression lines.  
- Figure: Coefficient plot — Standardized multiple linear regression estimates.  
- Figure: Diagnostics — Predicted vs. actual cholesterol values.  
- Table: Per-subtype correlation coefficients ($r$) and coefficient of determination ($R^2$).  

Figure captions (examples)
- Baseline age vs. cholesterol scatter plot showing flat regression slope ($R^2 = 0.0094$).  
- Distribution bar plot displaying high exercise angina proportion in Type 1 and Type 4 chest pain.  
- Subtype linear fits showing contrasting positive (Types 1, 2, 4) vs. negative (Type 3) slopes.  
- Multivariable coefficient chart showing relative effect size of clinical features on cholesterol predictions.

---

## Files & artifacts to include in repo
- `Dataset Heart Disease.csv` — raw input dataset.  
- `Project 1 - Identifying Risk Factors of Different Types of Chest Pain.Rmd` — complete executable R Markdown file.  
- `Project_1_Analysis.html` — rendered HTML report.  
- `Project_1_Analysis.pdf` — rendered PDF document.  
- `figures/` — saved PNG visualizations (bar charts, scatter plots, coefficient plots).  
- `sessionInfo.txt` — recorded R environment details for execution tracking.

---

## Reproducibility notes (what to provide)
- Documented R script / R Markdown setup instructions:  
  - Load `Dataset Heart Disease.csv` and execute initial structure and NA checks.  
  - Run package load script suppressing startup messages (`dplyr`, `ggplot2`, `plotly`, `tidymodels`, `gridExtra`).  
  - Execute chunk-by-chunk analysis to reproduce correlation matrices, subtype regressions, and multivariable linear models.  
- Exact R package requirements (`tidyverse`, `plotly`, `rmarkdown`, `rsample`).  
- Explicit path setting instructions ensuring the dataset rests in the working directory (`getwd()`).

---

## What I learned (detailed)
- Analytical rigor & decisions
  - Statistical independence: Unadjusted bivariate correlations can obscure critical subgroup dynamics; stratifying by chest pain type reveals concealed directional trends.  
  - Model limitations: Low $R^2$ values in linear models emphasize that simple clinical parameters leave substantial unexplained variance in serum cholesterol.  
- Interpretation & clinical insights
  - Exercise-induced angina provides stronger predictive utility than age alone when assessing cardiovascular presentation risk.  
  - Non-anginal chest pain (Type 3) behaves differently from exertional types, suggesting non-cardiac or unique clinical pathways.  
- Technical & professional skills
  - Practical competency in data wrangling using `dplyr` and data visualization with `ggplot2` and `plotly`.  
  - Experience structuring linear models (`lm()`), extracting model diagnostics, and rendering dynamic R Markdown reports.  
  - Translating raw statistical outputs into actionable clinical insights for healthcare decision-support discussions.  
- Project management & reproducibility
  - Structured documentation practices, ensuring script rendering and figure outputs align seamlessly in knit documents.

---

## Limitations (explicit)
- Low $R^2$ values indicate high residual variance; linear parameters alone do not fully capture cholesterol variation.  
- Single observational dataset without external cohort validation.  
- Unmeasured clinical confounders present (e.g., patient medication, dietary habits, smoking history, genetic markers).  
- Evaluation relies on linear assumptions; non-linear interactions were not evaluated in initial regression passes.

---

## Future directions
- Incorporate non-linear and tree-based machine learning architectures (Random Forest, XGBoost).  
- Introduce interaction terms into regression models (e.g., `exercise.angina : age`).  
- Reframe outcome targets toward binary classification of heart disease presence rather than continuous cholesterol estimation.  
- Validate findings across secondary external cardiovascular datasets.

---

## Broader significance
This project demonstrates that standard demographic assumptions (such as age dictating cholesterol or chest pain severity) do not fully capture cardiovascular risk profiles. By dissecting subgroup mechanics and evaluating multivariable feature importance, this work illustrates how data-driven stratification can refine risk assessment beyond basic demographic benchmarks.

---

## Acknowledgements
- Dataset provider: Kaggle Public Repositories (Heart Disease Dataset).  
- R Open-Source Community and maintainers of the `tidyverse` and `tidymodels` ecosystems.

---

## Contact
Vanya Murzin — evawnmurzin@gmail.com

Quick start
1. Clone the repository and place `Dataset Heart Disease.csv` in the root folder.  
2. Open `Project 1 - Identifying Risk Factors of Different Types of Chest Pain.Rmd` in RStudio.  
3. Run `rmarkdown::render("Project 1 - Identifying Risk Factors of Different Types of Chest Pain.Rmd")` to generate output reports.

License & contact
- Author: Vanya Murzin — Predictive Analytics Project  
- For questions, reproduction help, or collaboration: evawnmurzin@gmail.com
