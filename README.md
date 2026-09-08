# Telecom Customer Churn & Retention Intelligence
## From Churn Prediction to Retention Decisions

> **A churn model tells you who might leave. Telecom Customer Churn & Retention Intelligence determines who is worth saving — and why.**

Telecom Customer Churn & Retention Intelligence is an **end-to-end customer retention decision-intelligence system** built on a 100,000-customer US telecom dataset. It combines statistical analysis, domain-driven feature engineering, behavioral segmentation, machine-learning benchmarking, Optuna optimization, ensemble modeling, customer-value prioritization, and cost-aware retention economics.

The project deliberately goes beyond:

```text
Customer → Churn Probability
```

and builds:

```text
Customer Data
     ↓
Statistical Evidence
     ↓
Feature Engineering
     ↓
Behavioral Segmentation
     ↓
Model Benchmarking
     ↓
Hyperparameter Optimization
     ↓
Ensemble Churn Scoring
     ↓
Customer Value
     ↓
Expected Net Value
     ↓
Retention Priority
     ↓
Actionable Intervention
     ↓
Business Impact
```

---

## 30-Second Recruiter View

| | |
|---|---|
| **Business problem** | Identify customers at risk of churn and prioritize the customers for whom retention intervention is economically worthwhile. |
| **Dataset** | 100,000 customers · 100 raw fields · `Client.csv` + `Record.csv` |
| **Data science** | EDA · statistical testing · feature engineering · KMeans · Gaussian Mixture Models |
| **Modeling** | Logistic Regression · Random Forest · XGBoost · LightGBM · CatBoost |
| **Optimization** | Optuna TPE search · 50 completed trials · early stopping |
| **Final model** | Validation-weighted ensemble of 4 models |
| **Primary metric** | PR-AUC |
| **Holdout PR-AUC** | **0.6871** |
| **Holdout recall** | **93.1%** |
| **Lift @ top 10%** | **1.60×** |
| **Decision framework** | Score → Rank → Trigger → Offer |
| **Business layer** | Expected-value ranking + 3 retention ROI scenarios |

### The key idea

The model's output is **not the final deliverable**.

A churn probability becomes useful only when it answers:

> **Who should the business contact, when should it intervene, and is that intervention worth the cost?**

Telecom Customer Churn & Retention Intelligence adds those decision layers on top of predictive modeling.

---

# Why This Project Is Different

Most churn projects end here:

```text
Customer → Churn Prediction
```

Telecom Customer Churn & Retention Intelligence continues:

```text
                  ┌──────────────────┐
                  │  CHURN PROBABILITY│
                  └────────┬─────────┘
                           ↓
                  ┌──────────────────┐
                  │ CUSTOMER VALUE   │
                  └────────┬─────────┘
                           ↓
                  ┌──────────────────┐
                  │ EXPECTED VALUE   │
                  └────────┬─────────┘
                           ↓
                  ┌──────────────────┐
                  │ ACTIONABILITY    │
                  │    TRIGGER       │
                  └────────┬─────────┘
                           ↓
                  ┌──────────────────┐
                  │ RETENTION OFFER  │
                  └────────┬─────────┘
                           ↓
                  ┌──────────────────┐
                  │ BUSINESS IMPACT  │
                  └──────────────────┘
```

This changes the project from a **classification exercise** into a **decision-support system**.

The central operating framework is:

# **SCORE → RANK → TRIGGER → OFFER**

- **SCORE** — estimate churn risk for every customer.
- **RANK** — prioritize customers by expected economic value, not risk alone.
- **TRIGGER** — combine risk, value, and an actionable lifecycle condition.
- **OFFER** — translate the priority list into a retention intervention.

---

# Business Problem

Company A is a mature US telecom operator facing customer churn in a competitive market.

The business challenge is not simply to build the most accurate classifier. A retention team has finite budget and limited capacity, so the real question is:

> **Which customers should receive an intervention before they leave, and which of those interventions are economically justified?**

The project's central business hypothesis focuses on **device lifecycle**:

> Customers with aging devices may be more likely to churn, and a subsidized upgrade can provide an actionable intervention point while creating an opportunity for contract renewal.

This hypothesis is tested statistically rather than treated as an assumption.

The resulting system combines the device-lifecycle signal with broader behavioral and economic information:

**Lifecycle + Usage + Revenue + Billing + Service Friction + Customer Value + Segmentation**

---

# Project Architecture

```text
┌──────────────────────────────────────────────────────────────┐
│                        BUSINESS CONTEXT                      │
│          Telecom churn · retention · intervention cost       │
└──────────────────────────────┬───────────────────────────────┘
                               ↓
┌──────────────────────────────────────────────────────────────┐
│                     HYPOTHESIS FORMATION                     │
│       Lifecycle · friction · value · account structure       │
└──────────────────────────────┬───────────────────────────────┘
                               ↓
┌──────────────────────────────────────────────────────────────┐
│                         DATA LAYER                           │
│       Client.csv + Record.csv → 100,000 customers            │
└──────────────────────────────┬───────────────────────────────┘
                               ↓
┌──────────────────────────────────────────────────────────────┐
│                    DATA QUALITY + EDA                        │
│      Missingness · distributions · churn patterns             │
│                  · correlation analysis                       │
└──────────────────────────────┬───────────────────────────────┘
                               ↓
┌──────────────────────────────────────────────────────────────┐
│                    STATISTICAL VALIDATION                    │
│          Two-sample KS tests across 8 numeric signals        │
└──────────────────────────────┬───────────────────────────────┘
                               ↓
┌──────────────────────────────────────────────────────────────┐
│                    FEATURE ENGINEERING                       │
│       22 engineered features across lifecycle, value,        │
│            friction, behavior, revenue, structure            │
└──────────────────────────────┬───────────────────────────────┘
                               ↓
┌──────────────────────────────────────────────────────────────┐
│                    CUSTOMER SEGMENTATION                     │
│              KMeans (k=5) + GMM (k=5)                        │
└──────────────────────────────┬───────────────────────────────┘
                               ↓
┌──────────────────────────────────────────────────────────────┐
│                     MODEL TOURNAMENT                         │
│  Logistic · RF · XGBoost · LightGBM · CatBoost · 5-fold CV  │
└──────────────────────────────┬───────────────────────────────┘
                               ↓
┌──────────────────────────────────────────────────────────────┐
│                  HYPERPARAMETER OPTIMIZATION                 │
│                 Optuna TPE · 50 trials                       │
└──────────────────────────────┬───────────────────────────────┘
                               ↓
┌──────────────────────────────────────────────────────────────┐
│                     ENSEMBLE MODEL                           │
│        Validation-weighted blend of 4 model outputs          │
└──────────────────────────────┬───────────────────────────────┘
                               ↓
┌──────────────────────────────────────────────────────────────┐
│                     HOLDOUT EVALUATION                       │
│       PR-AUC · ROC-AUC · F1 · Precision · Recall · Lift     │
└──────────────────────────────┬───────────────────────────────┘
                               ↓
┌──────────────────────────────────────────────────────────────┐
│                   DECISION INTELLIGENCE                      │
│        Risk × Value × Trigger × Intervention Economics       │
└──────────────────────────────┬───────────────────────────────┘
                               ↓
┌──────────────────────────────────────────────────────────────┐
│                    RETENTION STRATEGY                        │
│             Target customers → estimate value → act          │
└──────────────────────────────────────────────────────────────┘
```

---

# Project at a Glance

| Dimension | Implementation |
|---|---|
| **Domain** | US telecom customer churn / retention economics |
| **Customers** | 100,000 |
| **Raw fields** | 100 |
| **Target** | `churn` |
| **Churn rate** | 49.56% |
| **Engineered features** | 22 |
| **Modeling frame** | 122 columns before encoding |
| **Statistical test** | Two-sample Kolmogorov–Smirnov |
| **Numeric hypotheses tested** | 8 |
| **Segmentation** | KMeans + Gaussian Mixture |
| **Clusters/components** | 5 + 5 |
| **Model families** | 5 |
| **Cross-validation** | 5-fold stratified CV |
| **Hyperparameter optimizer** | Optuna / TPE |
| **Optuna trials** | 50 |
| **Final ensemble** | 4-model validation-weighted blend |
| **Data split** | 64% train / 16% validation / 20% test |
| **Random seed** | 42 |
| **Primary metric** | PR-AUC |
| **Final holdout PR-AUC** | **0.6871** |
| **Final holdout ROC-AUC** | **0.6975** |
| **Final holdout recall** | **93.12%** |
| **Lift @ top 10%** | **1.60×** |
| **Targeting rule** | Top decile by expected net value + `eqpdays > 350` |
| **Business scenarios** | Conservative · Base · Aggressive |

---

# The Data

The modeling dataset is created by joining two confidential source files:

```text
Client.csv
    │
    │ Customer_ID
    ▼
Record.csv
```

### `Client.csv`

Contains customer/account attributes such as:

- demographics
- household information
- account structure
- plan information
- device information

### `Record.csv`

Contains behavioral and commercial information such as:

- usage
- revenue
- recurring charges
- overage
- roaming
- customer-care interactions
- churn label

The two sources are joined on:

```text
Customer_ID
```

The resulting dataset contains:

**100,000 customers · 100 raw fields · 0 missing target values**

The churn distribution is near-balanced:

| Outcome | Customers | Share |
|---|---:|---:|
| Churned | 49,562 | 49.56% |
| Retained | 50,438 | 50.44% |

Although the classes are close to balanced, the project prioritizes **PR-AUC** because the eventual operational problem is ranking and targeting rather than maximizing generic classification accuracy.

---

# Data Quality

The raw data contains substantial missingness in several demographic and household fields.

Examples include:

| Feature | Approx. missing |
|---|---:|
| `numbcars` | 49.4% |
| `dwllsize` | 38.3% |
| `HHstatin` | 37.9% |
| `ownrent` | 33.7% |
| `dwlltype` | 31.9% |
| `lor` | 30.2% |

The pipeline does not simply delete incomplete customers.

Instead, preprocessing uses:

- median imputation for numeric features
- most-frequent imputation for categorical features
- explicit missing-indicator columns
- scaling where appropriate
- one-hot encoding for categorical variables

The objective is to preserve useful information while preventing missingness from silently removing customers from the analysis.

---

# Exploratory Data Analysis

The EDA stage investigates the project's four main business themes:

### 1. Device lifecycle

Does churn increase as the customer's current device becomes older?

### 2. Service friction

Does repeated customer-care interaction indicate increasing churn risk?

### 3. Customer value

Do revenue and account economics change the importance of a churn event?

### 4. Account structure

Do multi-line accounts demonstrate different churn behavior?

The EDA produces:

- target-balance analysis
- device-age churn analysis
- customer-care analysis
- account/phone-count analysis
- revenue analysis
- new-cell analysis
- correlation heatmap
- additional programmatically generated charts

### The key discovery

Churn increases across device-age bands:

```text
<180 days
     ↓
180–365 days
     ↓
365–730 days
     ↓
730+ days
```

The device lifecycle signal becomes particularly useful because it is not merely predictive—it is **actionable**.

Unlike historical usage or revenue, device age can potentially be reset through an upgrade intervention.

![Churn rate by device age](figures/03_eda_device_age_churn.png)

![Correlation heatmap](figures/08_correlation_heatmap.png)

---

# Hypothesis-Driven Statistical Analysis

The project uses the **two-sample Kolmogorov–Smirnov test** to compare the full distributions of selected numeric variables between churned and retained customers.

This is preferable to relying only on mean comparisons because several behavioral variables are skewed and the KS test does not require normality.

### Tested variables

- `eqpdays`
- `months`
- `totmrc_Mean`
- `mou_Mean`
- `custcare_Mean`
- `phones`
- `rev_Mean`
- `ovrmou_Mean`

### Results

| Feature | KS statistic | p-value | Interpretation |
|---|---:|---:|---|
| `eqpdays` | **0.1642** | <0.001 | Strongest separation |
| `months` | 0.1158 | <0.001 | Strong |
| `totmrc_Mean` | 0.0761 | <0.001 | Moderate |
| `mou_Mean` | 0.0542 | <0.001 | Moderate |
| `custcare_Mean` | 0.0497 | <0.001 | Modest |
| `phones` | 0.0418 | <0.001 | Modest |
| `rev_Mean` | 0.0333 | <0.001 | Weakest of tested signals |
| `ovrmou_Mean` | 0.0254 | <0.001 | Weak |

The important number is the **KS statistic**, not simply the p-value. With 100,000 observations, very small differences can become statistically significant.

### Key statistical finding

`eqpdays` — device age — has the largest distributional separation:

> **KS D = 0.1642**

This supports device lifecycle as a major signal in the project's business hypothesis.

### Important scientific caveat

These tests establish **association, not causation**.

The historical data shows that churners and non-churners have different device-age distributions. It does not prove that device aging causes churn or that an upgrade will cause a customer to stay.

A causal claim would require experimentation or an appropriate quasi-experimental design.

---

# Feature Engineering

The project adds **22 domain-driven engineered features** to the 100 raw fields, producing a 122-column modeling frame before categorical encoding.

The features are organized around business meaning rather than arbitrary transformations.

## Lifecycle Signals

```text
device_age_band
eqpdays_per_month
eqpdays_months_interaction
device_age_relative
```

These represent device maturity relative to customer tenure and the training population.

---

## Customer Value Signals

```text
customer_value_score
clv_proxy
```

`customer_value_score` combines revenue, tenure, and phone count using training-set percentile information.

`clv_proxy` is:

```text
rev_Mean × months
```

It is deliberately treated as a **proxy**, not a full customer-lifetime-value model.

---

## Service-Friction Signals

```text
custcare_per_month
custcare_intensity
device_age_customer_care
```

These capture both the frequency and accumulated intensity of customer-care interactions and their relationship with device lifecycle.

---

## Behavioral & Revenue Signals

```text
revenue_per_phone
revenue_efficiency
rev_per_mou
overage_intensity
overage_per_month
roam_ratio
roaming_per_month
overage_tenure
revenue_customer_care
satisfaction_proxy
```

These transform raw activity into interpretable customer-level ratios and interaction signals.

`satisfaction_proxy` is a heuristic feature derived from friction and overage behavior; it is not an observed satisfaction survey measure.

---

## Account Structure

```text
multi_line_flag
```

A simple structural feature identifying accounts with more than one phone line.

---

## Segmentation Features

```text
kmeans_segment
gmm_segment
```

These provide behavioral group structure that is then available to the supervised models.

---

# Leakage Prevention

A central design requirement is preventing information from validation or test data from influencing training.

The pipeline follows a split-first principle.

```text
RAW DATA
   ↓
TRAIN / VALIDATION / TEST
   ↓
Train-derived transformations
   ↓
Validation / Test transformed using fitted objects
```

### Leakage controls include

- deterministic train/validation/test splitting
- training-only percentile calculations
- training-only median reference values
- segmentation fit on training data only
- validation/test assignment using already-fitted clustering models
- preprocessing fitted on training data before transformation
- hyperparameter optimization restricted to training/validation
- ensemble weight optimization using validation probabilities
- threshold selection on validation data
- final evaluation on an untouched test set

The final test set contains **20,000 customers** and is reserved for final reporting.

---

# Customer Segmentation

Supervised churn prediction is complemented with unsupervised behavioral segmentation.

Two methods are used:

### KMeans

```text
k = 5
```

### Gaussian Mixture Model

```text
components = 5
covariance = diagonal
```

Both are fitted using standardized behavioral/value features on the training split.

The segmentation incorporates variables including:

- device age
- tenure
- customer-care usage
- overage
- roaming
- revenue
- minutes of use
- recurring charge
- phone count
- customer value
- revenue efficiency

The resulting:

```text
kmeans_segment
gmm_segment
```

are used as modeling features and also support customer-segment profiling.

This adds a second analytical perspective:

> **Who is likely to churn?**

becomes:

> **What kind of customer is likely to churn?**

---

# Model Development

Five model families are benchmarked:

1. Logistic Regression
2. Random Forest
3. XGBoost
4. LightGBM
5. CatBoost

The models are evaluated using **5-fold stratified cross-validation** and then evaluated on the final held-out test set.

## Model Tournament

| Model | Test PR-AUC | Test ROC-AUC | Test F1 | Precision | Recall |
|---|---:|---:|---:|---:|---:|
| LightGBM | 0.6822 | 0.6944 | 0.6676 | 0.6039 | 0.7463 |
| XGBoost | 0.6813 | 0.6928 | 0.6816 | 0.5730 | 0.8410 |
| CatBoost | 0.6795 | 0.6918 | 0.6859 | 0.5629 | 0.8778 |
| Random Forest | 0.6578 | 0.6750 | 0.6596 | 0.5919 | 0.7450 |
| Logistic Regression | 0.6244 | 0.6440 | 0.6704 | 0.5185 | 0.9482 |

The boosted-tree models perform closely, while the tree ensembles substantially outperform the linear baseline on the primary ranking metric.

---

# Why PR-AUC?

The project uses **PR-AUC / Average Precision** as its primary metric.

The reason is operational.

A retention campaign does not act on every customer equally. It acts on a prioritized list.

The model therefore needs to answer:

> **Can true churners consistently appear near the top of the ranking?**

PR-AUC measures ranking quality across decision thresholds and is therefore closely aligned with the eventual targeting use case.

ROC-AUC remains useful as a complementary discrimination metric.

---

# Hyperparameter Optimization

LightGBM is optimized using **Optuna** with a TPE sampler.

### Configuration

```text
Optimizer:       Optuna
Sampler:         TPESampler
Seed:            42
Trials:          50
Objective:       Validation PR-AUC
Early stopping:  Enabled
```

The search explores:

```text
n_estimators
learning_rate
num_leaves
max_depth
min_child_samples
subsample
colsample_bytree
reg_alpha
reg_lambda
```

The optimized LightGBM reaches:

```text
PR-AUC  = 0.6857
ROC-AUC = 0.6962
F1      = 0.6866
Precision = 0.5423
Recall    = 0.9354
```

This makes it the strongest individual model in the project before ensemble blending.

![Optuna optimization history](figures/18_optuna_optimization_history.png)

---

# Ensemble Modeling

Instead of relying entirely on one model, Telecom Customer Churn & Retention Intelligence combines complementary model outputs.

The final validation-weighted ensemble contains:

| Model | Weight |
|---|---:|
| Optuna LightGBM | **75%** |
| XGBoost | **10%** |
| CatBoost | **10%** |
| LightGBM | **5%** |

Weights are searched exhaustively over a discretized grid in 0.05 increments, with validation PR-AUC as the optimization objective.

Conceptually:

```text
                 Optuna LightGBM
                       75%
                        │
XGBoost 10% ────────────┤
                        ├──→ ENSEMBLE SCORE
CatBoost 10% ───────────┤
                        │
LightGBM 5% ────────────┘
```

The purpose is not to combine models for the sake of complexity.

The boosted-tree families capture overlapping but not identical customer patterns, allowing the blend to retain the strongest model while adding complementary corrections.

---

# Final Model Performance

## Champion: ValidationWeightedEnsemble

The final model is evaluated once on the **20,000-customer held-out test set**.

| Metric | Result |
|---|---:|
| **PR-AUC** | **0.687082** |
| **ROC-AUC** | **0.697498** |
| **F1-score** | **0.687549** |
| **Precision** | **54.50%** |
| **Recall** | **93.12%** |
| **Lift @ Top 10%** | **1.60×** |
| Decision threshold | **0.3084** |

### What does 93.1% recall mean?

At the selected operating threshold, the system identifies approximately **93% of observed churners in the held-out test set**.

The trade-off is precision:

> roughly 54.5% of customers flagged at the operating threshold are observed churners.

For a retention system where missing a genuine churn opportunity can be more costly than contacting a customer who would have stayed anyway, the project intentionally favors recall.

![Precision-recall curve](figures/09_model_precision_recall.png)

![ROC curve](figures/10_model_roc_curve.png)

![Confusion matrix](figures/11_confusion_matrix.png)

![Lift chart](figures/12_lift_chart.png)

---

# Model Interpretation

The final ensemble is dominated by Optuna LightGBM, so its gain-based feature importance provides a useful view into the model's internal signal hierarchy.

### Top features from the executed model

| Rank | Feature | Signal type |
|---|---|---|
| 1 | `change_mou` | Usage trend |
| 2 | `change_rev` | Revenue trend |
| 3 | `totmrc_Mean` | Billing |
| 4 | `eqpdays_per_month` | Lifecycle |
| 5 | `revenue_per_phone` | Behavioral |
| 6 | `avgrev` | Revenue |
| 7 | `mouowylisv_Mean` | Usage |
| 8 | `avgqty` | Usage |
| 9 | `avgmou` | Usage |
| 10 | `mouiwylisv_Mean` | Usage |
| 11 | `rev_per_mou` | Behavioral |
| 12 | `mou_Mean` | Usage |
| 13 | `unan_vce_Mean` | Usage |
| 14 | `eqpdays_months_interaction` | Lifecycle |
| 15 | `mou_cvce_Mean` | Usage |

This is an important distinction:

**Device lifecycle is the project's actionable business hypothesis, but the predictive model learns from a much broader set of behavioral, revenue, billing, and lifecycle signals.**

The statistical analysis independently identifies `eqpdays` as the strongest distribution-separating numeric variable, while the final tree model also incorporates usage and revenue trends heavily.

![Feature importance](figures/19_feature_importance.png)

---

# The Device Lifecycle Discovery

The project centers on a particularly actionable pattern:

```text
Device age
    ↓
Increasing churn association
    ↓
Observed inflection around older devices
    ↓
Define an actionable intervention window
    ↓
Offer upgrade / renewal
```

The campaign trigger is:

```text
eqpdays > 350
```

This threshold is designed to identify customers entering an actionable device-lifecycle window rather than waiting until the device is already substantially older.

The important scientific distinction remains:

> **Device age is an actionable predictive signal, not proven causal treatment evidence.**

The project therefore treats the upgrade program as a business hypothesis to be tested, not as a guaranteed causal solution.

![Device age vs churn](figures/03_eda_device_age_churn.png)

---

# From Model to Decision

This is the core of Telecom Customer Churn & Retention Intelligence.

## 1. SCORE

Score the customer base with the final ensemble.

```text
customer → p_churn
```

The system produces a risk score for each customer.

---

## 2. RANK

Risk alone is insufficient.

A customer with a high probability of churn may not be the best retention target if the potential economic value is low.

The project therefore ranks customers using:

```text
Expected Net Value
```

The core calculation is:

```text
expected_preserved_value
=
p_churn
× acceptance_rate
× retained_months
× monthly_revenue
× margin

expected_net_value
=
expected_preserved_value
− campaign_cost
```

This converts:

```text
"How likely are they to churn?"
```

into:

```text
"How much expected economic value is associated with retaining them?"
```

---

## 3. TRIGGER

The retention campaign combines economic priority with the actionable device lifecycle signal.

A customer is targeted when:

```text
eqpdays > 350
AND
customer falls within the top decile by expected net value
```

This creates a two-dimensional decision rule:

```text
          HIGH CUSTOMER VALUE
                  ▲
                  │
            TARGET ZONE
                  │
                  │
                  │
LOW RISK ─────────┼───────── HIGH RISK
                  │
                  │
                  ▼
          LOW CUSTOMER VALUE
```

The system is therefore designed to prioritize:

**high-risk + high-value + actionable customers**

rather than simply selecting the highest churn probabilities.

![Retention priority](figures/17_priority_scatter.png)

---

# 4. OFFER

The modeled intervention is:

> **A subsidized device upgrade paired with an 18-month contract renewal.**

The business case tests the intervention under three scenarios rather than presenting a single artificially precise financial estimate.

---

# Expected-Value Framework

The model combines observed customer information with explicit business assumptions.

### Observed from the dataset

- `p_churn`
- `rev_Mean`
- customer ranking
- behavioral characteristics
- device lifecycle

### Assumed for scenario planning

- acceptance rate
- retained-month horizon
- subsidy + marketing cost
- gross-margin multiplier

| Variable | Conservative | Base | Aggressive |
|---|---:|---:|---:|
| Acceptance rate | 20% | **30%** | 40% |
| Retained months | 12 | **18** | 24 |
| Cost / customer | $45 | **$65** | $85 |
| Gross-margin multiplier | 0.80 | **1.00** | 1.10 |

These assumptions are explicitly separated from observed data.

---

# Modeled Business Impact

> ⚠️ **These are modeled scenario estimates, not realized production results.**

The calculation is applied to the held-out test population.

The top 10% by expected net value are targeted, corresponding to **2,000 customers** in the 20,000-customer test set.

| Scenario | Acceptance | Retained months | Cost / customer | Campaign cost | Modeled net value | Modeled ROI |
|---|---:|---:|---:|---:|---:|---:|
| Conservative | 20% | 12 | $45 | $90,000 | $235,406 | 2.62× |
| **Base ★** | **30%** | **18** | **$65** | **$130,000** | **$785,205** | **6.04×** |
| Aggressive | 40% | 24 | $85 | $170,000 | $1,619,735 | 9.53× |

### Base scenario

```text
2,000 targeted customers
        ↓
$130K campaign cost
        ↓
$785K modeled net value
        ↓
6.04× modeled ROI
```

These figures should be interpreted as **decision-model outputs**, not realized revenue.

A live campaign, treatment-control experiment, or validated causal model would be required to estimate incremental financial impact.

![Business ROI](figures/16_business_roi_waterfall.png)

---

# Why the Business Layer Matters

The same churn model can produce very different business decisions depending on how its predictions are consumed.

Consider two customers:

```text
Customer A
High churn probability
Low revenue
Low expected retention value

Customer B
Slightly lower churn probability
High revenue
High expected retention value
```

A probability-only system may prioritize Customer A.

A decision system can prioritize Customer B if:

```text
Expected Value(Customer B)
>
Expected Value(Customer A)
```

That is the central transformation:

> **Prediction tells you what may happen. Decision intelligence helps determine what to do about it.**

---

# Risk & Production Thinking

The repository does not pretend that a notebook is a production system.

Several production risks are explicitly identified.

| Risk | Why it matters | Proposed direction |
|---|---|---|
| **Cannibalization** | Some customers would renew without an intervention | A/B testing + uplift modeling |
| **Offer fatigue** | Repeated offers can reduce acceptance | 90-day suppression window |
| **Model drift** | Device and customer behavior can change over time | Rolling retraining + monitoring |
| **Data pipeline risk** | Incorrect or delayed lifecycle data can affect triggers | Automated data-quality alerts |

These controls are **proposed production extensions**, not implemented features.

That distinction is intentional.

---

# Limitations

A strong Data Science system should clearly state what it does **not** prove.

### 1. Modeled, not realized, financial impact

ROI estimates come from applying a business formula to historical holdout data under explicit assumptions.

They are not observed campaign revenue.

### 2. No causal treatment effect

The analysis shows associations between customer characteristics and churn.

It does not establish that an upgrade will cause a customer to remain.

### 3. No live A/B experiment

A randomized experiment is required to estimate the incremental retention effect of the intervention.

### 4. Probability calibration

The model is optimized for ranking and threshold performance. Its raw scores have not undergone a dedicated calibration procedure and should not automatically be interpreted as perfectly calibrated probabilities.

### 5. Deployment

The repository contains a notebook-based analytical pipeline rather than a production API, scheduled scoring service, or monitoring platform.

### 6. Dataset generalization

The analysis is based on a single historical telecom dataset. Findings should be revalidated before being transferred to another operator, market, plan mix, or device ecosystem.

---

# Future Roadmap

Telecom Customer Churn & Retention Intelligence already covers the major stages of an end-to-end Data Science workflow. The next step is to move from predictive decision support toward measurable intervention optimization.

## Phase 1 — Productionization

```text
Notebook
   ↓
Modular pipeline
   ↓
Scheduled batch scoring
   ↓
CRM / campaign integration
```

## Phase 2 — Model reliability

- probability calibration
- automated data-quality checks
- drift monitoring
- scheduled retraining
- model/version tracking

## Phase 3 — Causal measurement

Run a randomized retention experiment:

```text
Eligible customers
       │
       ├──────────────┐
       ↓              ↓
 Treatment         Control
       │              │
       ↓              ↓
 Upgrade offer     No offer
       │              │
       └──────┬───────┘
              ↓
       Compare retention
```

This would transform:

> **association**

into an estimate of:

> **incremental retention effect**

## Phase 4 — Uplift Modeling

The next-generation model should answer:

```text
Who will churn?
        +
Who will respond to an intervention?
        ↓
Who should actually receive the intervention?
```

This directly addresses cannibalization and moves the system from:

**churn prediction**

toward:

**treatment optimization**.

---

# End-to-End Data Science Scope

Telecom Customer Churn & Retention Intelligence demonstrates the following capabilities in one integrated workflow:

### Data Engineering
- Multi-source customer-level data integration
- Key validation
- Data-quality auditing
- Missing-data treatment

### Statistics
- Hypothesis-driven analysis
- Distribution comparison
- Kolmogorov–Smirnov testing
- Effect-size interpretation

### Exploratory Data Analysis
- Churn segmentation
- Device lifecycle analysis
- Revenue analysis
- Customer-care behavior
- Account structure
- Correlation analysis

### Feature Engineering
- Lifecycle features
- Revenue ratios
- Behavioral features
- Service-friction features
- Customer-value features
- Interaction terms
- Training-derived transformations

### Unsupervised Learning
- KMeans clustering
- Gaussian Mixture Models
- Behavioral segment profiling

### Supervised Learning
- Logistic Regression
- Random Forest
- XGBoost
- LightGBM
- CatBoost

### Model Optimization
- Optuna
- TPE sampling
- Early stopping
- Hyperparameter search

### Ensemble Learning
- Validation-weighted blending
- Grid-search weight optimization

### Model Evaluation
- PR-AUC
- ROC-AUC
- F1
- Precision
- Recall
- Lift
- Confusion matrix
- Precision-recall curves
- ROC curves

### Model Interpretation
- Gain-based feature importance
- Business interpretation of predictive signals

### Decision Science
- Customer prioritization
- Expected-value ranking
- Actionability triggers

### Business Analytics
- Campaign economics
- Cost assumptions
- Retained-value scenarios
- ROI modeling

### Production Thinking
- Model drift
- Data-quality monitoring
- Suppression logic
- Calibration
- A/B testing
- Uplift modeling

---

# Technology Stack

### Language

**Python 3.11**

### Data

- pandas
- NumPy
- PyArrow

### Statistics

- SciPy

### Machine Learning

- scikit-learn
- LightGBM
- XGBoost
- CatBoost

### Optimization

- Optuna

### Visualization

- Matplotlib
- Seaborn

### Model Persistence

- Joblib

### Compute

The notebook was executed in Google Colab using GPU-enabled builds of:

- LightGBM
- XGBoost
- CatBoost

CPU-only environments are also supported by the same libraries, although training may be slower.

---

# Repository Structure

```text
telecom-customer-churn-retention-intelligence/
│
├── README.md
├── LICENSE
├── CITATION.cff
├── requirements.txt
├── .gitignore
│
├── notebooks/
│   └── project_horizon.ipynb
│
├── reports/
│   ├── project_summary.pdf
│   └── final_presentation.pdf
│
├── docs/
│   ├── hypotheses.md
│   ├── methodology.md
│   └── business-case.md
│
└── figures/
    ├── 01_missingness_profile.png
    ├── 02_target_balance.png
    ├── 03_eda_device_age_churn.png
    ├── 04_eda_new_cell_churn.png
    ├── 05_eda_custcare_churn.png
    ├── 06_eda_phones_churn.png
    ├── 07_eda_revenue_churn.png
    ├── 08_correlation_heatmap.png
    ├── 09_model_precision_recall.png
    ├── 10_model_roc_curve.png
    ├── 11_confusion_matrix.png
    ├── 12_lift_chart.png
    ├── 16_business_roi_waterfall.png
    ├── 17_priority_scatter.png
    ├── 18_optuna_optimization_history.png
    └── 19_feature_importance.png
```

### Where to start

**For the full implementation:**  
`notebooks/project_horizon.ipynb`

**For methodology:**  
`docs/methodology.md`

**For hypotheses:**  
`docs/hypotheses.md`

**For the business case:**  
`docs/business-case.md`

**For executive communication:**  
`reports/final_presentation.pdf`

---

# Reproducibility

The main analytical workflow is contained in:

```text
notebooks/project_horizon.ipynb
```

### Reproducibility configuration

```text
Random seed: 42
Split:       64% train / 16% validation / 20% test
```

The seed is applied to the relevant random processes and model configurations.

### Required data

Because the underlying telecom dataset is confidential, the raw CSVs are not included.

To reproduce the notebook locally, the expected structure is:

```text
telecom-customer-churn-retention-intelligence/
│
├── telecom/
│   ├── Client.csv
│   └── Record.csv
│
└── notebooks/
    └── project_horizon.ipynb
```

The notebook can then discover the two source files and execute the pipeline.

### GPU vs CPU

The original execution used GPU-enabled versions of the boosting libraries.

A compatible CPU environment can run the workflow as well, although:

- training will take longer
- floating-point behavior can vary slightly
- optimization paths can differ across environments

Because the dataset is confidential and cannot be redistributed, **full external reproduction of the exact numerical run is not guaranteed without access to the original data**.

---

# Data Privacy

The raw customer data is confidential and is intentionally excluded from this public repository.

### Do not commit

```text
Client.csv
Record.csv
```

or any derivative file containing customer-level records.

This includes row-level exports such as:

```text
top_risk_customers.csv
```

### Safe repository contents

The public repository can contain:

- source code
- notebook logic
- feature definitions
- aggregated tables
- charts
- documentation
- reports
- presentation materials

The objective is to demonstrate the complete analytical methodology without exposing individual customer information.

---

# Results & Versioning Note

The project contains supporting deliverables generated from different executions of the analytical pipeline.

For the public-facing repository, the **executed notebook outputs are treated as the computational source of truth** for the headline metrics and business scenario shown in this README.

The authoritative current run is:

```text
Champion:
ValidationWeightedEnsemble

PR-AUC:
0.687082

ROC-AUC:
0.697498

Recall:
93.12%

Ensemble:
75% Optuna LightGBM
10% XGBoost
10% CatBoost
5% LightGBM

Base modeled net value:
$785,205

Base modeled ROI:
6.04×
```

Earlier project artifacts may contain different values because they represent previous executions of the pipeline.

This distinction is documented intentionally rather than silently mixing results from different runs.

For detailed methodology and business assumptions, see:

```text
docs/methodology.md
docs/business-case.md
```

---

# References

### Algorithms & Libraries

- Ke et al. (2017), **LightGBM: A Highly Efficient Gradient Boosting Decision Tree**
- Chen & Guestrin (2016), **XGBoost: A Scalable Tree Boosting System**
- Prokhorenkova et al. (2018), **CatBoost: Unbiased Boosting with Categorical Features**
- Akiba et al. (2019), **Optuna: A Next-generation Hyperparameter Optimization Framework**
- Pedregosa et al. (2011), **Scikit-learn: Machine Learning in Python**
- Harris et al. (2020), **Array programming with NumPy**
- McKinney (2010), **Data Structures for Statistical Computing in Python**

### Market Context

- GSMA Intelligence (2025), *The Mobile Economy 2025*
- Bain & Company (2024), *Telecom Retention Economics*

### Dataset

GCI World (2026), Company A dataset:

```text
Client.csv
Record.csv
```

Confidential and not redistributed.

---

# License

The code, documentation, and analysis in this repository are released under the **MIT License**.

See:

```text
LICENSE
```

The underlying customer dataset is confidential and is **not covered by the public repository license**.

---

# Citation

If you reference Telecom Customer Churn & Retention Intelligence, see:

```text
CITATION.cff
```

---

# Final Takeaway

Telecom Customer Churn & Retention Intelligence was built around a simple idea:

> **Predicting churn is useful. Knowing whom to save, when to act, and whether the intervention is worth paying for is more useful.**

The project connects the full chain:

```text
BUSINESS QUESTION
       ↓
HYPOTHESIS
       ↓
DATA
       ↓
STATISTICS
       ↓
EDA
       ↓
FEATURE ENGINEERING
       ↓
SEGMENTATION
       ↓
MODEL BENCHMARKING
       ↓
OPTUNA
       ↓
ENSEMBLE
       ↓
HOLDOUT EVALUATION
       ↓
INTERPRETATION
       ↓
RISK × VALUE
       ↓
RETENTION DECISION
       ↓
ECONOMIC IMPACT
```

That is the scope of Telecom Customer Churn & Retention Intelligence:

**not just predicting who leaves — but building the analytical machinery required to decide who is worth saving.**

---

*Built as a final Data Science project for GCI World 2026.*
