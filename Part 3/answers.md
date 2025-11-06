## Part 3: Critical Thinking 

### Ethics & Bias 

- **Impact of biased training data on patient outcomes**:
  - Underestimation of risk for certain subgroups (e.g., minorities, uninsured) can lead to fewer follow-ups and higher adverse events.
  - Overestimation for others can waste limited care-management resources and cause alert fatigue.
  - Reinforces historical disparities, undermining trust and potentially increasing legal/compliance risk.
- **One mitigation strategy**:
  - Reweight training samples to balance subgroups and apply subgroup calibration (e.g., equalized calibration error), with routine fairness audits across protected attributes.

### Trade-offs 

- **Interpretability vs. accuracy in healthcare**:
  - Simpler models (logistic regression, shallow trees) are easier to explain, support clinician trust, and ease regulatory review but may miss complex patterns.
  - More complex models (boosted trees, deep nets) can improve discrimination but reduce transparency; require explainability tooling, governance, and careful validation.
  - Practical approach: start with interpretable baselines; if complex models add clinically meaningful lift, pair with explanation (SHAP), calibration, and decision thresholds aligned to workflows.
- **Limited computational resources: impact on model choice**:
  - Favor lightweight models (regularized logistic regression, small gradient-boosted ensembles) and compact feature sets to reduce CPU/RAM and latency.
  - Prefer batch or scheduled inference over real-time when feasible; use quantization and early stopping; avoid large deep nets unless served via efficient external inference endpoints.


