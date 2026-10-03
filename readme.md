# 🔥 Forest Fire Weather Index (FWI) Predictor

A Flask web app that predicts the **Fire Weather Index (FWI)** from weather and fire-index readings, using a Ridge Regression model trained on the **Algerian Forest Fires dataset**.

## Features

- Clean, dark-themed form for entering the 9 input features
- Predicts FWI instantly and shows a colour-coded risk level (Low / Moderate / High / Extreme)
- Model and scaler loaded from pickle files
- Fully responsive, works on desktop and mobile

## Tech Stack

- **Backend:** Python, Flask
- **ML:** scikit-learn (Ridge Regression, StandardScaler), Pandas, NumPy
- **Frontend:** HTML, CSS (Jinja2 templates)

## Project Structure

```
codeatrix/
├── app.py                  # Flask app
├── requirements.txt
├── README.md
├── models/
│   ├── ridge.pkl           # Trained Ridge model
│   └── scaler.pkl          # Fitted StandardScaler
├── templates/
│   └── home.html           # Frontend form + result card
└── notebooks/
    ├── Ridge__Lasso_Regression.ipynb   # Data cleaning + EDA
    └── Model_Training.ipynb            # Feature selection + model training
```

## Input Features

| Feature | Description | Unit / Values |
|---|---|---|
| `Temperature` | Air temperature | °C |
| `RH` | Relative humidity | % |
| `Ws` | Wind speed | km/h |
| `Rain` | Rainfall | mm |
| `FFMC` | Fine Fuel Moisture Code | index |
| `DMC` | Duff Moisture Code | index |
| `ISI` | Initial Spread Index | index |
| `Classes` | Fire class | 0 = not fire, 1 = fire |
| `Region` | Region | 0 = Bejaia, 1 = Sidi Bel-Abbes |

**Target:** `FWI` (Fire Weather Index)

`BUI` and `DC` were dropped during training because they were highly correlated (> 0.85) with other features.

## Model Performance

Ridge Regression on the 25% test split:

| Metric | Score |
|---|---|
| R² | ~0.984 |
| MAE | ~0.564 |

Other models tried: Linear Regression, Lasso, LassoCV, RidgeCV, ElasticNet, ElasticNetCV.

## Getting Started

### 1. Clone the repo

```bash
git clone https://github.com/Aditya-hope/<your-repo-name>.git
cd <your-repo-name>
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Add the model files

Place `ridge.pkl` and `scaler.pkl` inside the `models/` folder. To generate them, run the end of `Model_Training.ipynb` with:

```python
import pickle
pickle.dump(ridge, open('models/ridge.pkl', 'wb'))
pickle.dump(scaler, open('models/scaler.pkl', 'wb'))
```

### 4. Run the app

```bash
python app.py
```

Open **http://127.0.0.1:5000** in your browser.

## How It Works

1. `GET /` returns the empty form (`home.html`).
2. Submitting the form sends a `POST` to `/predictdata` with all 9 values.
3. Flask scales the inputs with the saved `StandardScaler`, predicts with the Ridge model, and re-renders `home.html` with the result.

## requirements.txt

```
flask
scikit-learn
pandas
numpy
```

Pin `scikit-learn` to the same version you used to train the model, otherwise loading the pickle may fail or warn.

## Deployment

The app can be deployed on AWS Elastic Beanstalk. Name the main file `application.py` (with `application = Flask(__name__)`) since Elastic Beanstalk looks for that by default.

## Dataset

[Algerian Forest Fires Dataset](https://archive.ics.uci.edu/dataset/547/algerian+forest+fires+dataset) (UCI Machine Learning Repository), covering Bejaia and Sidi Bel-Abbes regions, June to September 2012.

## Author

**Aditya** · [GitHub: Aditya-hope](https://github.com/Aditya-hope)