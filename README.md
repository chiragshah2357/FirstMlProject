# Student Exam Performance Indicator

An end-to-end machine learning project that predicts a student's **math score** based on their demographic background and other exam scores. Built as a Flask web app with a full training pipeline behind it.

**Live demo:** [firstmlproject-1.onrender.com](https://firstmlproject-1.onrender.com)

> Note: hosted on Render's free tier, so the app may take ~50s to wake up on first load if it's been idle.

## What it does

Given a student's:
- Gender
- Race/ethnicity
- Parental level of education
- Lunch type (standard / free or reduced)
- Test preparation course status
- Reading score
- Writing score

...the model predicts their **math score** (out of 100).

## Tech stack

- **Python**, **Flask** – web app
- **scikit-learn** – preprocessing pipeline and regression model
- **pandas / numpy** – data handling
- **Render** – deployment

## How it works

The project is split into a training pipeline and a prediction pipeline, both built as a reusable `src` package.

**Training pipeline** (`src/components/`)
1. `data_ingestion.py` – reads the raw dataset, splits it into train/test sets, and saves them to `artifacts/`.
2. `data_transformation.py` – builds a preprocessing pipeline (imputation, one-hot encoding, scaling) and saves it as `artifacts/preprocessor.pkl`.
3. `model_training.py` – trains several regression models (Linear Regression, Random Forest, XGBoost, CatBoost, etc.), picks the best performer via `GridSearchCV`, and saves it as `artifacts/model.pkl`.

**Prediction pipeline** (`src/pipeline/predict_pipeline.py`)
- Loads the saved preprocessor and model.
- Transforms the user's input the same way the training data was transformed.
- Returns a predicted math score.

**Flask app** (`app.py`)
- `/` – landing page
- `/predictdata` – input form (GET) and prediction result (POST)

## Project structure

```
FirstMlProject/
├── app.py                      # Flask app
├── requirements.txt
├── setup.py
├── artifacts/                  # saved model, preprocessor, train/test data
├── notebook/                   # EDA and model training notebooks
├── src/
│   ├── components/
│   │   ├── data_ingestion.py
│   │   ├── data_transformation.py
│   │   └── model_training.py
│   ├── pipeline/
│   │   ├── predict_pipeline.py
│   │   └── train_pipeline.py
│   ├── exception.py
│   ├── logger.py
│   └── utils.py
└── templates/
    ├── index.html
    └── home.html
```

## Running locally

```bash
git clone https://github.com/chiragshah2357/FirstMlProject.git
cd FirstMlProject
pip install -r requirements.txt
python app.py
```

The app will be available at `http://localhost:5000`.

## Retraining the model

```bash
python src/components/data_ingestion.py
```

This runs the full pipeline (ingestion → transformation → training) and overwrites `artifacts/model.pkl` and `artifacts/preprocessor.pkl`.

## Dataset

[Students Performance in Exams](https://www.kaggle.com/datasets/spscientist/students-performance-in-exams) dataset from Kaggle.
