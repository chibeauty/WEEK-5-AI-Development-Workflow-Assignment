## Part 4: Reflection & Workflow Diagram (10 points)

### Reflection (5 points)

- **Most challenging part and why**:
  - Ensuring fairness while preserving clinical utility. Clinical data are heterogeneous and imbalanced; mitigating bias without sacrificing sensitivity for high-risk patients requires careful thresholding, calibration, and subgroup validation.
- **Improvements with more time/resources**:
  - Expand data coverage (e.g., SDOH, post-discharge follow-ups), establish automated fairness monitoring with subgroup calibration, and run prospective A/B pilots integrated into clinical workflows to refine thresholds and alerts.

### Diagram (5 points)

AI Development Workflow (flowchart):

```
Problem Definition
        |
        v
Data Collection -----> Governance & Privacy Checks
        |                         |
        v                         v
Data Preprocessing & Feature Engineering
        |
        v
Model Development (training, tuning)
        |
        v
Evaluation (metrics, calibration, fairness)
        |
        v
Deployment (serving, integration)
        |
        v
Monitoring (performance, drift, fairness) ---+
        ^                                      |
        |--------------------------------------+
```


