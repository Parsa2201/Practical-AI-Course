# Lesson 3

## Session Goal

In this session, students learn the core ideas behind Machine Learning. The main goal is to understand how ML systems learn from data, make predictions, measure mistakes, and improve over time. Students should also understand why a model that performs well on old data may fail on new data.

---

## 1. Classic AI vs Machine Learning

Classic AI and Machine Learning solve problems in different ways.

In classic AI, humans usually write explicit rules for the computer to follow. For example, a spam filter might use rules such as: if an email contains certain suspicious words, mark it as spam.

In Machine Learning, instead of writing every rule manually, we give the computer examples. The model studies the data and learns patterns from it. For example, instead of writing spam rules by hand, we give the model many emails labeled as spam or not spam, and it learns patterns that help it classify future emails.

The key idea is:

> Classic AI uses human-written rules. Machine Learning learns patterns from data.

---

## 2. Dataset

A dataset is a collection of examples used by a machine learning model.

Each example is one piece of data that the model can learn from. A dataset can contain information about houses, students, patients, emails, images, products, or almost anything else.

For example, in a house price prediction problem, a dataset might contain many houses and their information:

| House Size | Bedrooms | Location | Price |
|---|---:|---|---:|
| 120 m² | 3 | City Center | $300,000 |
| 80 m² | 2 | Suburb | $200,000 |
| 200 m² | 4 | City Center | $500,000 |

Data is the raw material of machine learning. If the data is poor, incomplete, or misleading, the model will usually learn poor patterns.

---

## 3. Features and Labels

In supervised learning, a dataset usually contains features and labels.

Features are the input information given to the model. They describe each example.

The label is the answer we want the model to predict.

For example, if we want to predict house price, the features could be house size, number of bedrooms, and location. The label would be the price.

In this example:
```text
House size, bedrooms, location → features
Price → label
```

The model looks at the features and tries to predict the label.

---

## 4. Machine Learning as Function Prediction

A machine learning model can be understood as a function that maps inputs to outputs.

The model receives input data, processes it, and produces a prediction.

text
Input → Model → Prediction

Examples:

text
House information → Model → Predicted price
Email text → Model → Spam or not spam
Image → Model → Cat or dog
Student data → Model → Predicted grade

The real relationship between input and output may be unknown, but the model tries to approximate it using examples from the dataset.

The key idea is:

> Machine Learning tries to learn a useful function from data.

---

## 5. Regression

Regression is a type of supervised learning where the model predicts a number.

Regression is used when the output is continuous or numerical.

Examples of regression problems include:

| Problem | Prediction |
|---|---|
| Predicting house price | A price, such as $300,000 |
| Predicting temperature | A number, such as 27°C |
| Predicting exam score | A number, such as 85 |
| Predicting delivery time | A number, such as 35 minutes |

In regression, the model answers questions like:

text
How much?
How many?
What value?

---

## 6. Classification

Classification is a type of supervised learning where the model predicts a category or class.

Classification is used when the output belongs to a fixed group of possible answers.

Examples of classification problems include:

| Problem | Prediction |
|---|---|
| Spam detection | Spam or not spam |
| Image recognition | Cat, dog, car, etc. |
| Disease diagnosis | Positive or negative |
| Customer churn prediction | Will leave or will stay |

In classification, the model answers questions like:

text
Which category?
Which class?
Which group?

---

## 7. Regression vs Classification

The difference between regression and classification depends on the type of output.

If the output is a number, it is usually a regression problem.

If the output is a category, it is usually a classification problem.

Examples:

| Task | Type |
|---|---|
| Predicting a house price | Regression |
| Predicting whether an email is spam | Classification |
| Predicting tomorrow's temperature | Regression |
| Predicting whether a student passes or fails | Classification |
| Predicting the weight of a package | Regression |
| Predicting the color category of an object | Classification |

The key question is:

> Is the model predicting a number or a category?

---

## 8. Loss Function

A loss function measures how wrong the model's prediction is.

After the model makes a prediction, we compare the prediction with the correct answer. The loss tells us how large the mistake is.

For example:

text
Actual price: $300,000
Predicted price: $250,000
Error: $50,000

A large mistake gives a high loss. A small mistake gives a low loss. A perfect or almost perfect prediction gives very low loss.

The key idea is:

> Loss is a score that tells the model how bad its prediction was.

---

## 9. Training

Training is the process where a model improves by learning from data.

During training, the model makes predictions, calculates loss, and adjusts itself to reduce that loss.

A simplified training loop looks like this:

text
Make prediction → Measure loss → Adjust model → Repeat

At the beginning, the model may make poor predictions. Over time, if training works well, the predictions become better and the loss becomes smaller.

Training does not mean the model understands the world like a human. It means the model finds patterns in the data that help it make better predictions.

---

## 10. Train, Validation, and Test Data

To evaluate a model fairly, we usually split the dataset into different parts.

The training set is used to teach the model.

The validation set is used to compare model versions or tune settings.

The test set is used at the end to check how well the model performs on new, unseen data.

A useful analogy is:

| Dataset Part | School Analogy |
|---|---|
| Training set | Homework |
| Validation set | Practice exam |
| Test set | Final exam |

The test set is important because we do not only care about how well the model performs on old data. We care about how well it performs on new data.

The key idea is:

> A good model should generalize to data it has not seen before.

---

## 11. Generalization

Generalization means the model can perform well on new examples, not just the examples it saw during training.

A model that generalizes well has learned the real pattern behind the data.

A model that does not generalize well may have only memorized the training examples.

For example, a student who memorizes homework answers may get 100% on the homework but fail the final exam. A student who understands the ideas may perform well on both homework and the exam.

Machine learning has the same problem:

text
Good training performance + good test performance → likely good generalization
Good training performance + bad test performance → likely memorization

---

## 12. Underfitting

Underfitting happens when a model is too simple to learn the real pattern in the data.

An underfitting model performs poorly on both training data and test data.

For example, if the data follows a curved pattern but the model can only draw a straight line, it may miss the important structure.

Signs of underfitting:

text
Training performance is poor
Test performance is poor
The model is too simple
The model misses the pattern

Underfitting means the model has not learned enough.

---

## 13. Overfitting

Overfitting happens when a model learns the training data too closely.

An overfitting model may perform extremely well on the training data but poorly on new data.

This usually happens when the model memorizes noise, random details, or accidental patterns instead of learning the general rule.

For example, a very wiggly curve might pass through every training point exactly, but fail to predict future points correctly.

Signs of overfitting:

text
Training performance is very high
Test performance is much lower
The model is too complex
The model memorizes instead of generalizing

Overfitting means the model learned too many details from the training data.

---

## 14. Good Fit

A good model is usually between underfitting and overfitting.

It is complex enough to learn the real pattern, but not so complex that it memorizes every small detail.

A good fit captures the main trend in the data and performs well on new examples.

The goal is not to get perfect training performance. The goal is to make useful predictions on unseen data.

The key idea is:

> A good model learns the pattern, not the noise.

---

## 15. Bias and Variance

Bias and variance are two important ways to understand model errors.

Bias is error caused by a model being too simple or making assumptions that are too strong.

High bias often leads to underfitting.

Variance is error caused by a model being too sensitive to the training data.

High variance often leads to overfitting.

A simple connection is:

| Concept | Usually Causes |
|---|---|
| High bias | Underfitting |
| High variance | Overfitting |

A good model tries to balance bias and variance.

---

## 16. Bias-Variance Intuition

Bias and variance can be understood using a target analogy.

If predictions are tightly grouped but far from the correct answer, the model has high bias. It is consistently wrong in the same direction.

If predictions are scattered widely, the model has high variance. It is unstable and changes too much depending on the data.

If predictions are close to the correct answer and grouped together, the model has low bias and low variance.

The ideal model is accurate and consistent.

---

## 17. Full Machine Learning Story

The full machine learning process can be summarized as one connected story.

First, we collect a dataset. The dataset contains examples. Each example has features, and in supervised learning, each example also has a label.

Then, we train a model. The model tries to learn a function from inputs to outputs.

The model makes predictions. A loss function measures how wrong those predictions are.

During training, the model adjusts itself to reduce loss.

After training, we test the model on unseen data to check whether it generalizes.

If the model is too simple, it may underfit. If it is too complex, it may overfit.

Bias and variance help us understand these problems more deeply.

---

## Key Terms

| Term | Meaning |
|---|---|
| Dataset | A collection of examples used for learning |
| Example / Sample | One data point in a dataset |
| Feature | An input used by the model |
| Label | The correct answer the model tries to predict |
| Model | The system that learns patterns from data |
| Prediction | The output produced by the model |
| Regression | Predicting a number |
| Classification | Predicting a category |
| Loss Function | A way to measure prediction error |
| Training | The process of improving the model |
| Test Set | Data used to evaluate the model after training |
| Generalization | Performing well on new data |
| Underfitting | When the model is too simple |
| Overfitting | When the model memorizes training data |
| Bias | Error from overly simple assumptions |
| Variance | Error from being too sensitive to data |

---

## Final Takeaway

Machine Learning is about learning patterns from data to make useful predictions.

A good model does not simply memorize old examples. It learns patterns that help it perform well on new examples.

The most important question in machine learning is not:

text
How well did the model do on the training data?

The more important question is:

text
How well does the model work on new data?
