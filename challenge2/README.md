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
| `challenge2_decision_tree_random_forest_DAVID.ipynb` | Decision Tree and Random Forest: default vs. tuned comparison |
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

## Decision Tree and Random Forest - David Williams

`challenge2_decision_tree_random_forest_DAVID.ipynb` compares the default and tuned versions of both models using the shared split and five folds.

The preprocessing stays inside the pipeline. Category codes are one-hot encoded, `ID` is dropped, and the numeric features are not scaled. Grid Search uses positive-class F1 to choose the settings.

| Model | Default CV F1 | Tuned CV F1 |
|---|---:|---:|
| Decision Tree | 0.5564 | 0.6775 |
| Random Forest | 0.6513 | 0.7051 |

The tuned Random Forest is my choice from these two. It has better recall, but the pruned Decision Tree is simpler and has a smaller training-CV gap. The forest uses 300 trees, `min_samples_leaf=4`, `max_features=0.5`, and balanced class weights. The tree uses entropy, depth 6, `min_samples_leaf=15`, and `ccp_alpha=0.001`.

These are development CV scores from the same folds used for tuning. The team's final model still needs to be selected across all contributions before scoring the hold-out. The final prediction cell is off, so the test data has not been used.

Run with Python 3.12. From the repo's top folder:

```bash
pip install -r requirements.txt -r challenge2/requirements-trees.txt
cd challenge2
jupyter notebook challenge2_decision_tree_random_forest_DAVID.ipynb
```

Run all cells from top to bottom. The notebook creates `results_trees/` itself and includes the saved tables and charts.

This README was produced with assistance from AI (Claude).