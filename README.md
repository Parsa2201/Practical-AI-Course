# Practical-AI-Course
In this series of lessons, you will learn the basics of a veriaty of AI algorithms and models. You also explore basic usecases of libraries that implemented these AI methods. By the end of the course, you will have a foundational understanding of what AI is, the notable algorithms in AI from 1950 to the present, and how to use them to solve problems using Python.

For this course, you should:
- have a basic understanding of Python,
- Be motivation and a self-learner.

# Lessons
The topics covered so far are:
## 1. History of AI
We explore the journey AI has taken, from simple AI agents and solvers to the large-scale AI systems widely used today, including LLMs (Large Language Models).

## 2. Search Algorithms
We dive into the concept of searching, and study the well-known algorithm called $A^\star$ . We see some examples and, by the end, solve a simple maze puzzle using the `networkx` library and its implementation of $A^\star$.

## 3. Machine Learning Fundamentals
We introduce the core ideas behind Machine Learning and how they differ from rule-based AI. In this lesson, we study datasets, features, and labels, and learn to think of ML models as functions that map inputs to predictions. We then explore the two main supervised learning tasks, regression and classification, and discuss how models measure mistakes using loss functions. Finally, we cover the ideas of training, train/validation/test splits, generalization, overfitting, underfitting, and the bias-variance tradeoff, building intuition for how to evaluate whether a model has truly learned a useful pattern from data.

## 4. Popular Machine Learning Models
We build on the fundamentals by exploring several widely used models and how each "thinks": decision trees that split data with axis-aligned questions, K‑nearest neighbours that classify or regress by majority vote or averaging among the closest examples, logistic regression that produces a probability via a linear score squashed through an S‑curve, and linear regression that fits a straight line to minimise squared errors. We then deepen the idea of loss functions, contrasting mean squared error for regression with cross‑entropy for classification, and introduce unsupervised learning through clustering with K‑means, an algorithm that discovers hidden groups without any labels. Finally, we take a sneak peek at neural networks as stacked layers of simple units capable of learning highly curved decision boundaries, setting the stage for the next session.