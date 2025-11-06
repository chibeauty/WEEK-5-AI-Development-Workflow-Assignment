## 1. Problem Definition 

- **Problem**: Predicting student dropout risk within the next academic term.
- **Objectives (3)**:
  - Identify at-risk students early to enable timely interventions.
  - Prioritize outreach resources based on predicted risk levels.
  - Improve overall term-to-term retention rate.
- **Stakeholders (2)**:
  - Student success/retention office.
  - Academic advisors and program coordinators.
- **KPI (1)**:
  - Term-to-term retention uplift (% increase vs. baseline control group).

## 2. Data Collection & Preprocessing 

- **Data Sources (2)**:
  - Learning Management System (LMS) activity logs (logins, assignment submissions, forum activity).
  - Student Information System (SIS) records (demographics, prior GPA, credits attempted/earned, financial status).
- **Potential Bias (1)**:
  - Sampling bias: part-time/online students may be underrepresented or behave differently in LMS activity, skewing risk estimates.
- **Preprocessing Steps (3)**:
  - Handle missing data via strategy per feature (e.g., median for numeric, explicit "unknown" category for categorical).
  - Normalize/standardize continuous variables and winsorize extreme outliers.
  - Aggregate temporal LMS events into weekly features (e.g., submissions/week) and align to a common observation window.

## 3. Model Development 

- **Chosen Model + Justification**:
  - Gradient Boosted Trees (e.g., XGBoost/LightGBM): strong performance on tabular, heterogeneous features; handles non-linearities and feature interactions; robust to missing values; offers feature importance for interpretability.
- **Data Split Strategy**:
  - Time-aware split to prevent leakage: train on earlier terms, validate on the next term, test on a held-out most recent term (e.g., 70%/15%/15%), with stratification by dropout label if class imbalance is high.
- **Hyperparameters to Tune (2) + Why**:
  - max_depth / num_leaves: controls model complexity and overfitting.
  - learning_rate: balances step size vs. number of trees, affecting generalization and convergence.

## 4. Evaluation & Deployment 

- **Evaluation Metrics (2) + Relevance**:
  - AUC-PR: better reflects performance on imbalanced dropout labels; emphasizes precision/recall trade-off.
  - Recall@TopK (e.g., top 10% highest risk): measures how many actual dropouts are captured within limited intervention capacity.
- **Concept Drift + Monitoring**:
  - Concept drift: the relationship between features and dropout changes over time (e.g., policy or curriculum changes). Monitor via population stability index (PSI) on key features/scores, drift detectors on prediction residuals, and periodic backtesting; set triggers for retraining/calibration.
- **Deployment Challenge (1)**:
  - Scalability for near-real-time scoring as LMS events stream in; address with message queues/stream processing (e.g., Kafka), stateless inference services with autoscaling, and batched feature computation to keep latency low.



