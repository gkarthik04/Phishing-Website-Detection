# 🎣 Phishing Website Detection

Machine-learning system that flags phishing websites from their URL and page characteristics, served through a lightweight Python REST API with a browser dashboard that shows **why** a URL was flagged (SHAP explanations).

<p>
  <img src="https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/scikit--learn-F7931E?logo=scikitlearn&logoColor=white" />
  <img src="https://img.shields.io/badge/SHAP-explainability-8A2BE2" />
  <img src="https://img.shields.io/badge/Best%20model-Random%20Forest%2096.6%25-2EA44F" />
</p>

## Results

Four classifiers were trained on the same 80/20 split of the UCI-style phishing dataset (11,055 websites, 30 engineered features):

| Model | Accuracy |
|---|---|
| Logistic Regression | 92.8% |
| SVM | 92.8% |
| KNN | 96.0% |
| **Random Forest** | **96.6%** ✅ |

Random Forest was chosen for the highest accuracy, robustness on binary/mixed features, and built-in feature importances that pair well with SHAP.

## How it works

```
URL ──► feature extraction (IP in URL, length, '@', subdomains, HTTPS token, …)
        └──► Random Forest ──► phishing / legitimate + probability
                    └──► SHAP ──► top features pushing the decision
```

| Endpoint | Purpose |
|---|---|
| `GET /` | Dashboard (`index.html`) |
| `POST /predict` | Classify a URL |
| `POST /explain` | SHAP contributions for a URL |
| `GET /metrics` | Accuracy of every trained model |

## Run locally

```bash
pip install pandas numpy scikit-learn shap
python app.py
# open http://localhost:8000
```

`rf_model.pkl` and `rf_meta.json` hold the trained model and its metadata; `phishing.csv` is the training data. SHAP is optional — the API still predicts without it.

## Project structure

```
app.py          REST server, feature extraction, training & inference
index.html      Dashboard UI
phishing.csv    Dataset (30 features + label)
rf_model.pkl    Trained Random Forest
rf_meta.json    Feature names & model accuracies
```

## Team

Built as an ML lab project (2024–25) by **G Sai Karthikeya** and **Joshith Sai Panchumarthi**.
