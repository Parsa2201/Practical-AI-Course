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

## 5. Neural Networks
We move from traditional machine-learning models to neural networks. Starting with the connection between linear regression and the artificial perceptron, we explore why stacking only linear layers cannot learn complex patterns and how activation functions such as ReLU solve this problem. We then build the full neural-network story: forward propagation, mean squared error, backpropagation, gradient descent, learning rate, feature scaling, and training loops. Finally, students use PyTorch to train a small neural network on a non-linear regression problem and compare it with earlier machine-learning approaches.

## 6. Convolutional Neural Networks and Computer Vision
We introduce computer vision and Convolutional Neural Networks (CNNs), which are neural networks designed to learn from images. Using handwritten-digit recognition as the main task, students learn how images are represented as grayscale or RGB pixel grids and why ordinary fully connected networks struggle with image structure. We then explore filters, convolution, feature maps, ReLU, max pooling, and how CNN layers gradually combine pixels into meaningful patterns such as edges, curves, loops, and finally complete digits. We use the MNIST dataset to connect the theory to practice, then build and train a small CNN in PyTorch to recognize unseen handwritten digits.