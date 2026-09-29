# 🔢 Digit Recognizer — Handwritten Digit Classification

This was my fourth Kaggle Machine Learning project and my first project working with image data.

The goal was to recognize handwritten digits from grayscale images.

**Kaggle Score: 0.96550**

---

## 🎯 Goal

The main goal of this project was to step outside the usual tabular Machine Learning workflow and work with image data.

I wanted to understand how a classical Machine Learning approach would perform on handwritten digit recognition before moving on to more advanced computer vision methods.

I focused on:

* understanding the structure of image data;
* building a baseline;
* comparing different models;
* using cross-validation;
* tuning the final model;
* analysing classification errors visually.

---

## 🖼️ Dataset

Each image represents a handwritten digit from **0 to 9**.

The images have a resolution of **28 × 28 pixels**, which means that each image can be represented by **784 pixel features**.

The target variable is the digit label.

Although the data represents images, I initially treated the pixels as regular numerical features and used classical Machine Learning models.

---

## 🔍 Exploratory Data Analysis

I started by checking:

* dataset shape;
* pixel value ranges;
* target distribution;
* missing values;
* image dimensions.

I also visualized individual images to make sure I understood how the pixel values represented the handwritten digits.

This was useful because looking at the raw numbers alone does not give much intuition about what the model is actually seeing.

---

## 🤖 Baseline

I started with **Logistic Regression** as a baseline.

The idea was to first establish a relatively simple reference point before trying a more complex model.

This gave me a starting point for comparing later experiments.

---

## 🌲 Random Forest

I then experimented with **Random Forest**.

I compared the model using cross-validation and later tuned its hyperparameters.

The main goal was not simply to find a better score, but to check whether the additional model complexity actually produced a meaningful improvement over the baseline.

---

## 📊 Cross-Validation & Tuning

I used **5-fold cross-validation** during model evaluation.

I also experimented with different Random Forest hyperparameters and compared the results with the previous model.

This helped me get more comfortable with the idea that a model should be evaluated on multiple splits rather than relying on a single validation result.

---

## 🔎 Error Analysis

One of the most useful parts of this project was looking at the mistakes made by the final model.

I used a **confusion matrix** to see which digits were most often confused with each other.

I also visualized incorrectly classified images and looked at the actual handwritten digits.

This helped me see that some mistakes are understandable even for a human because different handwritten styles can make two digits look quite similar.

---

## 🧪 Experiments

The main experiments included:

* Logistic Regression baseline;
* Random Forest;
* 5-fold cross-validation;
* Random Forest hyperparameter tuning;
* confusion matrix analysis;
* visual analysis of incorrect predictions.

The general workflow was:

```text
Baseline
   ↓
New model
   ↓
Cross-validation
   ↓
Tuning
   ↓
Error analysis
   ↓
Final model
```

---

## 🏁 Final Result

The final model achieved:

**Kaggle Score: 0.96550**

The final version of the notebook also contains a separate section for the final model and prediction pipeline, so that the final submission can be reproduced without having to rerun every experimental step.

---

## 💡 What I Learned

This project taught me several new things:

* image data can be used with classical Machine Learning models when represented as numerical features;
* cross-validation gives a more reliable estimate than a single split;
* hyperparameter tuning does not always lead to a major improvement;
* confusion matrices can reveal useful information that a single accuracy score cannot;
* visual error analysis makes model mistakes much easier to understand;
* a good final notebook should separate experimentation from the final reproducible pipeline.

---

## 🚧 What I Would Improve

If I returned to this project now, I would like to compare the classical approach with methods specifically designed for image data.

For example, I would be interested in seeing how a Convolutional Neural Network performs on the same dataset.

I would also like to investigate the misclassified digit pairs in more detail and see whether additional preprocessing could help.

---

## ▶️ How to Run

Install the required dependencies:

```bash
pip install -r requirements.txt
```

Then open the notebook and run the project from the beginning.

The final model and submission pipeline are separated from the experimental sections.

---

## 🔗 Links

* **Kaggle:** [Digit Recognizer](https://www.kaggle.com/competitions/digit-recognizer)
* **My Kaggle Profile:** [kunkorun](https://www.kaggle.com/kunkorun)
