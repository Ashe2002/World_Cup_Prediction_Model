# International Football Match Prediction

A leakage-safe machine-learning pipeline for forecasting international football
scores, match outcomes, and double-chance probabilities.

## Overview

This project builds predictive models for international football using 4,452
historical fixtures and information available before each match. It predicts
expected home and away goals, converts those estimates into scoreline
probabilities, and derives probabilities for home wins, draws, away wins, and
double-chance outcomes.

The project was developed ahead of the 2026 FIFA World Cup, with particular
attention paid to avoiding temporal data leakage.

## Methodology

- Constructed pre-match Elo, GAP, and rolling-form features.
- Used timestamp-aware updates so simultaneous fixtures could not leak
  information into one another.
- Applied a chronological train-validation-test split.
- Fitted all imputation, scaling, and PCA transformations on training data only.
- Compared simple baselines, Random Forests, PCA-based models, histogram
  gradient boosting with a Poisson objective, and direct result classification.
- Evaluated probabilistic predictions using log loss rather than accuracy alone.
- Converted expected goals into an independent-Poisson score matrix.

## Predictions Produced

- Expected home and away goals
- Exact scoreline probabilities
- Home-win, draw, and away-win probabilities
- Home-or-draw, away-or-draw, and home-or-away probabilities
- Model-implied probabilities for comparison with market odds

## Repository Contents

- `Predictor_Final.ipynb` - complete data preparation, feature engineering,
  modelling, evaluation, and prediction pipeline.
- `Final_Data_enriched.csv` - enriched match-level modelling data.
- `final_combined_international_matches.csv` - combined fixture information.
- `1_results.csv` - generated model predictions.
- `Bets.xlsx` - spreadsheet used to compare predictions with available odds.

## Running the Project

Place the CSV files in a local `Data/` directory or update the paths near the
start of `Predictor_Final.ipynb`. Open the notebook and run all cells in order.

The notebook uses Python with pandas, NumPy, Matplotlib, Seaborn, and
scikit-learn.

## Design Decisions

Football prediction is especially vulnerable to leakage because ratings and
rolling statistics evolve after every fixture. Features in this project are
therefore calculated using only matches strictly preceding the fixture being
predicted. Model selection uses validation data, while the newest period is
reserved for final testing.

## Limitations

The score model assumes conditional independence between home and away goals.
International football data are also sparse for some teams, and squad strength,
injuries, and confirmed line-ups are not explicitly modelled. As such a relatively simple Elo-goals prediction model outperformed the random forests and boosting models. 


