# Challenge 2: Online Shopper Purchase Prediction

Each row is one visit to an online store. We predict `Revenue`: whether the visit ended in a purchase. This is binary classification.

## The data

- 9,247 visits in `challenge2_train.csv`. No missing values.
- Only 1,431 visits (15.5%) end in a purchase, so always guessing "no purchase" is already right 84.5% of the time. Accuracy alone is misleading here.
- `OperatingSystems`, `Browser`, `Region` and `TrafficType` are stored as numbers but are really category codes.

## What is in this folder

| File | What it is |
|---|---|
| `challenge2_validation_split.ipynb` | Loads the data and sets up the validation split |
| `challenge2_neural_network.ipynb` | Neural network (MLP): tuning, threshold, and evaluation charts |
| `challenge2_knn.ipynb` | K-nearest neighbors: tuning, threshold, and evaluation charts |
| `challenge2_train.csv` | Training data, with `Revenue` |
| `challenge2_test.csv` | Test data, no `Revenue` |

## Validation strategy

- **Hold-out:** 20% of the rows (1,850), set aside and scored once, after the final model is chosen.
- **5-fold cross validation** on the other 80% (7,397 rows) to compare models, check for overfitting, and tune settings.
- Both are **stratified**, so every piece keeps the same 15.5% share of buyers. The notebook checks this.
- `RANDOM_STATE = 42` everywhere, so everyone gets the same folds.

## How to run it

From the top folder of the repo:

```bash
pip install -r requirements.txt
cd challenge2
jupyter notebook challenge2_validation_split.ipynb
```
