# Readmission Risk Project

I'm exploring whether machine learning applied to real clinical data is something I want to build a career around, coming from a background in Cognitive Science with a specialization in machine learning and statistics. Rather than stopping at "the model works," the point here was to build something end-to-end: clean real (messy) hospital data, train and compare several models, understand *why* the best one makes the predictions it makes, and check whether it actually works fairly across different patient groups — the parts of the job that matter most before anything like this could touch real patient care.

The model itself predicts whether a diabetic patient will be readmitted to hospital within 30 days of discharge.

Full week-by-week plan and reasoning: [`docs/project-plan.md`](docs/project-plan.md).

## Dataset

[Diabetes 130-US Hospitals for Years 1999–2008](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008) (UCI Machine Learning Repository). Not committed to this repo — download it fresh from the link above into `data/` (already git-ignored).

## Getting started

Everything runs in Jupyter notebooks via VS Code, using a local Python virtual environment:

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

Notebooks live in `notebooks/`, one per stage of the project (`week0_setup.ipynb` through `week5_fairness_audit.ipynb`), each runnable on its own from a fresh kernel.

## Key findings

**Model performance:** logistic regression, random forest, and gradient boosting all landed within ~0.005 ROC-AUC of each other (~0.64), which suggests this feature set has a hard ceiling that model complexity alone can't push past — a good early lesson that better features usually beat fancier algorithms. Gradient boosting was selected as the final model (5-fold CV ROC-AUC: 0.644).

**What drives the model's predictions (via SHAP):** prior hospitalization history (`number_inpatient`) is by far the strongest signal, followed by discharge destination — patients discharged to a skilled nursing facility or rehab center are predicted at meaningfully higher risk than those discharged home. Both line up with real clinical intuition.

**Fairness audit:** the model shows a real, well-powered gap by gender — it catches about 54% of true readmissions among women but only 48% among men — and an even larger gap by age, catching 60-68% of true readmissions among patients over 70 but only 28-31% among patients 30-60, a difference too large to be explained by the modest gap in base readmission rates alone. Across race, the two largest groups (Caucasian, African American) performed similarly; results for smaller race groups weren't reliable enough to draw conclusions from given limited sample sizes. **Bottom line: this model would need real fixes — not just fairness reporting — before it could be trusted for equitable clinical use, since it currently under-flags real risk for men and for middle-aged patients specifically.**

## License

Personal learning project — no license applied.