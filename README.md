# CAI 4105 Fall 2026, Team Seven: Rental Price Prediction

We predict the monthly rent of an apartment from its size, rooms, and location.

## Results

The score is **RMSE**: the typical size of our miss, in dollars. Lower is better. All scores use the same 5-fold cross validation on the training data.

| Step | Best model | Average miss |
|---|---|---|
| Starting point | Hist Gradient Boosting | $409 |
| Added new features and log rent | Hist Gradient Boosting | $399 |
| Tuned the settings | Hist Gradient Boosting | **$384** |

**Final model:** tuned Hist Gradient Boosting. It ties for the best score, holds up best on locations it has never seen, and trains in seconds.

## What is in this repo

| File | What it is |
|---|---|
| `rental_price_model.ipynb` | The full project: look at the data, clean it, compare models, tune, and make predictions |
| `challenge1_train.csv` | Training data, with prices |
| `challenge1_test.csv` | Test data, no prices |
| `predictions.csv` | Our rent guesses for the test data |
| `requirements.txt` | Python packages needed to run the notebook |

## How to run it

```bash
pip install -r requirements.txt
jupyter notebook rental_price_model.ipynb
```

Then choose **Run All**. It takes about 5 minutes on a laptop and rewrites `predictions.csv`.

## What we did

1. **Looked at the data.** No missing values. Rent is lopsided (a few very expensive units). Many listings share the exact same map point.
2. **Cleaned it.** Removed 111 rows that were exact copies.
3. **Compared five models.** Linear Regression, Decision Tree, Random Forest, Gradient Boosting, and Hist Gradient Boosting.
4. **Added new features.** Space per room, log of square feet, and a tilted version of the map coordinates. We also tried predicting the log of rent.
5. **Tuned the best models** with grid search.
6. **Ran a tougher test** where each location only appears in one fold, to see how the model does in places it has never seen.
7. **Picked the final model** and wrote `predictions.csv`.

The notebook explains each step and each choice in more detail.

## Validation Strategy.

We want to know how well a model will do on rents it has never seen, so we set aside 20% of the training rows (1,480 of 7,398) and do not touch them until the final model is chosen. The other 5,918 rows are split into 5 folds: each model trains on 4 folds and is scored on the 5th, five times over. Every model and every grid search uses these same folds, so the scores are comparable. Data preparation (filling gaps, scaling numbers, turning categories into columns) happens inside each fold, so the scoring rows never influence it.

- **Score:** RMSE, the typical size of our miss in dollars. Lower is better. We also report MAE and R².
- **Starting point:** always guessing the average rent misses by about $812. Any real model has to beat that.
- **Overfitting check:** a model whose training score is much better than its fold score is memorizing, not learning.
- **Final check:** the 1,480 held-out rows are scored once, then the final model is retrained on all 7,398 rows to make `predictions.csv`.
- **Code:** `Angel_Validation.ipynb` (the `compare` and `holdout_rmse` functions).

**Final hold-out RMSE:** _fill in after the final model is chosen_

## Progress log

| Date | Change |
|---|---|
| 2026-09-22 | Starting template and data added |
| 2026-09-29 | Data cleaning, new features, log rent, fifth model, tuning, tougher test, written notes, README |

## Working together

- Make a new branch for each change. Do not work straight on `main`. Open a pull request into `main` when a change is ready.
- Rerun the whole notebook before committing so the saved results match the code.
- Write short commit messages that say what changed and why.
