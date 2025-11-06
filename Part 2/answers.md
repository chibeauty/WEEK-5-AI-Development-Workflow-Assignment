## Part 2: Case Study Application 

### Scenario
A hospital wants an AI system to predict patient readmission risk within 30 days of discharge.

---

## 1) Problem Scope 

- **Problem**: Predict the likelihood that a discharged patient will be readmitted within 30 days.
- **Objectives**:
  - Prioritize high-risk patients for post-discharge follow-up and care coordination.
  - Reduce 30-day readmission rates and associated costs/penalties.
  - Support clinicians with risk-informed decision-making at discharge.
- **Stakeholders**:
  - Clinicians (hospitalists, nurses, discharge planners).
  - Care management and quality improvement teams.
  - Hospital administration and compliance officers.

---

## 2) Data Strategy 

- **Proposed Data Sources**:
  - Electronic Health Records (EHR): diagnoses (ICD), procedures (CPT), problem lists, vitals, labs, medications, allergies, length of stay, discharge summaries.
  - Demographics and social determinants: age, sex, language, insurance, ZIP-level deprivation indices.
  - Utilization history: prior admissions/ED visits, outpatient follow-ups, no-shows.
  - Care coordination data: discharge disposition, home health orders, follow-up appointment scheduling.
- **Ethical Concerns (2)**:
  - Patient privacy and data security: risk of re-identification if data are mishandled.
  - Fairness: model may encode historical disparities leading to biased risk scores for protected groups.
- **Preprocessing Pipeline (incl. feature engineering)**:
  - Data cleaning: deduplicate encounters; harmonize code systems; handle missing labs/vitals (e.g., clinically informed imputation or missingness indicators).
  - Encoding: one-hot or target encoding for high-cardinality codes (group ICD by CCS categories); text summarization from discharge notes using clinical concept extraction (e.g., counts of conditions, sentiment/negation flags).
  - Scaling: standardize continuous features (labs, vitals); winsorize outliers.
  - Temporal aggregation: compute lookback-window features (e.g., 6-month counts of admissions, ED visits, total LOS).
  - Social risk features: area-level indices (deprivation score), transportation access proxies.
  - Labeling and leakage control: define readmission within 30 days post-index discharge; exclude post-discharge data from features.

---

## 3) Model Development 

- **Selected Model & Justification**:
  - Gradient Boosted Trees (e.g., XGBoost/LightGBM): strong performance on tabular clinical data, handles non-linearities and mixed feature types, robust to missingness, offers feature importance and supports calibrated probabilities.
- **Confusion Matrix (hypothetical, threshold chosen to balance precision/recall)**:

  |                | Predicted Readmit | Predicted No Readmit |
  |----------------|-------------------|----------------------|
  | Actual Readmit | TP = 180          | FN = 120             |
  | Actual No      | FP = 70           | TN = 630             |

  - **Precision** = TP / (TP + FP) = 180 / (180 + 70) = 0.72
  - **Recall** = TP / (TP + FN) = 180 / (180 + 120) = 0.60

---

## 4) Deployment 

- **Integration Steps**:
  - Feature pipeline: nightly (batch) and on-demand (near real-time) feature computation from EHR via HL7/FHIR interfaces.
  - Model service: containerized inference API with authentication; expose risk score and key contributing factors.
  - EHR integration: embed risk score into discharge workflow (e.g., summary view, best-practice alerts) with audit logging.
  - Monitoring: track data drift, calibration, latency, and clinical outcomes; implement alerting and rollback mechanisms.
  - MLOps: version models/artifacts, automate retraining and A/B evaluation with champion–challenger.
- **Regulatory Compliance (e.g., HIPAA)**:
  - Enforce least-privilege access, RBAC, and multi-factor authentication; maintain audit trails.
  - Encrypt PHI at rest and in transit; secure key management (HSM/KMS).
  - Execute BAAs with vendors; perform risk assessments; maintain incident response plans.
  - Data minimization and de-identification for secondary use; comply with patient consent and disclosure policies.

---

## 5) Optimization 

- **Method to Address Overfitting**:
  - Early stopping with time-aware validation (train on earlier encounters, validate on later periods) to halt boosting when generalization degrades; combine with L2 regularization and tree depth limits.



