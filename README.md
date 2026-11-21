# ☀️ Solar Power Generation Prediction

![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-Gradient%20Boosting-F7931E?logo=scikitlearn&logoColor=white)
![Streamlit](https://img.shields.io/badge/Deployed%20with-Streamlit-FF4B4B?logo=streamlit&logoColor=white)

Predicting the power output of a solar installation from weather conditions, so an operator can plan grid supply
and storage ahead of time. The best model (tuned **Gradient Boosting**) explains **~90% of the variance** in power output
on unseen data, and it's deployed as a **Streamlit** web app.

🎥 **[Watch the demo video](https://youtu.be/sFNEGxjOMiI?si=l29N0xLx2-k_Cp5z)**

---

## 📌 Business problem

Solar output is intermittent: it swings with sun angle, cloud cover, humidity and wind. When a grid operator under-forecasts,
it pays for backup power it didn't need; when it over-forecasts, it has to buy expensive power at short notice.
**Goal:** predict power generated (in Joules, per 3-hour period) from weather variables that are available in advance.

## 🗂️ Dataset

`solarpowergeneration.csv`: **2,920 observations** (3-hour periods), 9 weather features plus the target.

| Feature | Description |
|---|---|
| `distance-to-solar-noon` | Radians from solar noon (how high the sun is) |
| `temperature` | Daily average temperature |
| `wind-direction`, `wind-speed` | Daily averages |
| `sky-cover` | 0 (clear) – 4 (fully covered) |
| `visibility`, `humidity` | Daily averages |
| `average-wind-speed-(period)`, `average-pressure-(period)` | Averages over the 3-hour period |
| **`power-generated`** | **Target**: energy produced in the period |

## 🔬 Approach

1. **Data cleaning**: missing values and outliers checked and treated.
2. **EDA**: distributions and correlations. **Distance to solar noon** (r = −0.75) and **humidity** (r = −0.52) have the strongest relationship with output: the further from midday and the more humid, the less power.
3. **Modelling**: 8 regressors compared (Linear, Ridge, Lasso, Decision Tree, Random Forest, Gradient Boosting, SVR, KNN) on a 70/30 train-test split (2,043 / 877 rows).
4. **Tuning**: `GridSearchCV` over 243 Gradient Boosting configurations (5-fold CV).
5. **Validation**: 5-fold cross-validation of the tuned model.
6. **Deployment**: Streamlit app where a user enters weather inputs and gets a predicted output.

## 📊 Results

| Metric | Value |
|---|---|
| Best model | Gradient Boosting Regressor |
| R² on test set | **0.905** |
| RMSE on test set | ~3,054 J |
| 5-fold cross-validated R² | **0.855** (folds: 0.84, 0.77, 0.83, 0.92, 0.91) |
| Best hyper-parameters | `learning_rate=0.1, max_depth=5, n_estimators=100, min_samples_leaf=4, min_samples_split=10` |

The cross-validated R² (0.855) is a more honest estimate of real-world performance than the single test split (0.905).
One fold scores 0.77, so performance varies with the weather pattern in the period.

## 🖥️ Run it locally

```bash
git clone https://github.com/Rahman17309/Solar-Power-Prediction.git
cd Solar-Power-Prediction
pip install -r requirements.txt
jupyter notebook "Solar Panel- Regression project.ipynb"     # analysis & modelling
streamlit run Deployment/solar_modeldeployment.py              # web app
```

## 📁 Repository structure

```
├── Solar Panel- Regression project.ipynb   # EDA, modelling, tuning, evaluation
├── solarpowergeneration.csv                 # dataset
├── Deployment/
│   ├── solar_modeldeployment.py             # Streamlit app
│   └── gradient_boosting_model1.pkl         # trained model
├── Regression Project.pptm                  # project presentation
├── Solar_power_generation_RD.docx           # requirements document
└── Solar Demo video.mp4                     # app demo
```

## 🚧 Limitations & next steps

- Observations are 3-hour periods; a **time-based split** (train on earlier months, test on later) would better reflect forecasting than a random split.
- Add **weather-forecast** inputs (rather than observed weather) to make it a true day-ahead forecast.
- Report error in business terms (e.g. % of capacity) and compare against a simple persistence baseline.

## 🧰 Tech stack

Python · pandas · NumPy · scikit-learn · Matplotlib · Seaborn · Streamlit

---

## 👤 Author

**M A Rahman**: Data Analyst · SQL · Python · Power BI · Ex-Amazon
[LinkedIn](https://www.linkedin.com/in/rahman17309/) · [GitHub](https://github.com/Rahman17309) · [Portfolio](https://rahman17309.github.io/Portfolio.github.io/) · rahmandatascience09@gmail.com
