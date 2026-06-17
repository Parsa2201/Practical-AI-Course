# Lesson 4: Popular Machine Learning Models

 You’ll learn what machine learning models do, how they think, how we measure their mistakes, and what unsupervised learning and neural networks are all about.

---

## 1. How a Model Actually Learns

Every supervised machine learning model follows a cycle:

1. **Training data** contains **features** (the measurements we have) and **labels** (the answers we want).
2. The **model** looks at the features and makes a **prediction**.
3. A **loss function** compares the prediction with the true label and calculates an error.
4. The model **adjusts its internal parameters** to make the error slightly smaller.
5. This loop – predict, measure error, adjust – repeats many times.

After enough cycles, the model becomes good at the task. We then **freeze** it and use it on new data that has no labels.

This is the same whether you’re using a tree, a neighbour‑based model, a linear equation, or a neural network.

---

## 2. The Models

### 2.1 Decision Tree
A decision tree is like a flowchart. Starting at the top, it asks simple yes/no questions about the features. Each question splits the data into two groups. After a few questions, you arrive at a leaf that gives the final prediction.

Because the questions are always “is feature X greater than a certain value?”, the decision boundary is made of **axis‑parallel rectangles** (hyper‑rectangles).

- **Overfitting risk:** If the tree is too deep, it memorises noise instead of learning general patterns.

### 2.2 K‑Nearest Neighbours (KNN)
KNN doesn’t build a model during training – it just **remembers** all the training examples. When you ask it to predict for a new point, it:

1. Finds the **K** most similar training examples (the nearest neighbours).
2. For **classification**: takes a majority vote among those neighbours.
3. For **regression**: averages their target values.

The decision boundary can be very complex and wiggly, because it’s shaped by the local distribution of points.

- **Important:** KNN is sensitive to feature scales. If one feature has a much wider range than another, it dominates the distance calculation.
- **Choice of K:** small K can overfit (jumpy boundary), large K can underfit (too smooth).

### 2.3 Logistic Regression
Despite the name, this is a **classification** algorithm. It works in two steps:

1. **Linear step:** It computes a score as a weighted sum of the features (like drawing a straight line through the data).
2. **Squashing step:** It passes that score through an S‑shaped function (the sigmoid). This turns any score into a number between 0 and 1 – a **probability**.

If the probability is ≥ 0.5, we predict class 1; otherwise class 0.  
The decision boundary is always a straight line (or flat plane in higher dimensions).

- Logistic regression works best when the two classes are roughly separable by a straight line.

### 2.4 Linear Regression
Linear regression is for **predicting a number**. It assumes the relationship between the features and the target is a straight line (or hyperplane):

$$
\hat{y} = b_0 + b_1 x_1 + b_2 x_2 + \dots
$$

It finds the line that makes the predictions as close as possible to the true values, on average. The distance between a point and the line is called the **residual**.

- Linear regression is simple, fast, and easy to interpret.
- It assumes the underlying relationship is linear – if the data is curved, it will not fit well.

---

## 3. Loss Functions – How We Measure Mistakes

A **loss function** gives a number that says “how wrong is this prediction?”.  
During training, the model tries to make this number as small as possible.

### 3.1 Mean Squared Error (MSE) – for Regression
For each data point, we:
- Take the difference between the true value and the prediction (the residual).
- Square that difference.
- Take the average over all points.

Squaring means:
- The error is always positive.
- Large errors count **much more** than small ones (e.g., an error of 4 contributes 16, not 4).

That’s why linear regression avoids large mistakes at all costs.

### 3.2 Cross‑Entropy (Log Loss) – for Classification
Cross‑entropy looks at the **probability** the model assigned to the true class.

- If the model is confident and correct (e.g., 95% for the true class), the loss is tiny.
- If the model is confident and **wrong** (e.g., 90% for a wrong class), the loss is enormous.

This forces the model not only to be right, but also to be **calibrated** – it shouldn’t be overconfident when it’s actually uncertain.

> In plain words: MSE punishes large numerical errors; cross‑entropy punishes confident wrong guesses.

---

## 4. Supervised vs Unsupervised Learning

|  | Supervised | Unsupervised |
|---|---|---|
| **What we have** | Features + Labels | Only Features |
| **Goal** | Learn to predict the label from the features | Discover hidden structure in the data |
| **Example tasks** | Classification, Regression | Clustering, Dimensionality Reduction |
| **Algorithms** | Decision Tree, KNN, Logistic Regression, Linear Regression | K‑Means, DBSCAN, PCA |

In supervised learning, we have a “teacher” (the labels).  
In unsupervised learning, the algorithm explores the data alone, looking for patterns, groups, or a simpler representation.

---

## 5. Clustering with K‑Means

Clustering means grouping similar data points together. K‑Means is the most common clustering algorithm.

**How K‑Means works:**
1. Pick the number of clusters **K**.
2. Place K **centroids** randomly in the feature space.
3. **Assignment step:** Every point is assigned to the nearest centroid.
4. **Update step:** Each centroid is moved to the average position of all the points assigned to it.
5. Repeat steps 3 and 4 until the centroids stop moving.

The result is K groups of points that are close to each other and far from other groups.

- K‑Means finds clusters **without any labels**.
- It is fast and works on many types of data.
- You must choose K yourself – techniques like the “elbow method” can help.

**Real‑world uses:** customer segmentation, grouping similar documents, finding subtypes in biological data.

---

## 6. Neural Networks – A Preview

A neural network is built from **many tiny units (neurons)** arranged in layers:

- **Input layer:** receives the raw features.
- **Hidden layer(s):** each neuron does a small weighted sum and applies a simple non‑linear function. The hidden layers automatically learn useful internal representations of the data.
- **Output layer:** produces the final prediction (probabilities for each class).

When you connect many of these units together, the network can learn **extremely complex, curved decision boundaries** – shapes that a decision tree or logistic regression could never draw.

- Neural networks are powerful but can overfit if too large.
- Training them requires a lot of data and computation.
- They are the foundation of modern AI (image recognition, language models, etc.).

*We will train our own neural network in the next session.*

---

## 7. Comparing Models – How to Choose?

No single model is always the best. When deciding which one to use, think about:

- **Nature of the problem:** Classification or regression? Linear or non‑linear?
- **Data size and dimensionality:** KNN slows down with many features; logistic regression loves them.
- **Feature scales:** KNN needs scaling; tree models do not.
- **Interpretability:** Decision trees and linear models are easy to explain; neural networks are black boxes.
- **Performance:** Compare accuracy, loss, and maybe speed.

Always try at least two different models and compare their performance before making a final choice.

---

## 8. Homework Summary

You’ll apply everything you learned to a real dataset: **Blood Donation Prediction**.  
Given `donation_train.csv` (features + labels) and `donation_test.csv` (features only), you need to:

1. Decide whether this is a classification or regression task.
2. Explore whether the features need scaling.
3. Try at least two models and evaluate them using **accuracy** and **cross‑entropy loss**.
4. Choose the best model and write a short paragraph explaining **why** you chose it.
5. Predict on the test set and save your results.

A skeleton notebook is provided – you fill in your code and your reasoning.

---

## 9. Key Takeaways

- All models learn by iterating: **predict → measure error → update**.
- Different models draw different decision boundaries (straight lines, rectangles, smooth curves).
- **Loss functions** quantify mistakes: MSE for regression, cross‑entropy for classification.
- Unsupervised learning finds hidden structure without labels – clustering is one powerful example.
- Neural networks are extremely flexible and will be explored soon.
- Choosing the right model is a skill – it comes from comparison, practice, and understanding your data.