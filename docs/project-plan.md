# Readmission risk project — week-by-week plan

## What you're building

A model that predicts whether a diabetic patient will be readmitted to hospital, plus an honest account of *why* the model thinks what it thinks and *who* it works well or poorly for. Not just a notebook that ends at "accuracy: 0.81" — a small, complete simulation of what medical data science work actually involves: cleaning messy real-world clinical data, building a defensible model, and then interrogating that model before you'd ever let it near a real decision.

This is the test for whether medical data science is the master's track for you. Pay attention, week to week, to which part you look forward to versus which part you're gritting your teeth through — that's more diagnostic than how good the final model turns out to be.

## Dataset

**Primary recommendation: Diabetes 130-US Hospitals (1999–2008)**, from the UCI Machine Learning Repository — [archive.ics.uci.edu/dataset/296](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008).

Roughly 100,000 hospital encounters for diabetic patients across 130 US hospitals, with demographics (age band, sex, race), diagnoses, medications, lab results, and whether the patient was readmitted within 30 days. No credentialing or data-use agreement required — you can download it directly. It's genuinely messy (inconsistent categorical codes, missing values, multiple visits per patient) which is a feature, not a bug, for what you're trying to learn — but that also means Week 1 will take real work.

**Fallback if that feels like too much to start with: UCI Heart Disease (Cleveland)** — [archive.ics.uci.edu/dataset/45](https://archive.ics.uci.edu/dataset/45/heart+disease), mirrored cleanly on Kaggle at [kaggle.com/datasets/redwankarimsony/heart-disease-data](https://www.kaggle.com/datasets/redwankarimsony/heart-disease-data). Around 300 rows, predicting presence of heart disease from clinical measurements. Much smaller and cleaner, so Week 1 goes fast — the tradeoff is a thinner fairness audit later, since it has fewer demographic fields to slice by.

My suggestion: start with the diabetes dataset. If Week 1 turns out to be more data-wrangling pain than you want, we switch to heart disease without having lost much — the modeling and interpretability skills carry over directly.

## Tools

Google Colab, not a local Python install. It's free, runs in your browser, comes with pandas/scikit-learn/matplotlib pre-installed, and means zero environment setup — one less place for things to go wrong while you're still building coding confidence. We'll add `shap` with one `!pip install` line when Week 4 arrives.

## How we'll work together

Each week, you'll come to me with what you have — even if it's broken, half-finished, or just an error message — and I'll walk you through the next concrete step: what code to write, what a given error means, what a result is telling you. You don't need to know the "right" way to do things going in; you need to be willing to run code, look at what happens, and describe it back to me. That loop is the whole method. Paste code and errors directly rather than describing them from memory — I can debug precisely, I can't debug a paraphrase.

Treat the week numbers below as a guide, not a deadline — if a week takes you nine days instead of six, that's completely normal for a first project like this.

## Week 0 — Setup and first look (2–3 days)

Get a Colab notebook running, load the dataset, and just look at it: shape, column names, a `.head()`, a `.describe()`. No analysis yet — the goal is just to end this week with data loaded and zero errors, so Week 1 starts from solid ground.

*Deliverable:* a Colab notebook that loads the raw data and prints basic shape/summary info.

## Week 1 — Cleaning and exploratory data analysis

This is usually the least glamorous and most important week. You'll identify missing values and decide what to do with each (drop, fill, or treat "missing" as its own meaningful category), figure out which columns are actually useful versus administrative noise, and define your target variable precisely (readmitted within 30 days — yes/no). You'll also produce a handful of plots: how the target variable is distributed, how key features relate to it, whether there's obvious class imbalance.

*What you're learning:* real clinical data is never clean, and the decisions you make while cleaning it quietly shape everything downstream — this is where a lot of real-world model bias actually gets introduced, often invisibly.

*Deliverable:* a cleaned dataset ready for modeling, and 3–4 plots that tell you something about it.

## Week 2 — Baseline models

Train your first models: logistic regression and a decision tree. Set up a proper train/test split, and get comfortable with the metrics that matter for this kind of problem — accuracy alone will mislead you given the class imbalance, so precision, recall, and ROC-AUC become your real scoreboard. The goal isn't a good model yet, it's a *baseline* you can measure improvement against.

*What you're learning:* why "high accuracy" can be a meaningless or even misleading number in healthcare prediction, and how to pick metrics that match what actually matters clinically (missing a true readmission is a different kind of costly than a false alarm).

*Deliverable:* a baseline model with a clear metrics report, plus one paragraph on why you chose the metrics you did.

## Week 3 — Better models

Try a random forest and a gradient-boosted model (XGBoost or scikit-learn's `HistGradientBoostingClassifier`), add cross-validation so your evaluation isn't a fluke of one particular split, and do some light hyperparameter tuning. Pick your best-performing model and lock it in as "the model" for the rest of the project.

*What you're learning:* the practical craft of model selection and validation — the part of ML that coursework often skips past quickly but that consumes most of a real project's time.

*Deliverable:* a comparison table of models and metrics, with your final model selected and justified.

## Week 4 — Interpretability

Install `shap` and use it to understand what your chosen model is actually keying on: which features drive its predictions, in which direction, and whether that matches clinical intuition or looks suspicious. This is where the project stops being "a model that works" and starts being "a model you understand."

*What you're learning:* SHAP and feature-importance analysis, and — just as important — how to read an interpretability result critically rather than treating a nice-looking plot as automatic proof the model is trustworthy.

*Deliverable:* SHAP summary plots plus a short write-up of the three most important drivers of the model's predictions, and whether they make clinical sense to you.

## Week 5 — Fairness audit

Slice your model's performance by the demographic fields in the data — race, sex, age band — and check whether it performs consistently well across groups, or quietly worse for some. Write up what you find plainly, including if the answer is uncomfortable. This is not a formality; it's the part of the project that most directly rehearses the actual ethical stakes of deploying a model like this.

*What you're learning:* what a fairness audit concretely involves, and a grounded, specific answer (rather than an abstract one) to the question "would I trust this model in a real clinical workflow, and under what conditions?"

*Deliverable:* a subgroup performance table and a half-page write-up of where you would and wouldn't trust this model, and why.

## Week 6 — Write-up and reflection

Pull everything into a short final report: what you built, what it does well, where it breaks down, and what you'd do differently with more time or better data. Then step back from the technical content and answer, in writing, the questions below — this part is for you, not for the project.

*Deliverable:* a one-to-two-page project summary, plus your reflection notes.

## Stretch goals (optional, only if Week 6 leaves you wanting more)

Try the same pipeline on a second dataset to see how much of your process generalizes; or build a tiny Streamlit interface where you can input a hypothetical patient and see the prediction plus its top SHAP drivers — a small taste of what deploying this as an actual tool would involve.

## Reflection questions for your master's decision

When you get to Week 6, answer these honestly:

Which week did you most look forward to starting — the cleaning and modeling weeks (2–3), the interpretability week (4), or the fairness/ethics week (5)? Did the technical model-building feel like the main event with the fairness audit as an add-on, or did it feel the other way around? When you found a problem in the model (bias, a confusing SHAP result, a metric that didn't make sense), did you want to go fix the model, or did you want to go read about *why* healthcare AI has this problem more broadly? And: knowing what you know now about what a week of this work actually feels like hour to hour, does more of it sound appealing, or does a different kind of hour sound better?

There's no right answer — the point is that your honest reaction to these six weeks is better evidence for your master's decision than any amount of thinking about it in the abstract.

---

*Sources for the datasets referenced above:*
- [Diabetes 130-US Hospitals for Years 1999-2008 — UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008)
- [Heart Disease — UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/45/heart+disease)
- [UCI Heart Disease Data (clean Kaggle mirror)](https://www.kaggle.com/datasets/redwankarimsony/heart-disease-data)
