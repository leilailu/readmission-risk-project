# Readmission Risk Project

A machine learning model that predicts whether a diabetic patient will be readmitted to hospital — built with an interpretability analysis (what is the model actually keying on?) and a fairness audit (does it work equally well across patient subgroups?) rather than stopping at a single accuracy number.

This is a personal gap-year project exploring medical data science as a possible master's direction, following the plan in [`docs/project-plan.md`](docs/project-plan.md).

## Dataset

[Diabetes 130-US Hospitals for Years 1999–2008](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008) (UCI Machine Learning Repository). Not committed to this repo — download it fresh from the link above into `data/` (already git-ignored).

Fallback dataset if needed: [UCI Heart Disease (Cleveland)](https://archive.ics.uci.edu/dataset/45/heart+disease).

## Getting started

Work happens in [Google Colab](https://colab.research.google.com/) — no local Python setup required. Open a new Colab notebook, upload the dataset, and go. Export finished notebooks into `notebooks/` in this repo to keep a record as you go (File → Download → Download .ipynb in Colab, then drop the file into this folder).

If you'd rather run things locally instead of Colab:

```bash
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook
```

## Progress

- [ ] Week 0 — Setup, data loaded
- [ ] Week 1 — Cleaning + exploratory data analysis
- [ ] Week 2 — Baseline models (logistic regression, decision tree)
- [ ] Week 3 — Better models (random forest, gradient boosting) + cross-validation
- [ ] Week 4 — Interpretability (SHAP)
- [ ] Week 5 — Fairness / subgroup audit
- [ ] Week 6 — Write-up + reflection

See [`docs/project-plan.md`](docs/project-plan.md) for the full week-by-week plan, including what each week is for and what it should produce.

## License

Personal learning project — no license applied.
