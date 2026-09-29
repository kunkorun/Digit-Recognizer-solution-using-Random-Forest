# Digit Recognizer — Handwritten Digit Classification

In this project, I tackle the handwritten digit classification task from the [Kaggle Digit Recognizer competition](https://www.kaggle.com/competitions/digit-recognizer). The input consists of 28×28 grayscale images, and the goal is to predict a digit from 0 to 9.

For me, this project was less about the final accuracy and more about building a solid approach: with honest EDA, cross-validation, error analysis, and clear takeaways.

---

## Problem

The task is multiclass classification (10 classes). I use **accuracy** as the main evaluation metric.

## Data

The dataset consists of pixel-based representations of handwritten digits:

- `train.csv` — 42,000 labeled observations (working dataset)
- `test.csv` — 28,000 unlabeled observations (for the final Kaggle submission)
- `sample_submission.csv` — submission format template

Each image is represented by 784 pixel features (28 × 28) with values ranging from 0 to 255. The training set includes the target column `label`. I found no missing values during the data audit, and the classes are roughly balanced (5 is the least frequent, while 1, 3, and 7 are the most frequent).

## EDA

During the exploratory analysis, I:

- inspected individual digit images
- compared examples across different classes
- built average images for all 10 classes
- examined the distribution of zero and non-zero pixels
- analyzed stroke position variability within the same class

The average images revealed something interesting: while each digit has a common overall structure, the position of individual strokes can vary significantly across examples. Because of this, some areas in the average images look blurred — and these blurred regions later helped explain some of the model's errors.

## Modeling Approach

I followed this pipeline:

1. Data audit
2. Exploratory data analysis
3. Logistic Regression baseline
4. Random Forest baseline
5. 5-fold cross-validation
6. RandomizedSearchCV for Random Forest hyperparameters
7. Error analysis via confusion matrix
8. Manual review of misclassified images
9. Final Random Forest training on the full train set
10. Kaggle submission

### Logistic Regression

My first logistic regression scored around **0.906** on cross-validation. The initial run ended with a convergence warning — I added `StandardScaler`, the warning disappeared, but it didn't boost accuracy. For pixels, scaling turned out to be useful only for convergence, not for performance.

### Random Forest

Random Forest on the raw pixels scored around **0.9639** mean accuracy on 5-fold CV — noticeably better than the linear model. So, I took it as the baseline for further tuning.

## Hyperparameter Tuning

I searched for hyperparameters using `RandomizedSearchCV` over the following space:

| Parameter | Values |
|---|---|
| `n_estimators` | 100, 200, 300 |
| `max_depth` | None, 5, 10, 15, 20, 30 |
| `min_samples_split` | 2, 5, 10, 20 |
| `min_samples_leaf` | 1, 2, 4, 8 |
| `max_features` | sqrt, log2, None |

Search settings: `n_iter=20`, `cv=5`, `scoring='accuracy'`, `random_state=42`.

Best configuration found:

```text
n_estimators      = 100
max_depth         = 30
min_samples_split = 2
min_samples_leaf  = 1
max_features      = sqrt
```

Interestingly, the tuning **did not yield a substantial improvement** over the baseline. I decided not to complicate the search further: adding complexity without a clear payoff didn't seem justified to me.

## Error Analysis

The confusion matrix highlighted the most frequent confusions:

- `1 → 6`
- `4 → 9`
- `7 ↔ 9`

I manually inspected a few examples from the `4 → 9` pair — in 3 out of 5 cases, the four genuinely looked like a nine. This partially confirmed my hypothesis: some errors are tied to natural handwriting variability and similar digit shapes.

However, the number of errors in each pair was relatively small (under 20), so I decided against building dedicated feature engineering for these specific cases.

## Final Result

I trained the final Random Forest on the entire labeled train set and generated predictions for the Kaggle test set.

| Metric | Value |
|---|---|
| Local validation accuracy | 0.96095 |
| Kaggle accuracy | 0.96550 |

The final submission contains 28,000 predictions — exactly matching the number of rows in the Kaggle test set.

## Conclusions

For me, the main takeaway from this project isn't just the final accuracy, but the refinement of my overall approach. Along the way, I learned to:

- compare models using cross-validation rather than relying on a single holdout score
- formulate hypotheses **before** changing anything in the model or features
- use visual analysis to explore image data
- conduct error analysis before deciding whether it's even worth improving the model further
- distinguish a small random difference from a meaningful improvement
- keep the final pipeline separate from exploratory experiments

## Limitations and Future Work

The main limitation of the project is the relatively modest error analysis. The confusion matrix and manual review of a few errors helped explain certain cases, but didn't lead to a new feature engineering approach.

Another point is the model choice. Random Forest performed well, but I didn't compare it with approaches that better capture the spatial structure of images (like CNNs).

A logical next step would be to compare Random Forest with image-specific models and evaluate them using the same validation methodology.

## How to Run

1. Place the Kaggle dataset files in the project root:
   - `train.csv`
   - `test.csv`
   - `sample_submission.csv`

2. Run the notebook (or script) from top to bottom.

3. The final block will train the selected Random Forest on the full labeled dataset and save the predictions to `submission.csv`.

## Reproducibility

I used `random_state=42` in all experiments where applicable. The final model uses the hyperparameters found during the `RandomizedSearchCV` run.

## Git Workflow

I used the repository to maintain reproducibility and track meaningful changes.

Example workflow:

```bash
git status
git add .
git commit -m "Complete Digit Recognizer modeling and submission"
git push
```

I try to keep commits atomic with clear messages, for example:

- `Add data audit and EDA`
- `Add Logistic Regression baseline`
- `Add Random Forest baseline`
- `Add Random Forest tuning`
- `Add error analysis`
- `Finalize Kaggle submission`
- `Add project README`