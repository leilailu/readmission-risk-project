# Final Report: Readmission Risk Project

## What I built

A model predicting whether a diabetic patient will be readmitted to hospital within 30 days of discharge, using the UCI Diabetes 130-US Hospitals dataset (~70,000 patients after cleaning). Beyond training a model, the project focused on three things that matter more in real healthcare ML than raw accuracy: understanding *why* the model makes the predictions it makes, checking whether it performs fairly across patient subgroups, and being honest about where it falls short.

## Results

Four models were compared: logistic regression, a decision tree, random forest, and gradient boosting. All but the decision tree landed within ~0.005 ROC-AUC of each other (~0.64), meaning added model complexity barely improved performance — the ceiling here is set by the available features, not by algorithm sophistication. Gradient boosting was selected as the final model (5-fold cross-validated ROC-AUC: 0.644).

## What drives the model (SHAP)

The strongest predictor by far is `number_inpatient` — a patient's history of prior hospitalizations. Discharge destination is the next strongest signal: patients discharged to a skilled nursing facility or rehab center are predicted at meaningfully higher risk than those discharged home. Both align with real clinical intuition.

## Fairness audit

The model shows two real, statistically well-powered disparities: it catches only 48% of true readmissions among men versus 54% among women, and catches just 28–31% of true readmissions among patients aged 30–60 versus 60–68% among patients over 70, a gap too large to be explained by the modest difference in base readmission rates between these groups. Across race, the two largest groups (Caucasian, African American) performed comparably; results for smaller race groups were too small a sample to draw reliable conclusions from.

## Limitations and what I'd do differently

The feature set was deliberately limited (a curated subset of ~20 columns rather than all 47, and the ~20 individual medication columns and free-text diagnosis codes were left out entirely) — richer feature engineering, especially from the diagnosis codes, might raise the performance ceiling that plateaued across all four models. The fairness gaps identified were described but not fixed — a next step would be investigating *why* they exist (missing features relevant to these groups? different clinical presentation patterns for men or middle-aged patients?) and testing whether group-specific decision thresholds close the gap without sacrificing overall performance. The dataset itself is also over 15 years old (1999–2008) and drawn from US hospitals specifically, so conclusions may not generalize to current or non-US clinical settings.

## Bottom line

The model demonstrates a real, working pipeline from raw clinical data to an interpretable, audited prediction, but in its current form, it would not be ready for equitable real-world deployment without addressing the gender and age-based gaps identified in the fairness audit.