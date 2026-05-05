# 🏠 Boston Housing Dataset — Business Intelligence Case Study

**Module:** Business Intelligence (CCS 3342)  
**Author:** S.A. Thilani Dilmani  
**Registration Number:** CIT-23-02-0173

---

## 📌 Project Overview

This project applies the full data science pipeline to the classic **Boston Housing Dataset** (Harrison & Rubinfeld, 1978) to uncover the key factors that drive residential property prices across 506 census tracts in Boston, Massachusetts.

The analysis covers data cleaning, exploratory data analysis (EDA), inferential statistics, multiple linear regression modelling, and Power BI visualisation — delivering evidence-based recommendations for real estate developers, investors, and urban planners.

---

## 📁 Repository Structure

```
├── boston.csv                                              # Raw dataset (506 records × 14 features)
├── CCS3342_Case_Study__CIT-23-02-0173.ipynb               # Main Jupyter Notebook (full pipeline)
├── CCS_3342-Case_Study__CIT-23-02-0173.ipynb              # Alternate notebook version
├── Case_Study__Final_Assignment_Visualizations_CIT-23-02-0173.pbix   # Power BI dashboard file
├── CCS3342_Case_Study_Power_BI_Visualizations_CIT-23-02-0173.pdf     # Power BI dashboard (PDF export)
├── CCS3342_Case_Study_Presentation_CIT-23-02-0173.pdf     # Presentation slides (PDF)
├── CCS3342_Case_Study_Presentation_Video_CIT-23-02-0173.mp4          # Recorded presentation video
└── README.md
```

---

## 📊 Dataset

| Property | Detail |
|---|---|
| **Source** | Harrison & Rubinfeld (1978) |
| **Original Records** | 506 |
| **Records After Cleaning** | 466 (40 outliers removed) |
| **Features** | 14 |
| **Target Variable** | `MEDV` — Median house value ($1,000s) |

### Feature Descriptions

| Feature | Type | Description |
|---|---|---|
| CRIM | Continuous | Per capita crime rate by town |
| ZN | Continuous | Residential land zoned for large lots (%) |
| INDUS | Continuous | Non-retail business acres per town (%) |
| CHAS | Binary 0/1 | Charles River proximity (1 = bounds river) |
| NOX | Continuous | Nitric oxide concentration (parts per 10M) |
| **RM** ⭐ | Continuous | Average number of rooms per dwelling |
| AGE | Continuous | % owner-occupied units built before 1940 |
| DIS | Continuous | Weighted distances to 5 employment centres |
| RAD | Integer | Index of accessibility to radial highways |
| TAX | Continuous | Full-value property-tax rate per $10,000 |
| PTRATIO | Continuous | Pupil-teacher ratio by town |
| B | Continuous | 1000(Bk−0.63)² — race composition index |
| **LSTAT** ⭐ | Continuous | % lower-status of the population |
| **MEDV** 🎯 | Continuous | **TARGET** — Median home value in $1,000s |

---

## 🔧 Data Cleaning & Pre-Processing

- **Missing Values:** 0 — dataset is 100% complete, no imputation required
- **Duplicates:** 0 — all 506 records are unique
- **Outlier Removal:** IQR method on MEDV → Q1=$17,020, IQR=$7,980, bounds [$5,060 – $36,960]; 40 outliers removed (mainly $50k censored values)
- **Final Dataset:** 466 records

### Feature Engineering

| New Feature | Formula | Purpose |
|---|---|---|
| `PRICE_USD` | MEDV × 1000 | Convert to dollar values |
| `LOG_CRIM` | log(1 + CRIM) | Reduce skewness: 5.29 → 1.31 |
| `HIGH_PRICE` | 1 if MEDV ≥ $20,600 | Binary target for classification metrics |

---

## 📐 Inferential Statistics — Hypothesis Testing

Three statistical tests were conducted at **α = 0.05**:

| Test | Variable | Finding | Result |
|---|---|---|---|
| T-Test 1 | River Proximity (CHAS) | River tracts avg $23,193 vs $20,561 — **$2,632 premium (12.8%)** | ✅ REJECT H₀ (p=0.0362) |
| T-Test 2 | Room Count (RM) | High-RM avg $23,427 vs $18,012 — **$5,415 premium (30.1%)** | ✅ REJECT H₀ (p<0.000001) |
| One-Way ANOVA | Highway Access (RAD) | Low=$22,211, Medium=$23,297, High=$15,242 | ✅ REJECT H₀ (p<0.0001) |

**95% Confidence Interval** (n=100): Sample mean = $20,363 | CI = ($19,129 — $21,597) — high sampling accuracy confirmed.

---

## 🤖 Regression Model

**Algorithm:** Multiple Linear Regression (`sklearn.linear_model.LinearRegression`)

```
MEDV = β₀ + β₁·RM + β₂·LSTAT + β₃·PTRATIO + ... + β₁₃·CHAS + ε
```

| Configuration | Detail |
|---|---|
| Features (X) | 13 — RM, LSTAT, PTRATIO, NOX, CRIM, DIS, TAX, AGE, ZN, INDUS, RAD, B, CHAS |
| Target (y) | MEDV |
| Train / Test Split | 80% (372 records) / 20% (94 records), random_state=42 |
| Feature Scaling | StandardScaler (zero mean, unit variance) |
| Cross-Validation | 5-Fold CV on full scaled dataset |

---

## 📈 Model Evaluation

### Regression Metrics (Test Set, n=94)

| Metric | Value | Interpretation |
|---|---|---|
| R² | **0.7525** | Model explains 75.2% of price variance |
| RMSE | **3.151** | Average prediction error of $3,151 |
| MAE | **2.231** | Mean absolute error of $2,231 per property |

### Classification Metrics (Threshold: MEDV = $20,600)

| Metric | Value |
|---|---|
| Accuracy | **88.3%** |
| Precision | **87.0%** |
| Recall | **88.9%** |
| F1 Score | **87.9%** |

### 5-Fold Cross-Validation

- Mean R² = **0.4495 ± 0.2383** across 5 folds

---

## 📊 Power BI Dashboard

The Power BI dashboard (`*.pbix`) includes the following visualisations:

- **Average House Price by Poverty Level** (LSTAT groups: Low=$33k → Very High=$12k)
- **Room Count vs House Price** (scatter plot)
- **River vs Non-River Tract Split** (donut chart: 93.08% non-river, 6.92% river)
- **Air Pollution vs Average House Price** (NOX bins)
- **Average Price by Highway Access** (RAD groups)

---

## 💡 Business Recommendations

1. **Prioritise Room Count (RM)** — Strongest positive predictor (coef=+1.366). High-RM areas average $5,415 more (p<0.000001).
2. **Target Low-Poverty Areas (LSTAT)** — Strongest negative predictor (coef=−2.178). Low-poverty areas average $33k vs $12k in high-poverty zones.
3. **Improve School Quality (PTRATIO)** — 3rd highest coefficient (−1.617). Areas with PTRATIO>20 show significantly lower values.
4. **Avoid High-Pollution Areas (NOX)** — Prices drop from $27k (clean zones) to $13.5k (high-NOX areas).
5. **Market River Properties as Premium (CHAS)** — Only 6.92% of tracts qualify, commanding a $2,632 premium.
6. **Deploy Model as Valuation Tool** — R²=0.7525, Accuracy=88.3%, F1=87.9% confirm it as a reliable pricing tool.

---

## 🛠️ Technologies Used

| Tool | Purpose |
|---|---|
| Python 3 | Core analysis language |
| Pandas & NumPy | Data manipulation |
| Matplotlib & Seaborn | Data visualisation |
| Scikit-learn (sklearn) | Regression modelling & evaluation |
| SciPy | Hypothesis testing (T-test, ANOVA) |
| Jupyter Notebook | Interactive development environment |
| Power BI Desktop | Business intelligence dashboard |

---

## 🚀 How to Run

1. Clone or download this repository.
2. Install dependencies:
   ```bash
   pip install pandas numpy matplotlib seaborn scikit-learn scipy jupyter
   ```
3. Open the Jupyter Notebook:
   ```bash
   jupyter notebook CCS3342_Case_Study__CIT-23-02-0173.ipynb
   ```
4. Run all cells sequentially to reproduce the full analysis.
5. Open the `.pbix` file in **Power BI Desktop** to explore the interactive dashboard.

---

## 📚 References

- Harrison, D., & Rubinfeld, D. L. (1978). Hedonic prices and the demand for clean air. *Journal of Environmental Economics and Management, 5*(1), 81–102.
- Scikit-learn documentation: https://scikit-learn.org
- Power BI documentation: https://learn.microsoft.com/en-us/power-bi/

---

