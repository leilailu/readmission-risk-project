# My readmission risk project — week-by-week plan

## What I'm building

A model that predicts whether a diabetic patient will be readmitted to hospital, plus an honest account of *why* the model thinks what it thinks and *who* it works well or poorly for. Not just a notebook that ends at "accuracy: 0.81" — a small, complete simulation of what medical data science work actually involves: cleaning messy real-world clinical data, building a defensible model, and then interrogating that model before I'd ever let it near a real decision.

This is my test for whether medical data science is the master's track for me. I'm paying attention, week to week, to which part I look forward to versus which part I'm gritting my teeth through — that's more diagnostic than how good the final model turns out to be.

## Dataset

**Primary choice: Diabetes 130-US Hospitals (1999–2008)**, from the UCI Machine Learning Repository — [archive.ics.uci.edu/dataset/296](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008).

Roughly 100,000 hospital encounters for diabetic patients across 130 US hospitals, with demographics (age band, sex, race), diagnoses, medications, lab results, and whether the patient was readmitted within 30 days. No credentialing or data-use agreement required — I can download it directly. It's genuinely messy (inconsistent categorical codes, missing values, multiple visits per patient) which is a feature, not a bug, for what I'm trying to learn — but that also means Week 1 will take real work.

**Fallback if that feels like too much to start with: UCI Heart Disease (Cleveland)** — [archive.ics.uci.edu/dataset/45](https://archive.ics.uci.edu/dataset/45/heart+disease), mirrored cleanly on Kaggle at [kaggle.com/datasets/redwankarimsony/heart-disease-data](https://www.kaggle.com/datasets/redwankarimsony/heart-disease-data). Around 300 rows, predicting presence of heart disease from clinical measurements. Much smaller and cleaner, so Week 1 goes fast — the tradeoff is a thinner fairness audit later, since it has fewer demographic fields to slice by.

My plan: start with the diabetes dataset. If Week 1 turns out to be more data-wrangling pain than I want, I'll switch to heart disease without losing much — the modeling and interpretability skills carry over directly.

## Tools

VS Code with a local Python virtual environment. Originally planned to use Google Colab, but switched to VS Code early in Week 0 since I was already familiar with it — no meaningful downside, and it meant working directly inside the same project folder as everything else. Set up with:

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

`requirements.txt` already includes everything needed for the whole project, including `shap` for Week 4 — no extra installs required partway through.

## My approach

Each week, I came to my AI assistant with whatever I had — even if it was broken, half-finished, or just an error message — and got walked through the next concrete step: what code to write, what a given error meant, what a result was telling me. I didn't need to know the "right" way to do things going in; I needed to be willing to run code, look at what happened, and describe it back. That loop was the whole method — pasting code and errors directly, rather than describing them from memory, made debugging far more precise.

I treated the week numbers as a guide, not a deadline, some weeks took longer than others, which is completely normal for a first project like this.

## Week 0 — Setup and first look (2–3 days)

## Week 0 — Setup and first look (2–3 days)

Get a notebook running in VS Code (with a local Python virtual environment), load the dataset, and just look at it: shape, column names, a `.head()`, a `.describe()`. No analysis yet — the goal was just to end this week with data loaded and zero errors, so Week 1 could start from solid ground.

*Deliverable:* a notebook that loads the raw data and prints basic shape/summary info.

## Week 1 — Cleaning and exploratory data analysis

This is usually the least glamorous and most important week. I identified missing values and decided what to do with each (drop, fill, or treat "missing" as its own meaningful category), figured out which columns were actually useful versus administrative noise, and defined my target variable precisely (readmitted within 30 days — yes/no). I also produced a handful of plots: how the target variable is distributed, how key features relate to it, whether there's obvious class imbalance.

*What I learned:* real clinical data is never clean, and the decisions I make while cleaning it quietly shape everything downstream — this is where a lot of real-world model bias actually gets introduced, often invisibly.

*Deliverable:* a cleaned dataset ready for modeling, and 3–4 plots that told me something about it.

## Week 2 — Baseline models

Trained my first models: logistic regression and a decision tree. Set up a proper train/test split, and got comfortable with the metrics that matter for this kind of problem — accuracy alone is misleading given the class imbalance, so precision, recall, and ROC-AUC became my real scoreboard. The goal wasn't a good model yet, it was a *baseline* to measure improvement against.

*What I learned:* why "high accuracy" can be a meaningless or even misleading number in healthcare prediction, and how to pick metrics that match what actually matters clinically (missing a true readmission is a different kind of costly than a false alarm).

*Deliverable:* a baseline model with a clear metrics report, plus one paragraph on why I chose the metrics I did.

## Week 3 — Better models

Tried a random forest and a gradient-boosted model (`HistGradientBoostingClassifier`), added cross-validation so my evaluation wasn't a fluke of one particular split, and did some light hyperparameter tuning. Picked my best-performing model and locked it in as "the model" for the rest of the project.

*What I learned:* the practical craft of model selection and validation — the part of ML that coursework often skips past quickly but that consumes most of a real project's time.

*Deliverable:* a comparison table of models and metrics, with my final model selected and justified.

## Week 4 — Interpretability

Installed `shap` and used it to understand what my chosen model is actually keying on: which features drive its predictions, in which direction, and whether that matches clinical intuition or looks suspicious. This is where the project stopped being "a model that works" and started being "a model I understand."

*What I learned:* SHAP and feature-importance analysis, and — just as important — how to read an interpretability result critically rather than treating a nice-looking plot as automatic proof the model is trustworthy.

*Deliverable:* SHAP summary plots plus a short write-up of the three most important drivers of the model's predictions, and whether they make clinical sense to me.

## Week 5 — Fairness audit

Sliced my model's performance by the demographic fields in the data — race, sex, age band — and checked whether it performs consistently well across groups, or quietly worse for some. Wrote up what I found plainly, including where the answer was uncomfortable. This wasn't a formality; it was the part of the project that most directly rehearsed the actual ethical stakes of deploying a model like this.

*What I learned:* what a fairness audit concretely involves, and a grounded, specific answer (rather than an abstract one) to the question "would I trust this model in a real clinical workflow, and under what conditions?"

*Deliverable:* a subgroup performance table and a half-page write-up of where I would and wouldn't trust this model, and why.

## Week 6 — Write-up and reflection

Pulled everything into a short final report: what I built, what it does well, where it breaks down, and what I'd do differently with more time or better data. Then stepped back from the technical content and answered, in writing, the questions below — this part was for me, not for the project.

*Deliverable:* a one-to-two-page project summary, plus my reflection notes (see [`docs/reflection.md`](reflection.md)).

## Stretch goals (optional)

Try the same pipeline on a second dataset to see how much of my process generalizes; or build a tiny Streamlit interface where I can input a hypothetical patient and see the prediction plus its top SHAP drivers — a small taste of what deploying this as an actual tool would involve.

## Reflection questions for my master's decision

Which week did I most look forward to starting — the cleaning and modeling weeks (2–3), the interpretability week (4), or the fairness audit week (5)? Did the technical model-building feel like the main event with the fairness audit as an add-on, or did it feel the other way around? When I found a problem in the model (bias, a confusing SHAP result, a metric that didn't make sense), did I want to go fix the model, or did I want to go read about *why* healthcare AI has this problem more broadly? And: knowing what I know now about what a week of this work actually feels like hour to hour, does more of it sound appealing, or does a different kind of hour sound better?

There's no right answer — the point is that my honest reaction to these six weeks is better evidence for my master's decision than any amount of thinking about it in the abstract.

---

*Sources for the datasets referenced above:*
- [Diabetes 130-US Hospitals for Years 1999-2008 — UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008)
- [Heart Disease — UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/45/heart+disease)
- [UCI Heart Disease Data (clean Kaggle mirror)](https://www.kaggle.com/datasets/redwankarimsony/heart-disease-data)