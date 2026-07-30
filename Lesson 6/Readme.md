# Lesson 6: Convolutional Neural Networks and Computer Vision

## Session Goal

In this session, we introduce **Computer Vision** and **Convolutional Neural Networks (CNNs)**. The goal is to understand how a computer can learn from images and why CNNs are better suited to images than a normal fully connected neural network.

We focus on handwritten-digit recognition. By the end of the lesson, students should understand the journey:

```text
Image → pixels → filters → feature maps → useful visual patterns → predicted digit
```

This lesson builds directly on the previous lesson about neural networks. A CNN still uses predictions, loss, backpropagation, and gradient descent. The main difference is its special architecture for recognizing visual patterns.

---

## 1. What is Computer Vision?

**Computer Vision** is the part of AI that helps computers learn from images and video.

Some common computer-vision tasks are:

| Task | Question it answers | Example |
|---|---|---|
| Classification | What is in this image? | Is this image a cat or a dog? |
| Object Detection | What objects are present, and where are they? | Find cars and people in a street image. |
| Segmentation | Which exact pixels belong to each object? | Mark every pixel that belongs to a dog. |
| Face Recognition | Whose face is this? | Decide whether two photos show the same person. |

In this lesson, we focus on **classification**.

---

## 2. Today's Task: Handwritten Digit Recognition

We give the model an image of a handwritten digit. The model must choose one class from 0 to 9.

```text
Image of a handwritten digit → CNN → predicted digit
```

This is a classification problem because the answer is a category, not a continuous number.

The task is harder than it first appears. Different people write the same digit differently. A 7 can be thick, thin, tilted, shifted, or curved, but it should still be recognized as a 7.

The key challenge is:

> How can a model recognize the same pattern when its pixels are not exactly in the same place?

---

## 3. Images are Numbers

Humans see a handwritten `8`. A computer receives a grid of numbers called **pixels**.

For a grayscale image, each pixel has one brightness value:

```text
Low value → dark pixel
High value → bright pixel
Middle value → gray pixel
```

For example, a value of `0` can represent black and `255` can represent white. Many libraries scale these values to the range 0 to 1 instead.

An MNIST image has:

```text
28 rows × 28 columns = 784 pixels
```

### Grayscale vs. RGB

MNIST uses **grayscale**, so every pixel has one value.

Colour images usually use **RGB**:

```text
Red value + Green value + Blue value = one colour pixel
```

Therefore, a colour image usually has three channels, while an MNIST image has one channel.

---

## 4. Why Not Use a Normal Neural Network?

We could flatten a 28 × 28 image into one long list of 784 numbers and feed it to a normal neural network.

```text
28 × 28 image → 784-number list → dense neural network
```

This works, but it has important problems:

1. **Neighbouring pixels become separated.** Pixels that were beside each other in the image become far apart in the long list.
2. **There can be too many connections.** Larger images would create enormous numbers of weights.
3. **Small shifts can confuse the model.** A digit moved a few pixels is still the same digit, but its long list of numbers changes.

Images have local structure. Nearby pixels often form meaningful patterns such as an edge, a curve, a corner, or a loop. CNNs are designed to preserve and use this structure.

---

## 5. The CNN Idea: Look Locally

A CNN does not start by trying to understand the entire image at once. It looks at small nearby regions first.

```text
Small image patch → look for a pattern → move to the next patch
```

Examples of useful small patterns are:

- vertical lines;
- horizontal lines;
- diagonal lines;
- curves;
- corners;
- loops.

The small pattern detector is called a **filter** or **kernel**.

---

## 6. Filters and Convolution

A filter is a small grid of numbers. A common size is 3 × 3. It slides across an image and asks:

> Does this small patch look like my pattern?

For example, a simple vertical-edge filter can look like this:

```text
-1   0   +1
-1   0   +1
-1   0   +1
```

The left side and right side of an image patch are compared. If the patch contains a strong vertical change, the filter can produce a strong output.

### One Convolution Calculation

At one location, the CNN:

1. takes a 3 × 3 patch from the image;
2. multiplies matching cells in the image patch and filter;
3. adds those results;
4. writes one output value.

Then the filter moves to the next location and repeats the same process.

This sliding process is called **convolution**.

Traditional image processing often uses manually designed filters. In a CNN, filters start as random values and are learned during training.

---

## 7. Feature Maps

After a filter scans the whole image, it produces a new grid called a **feature map**.

```text
Input image + filter → feature map
```

A strong or bright value in a feature map means:

> This filter found its pattern in this part of the image.

A vertical-edge filter produces a map showing where vertical edges appear. A curve filter produces a map showing where curves appear.

One filter produces one feature map. A real CNN learns many filters at the same time, so one image can produce many feature maps.

---

## 8. ReLU in CNNs

CNNs still use the **ReLU** activation function from the previous lesson.

```text
Convolution → ReLU → useful activated features
```

ReLU keeps positive values and turns negative values into zero.

```text
If value > 0: keep it
If value ≤ 0: change it to 0
```

This helps the network focus on useful detected signals and keeps the important non-linearity that neural networks need.

---

## 9. Pooling: Keep What Matters

**Pooling** makes a feature map smaller while keeping important clues.

The most common kind is **max pooling**. It looks at a small region, often 2 × 2, and keeps the largest value.

```text
1   7          7
2   4    →     
```

Pooling helps because:

- later layers have fewer values to process;
- the model uses less computation;
- a small shift in an image is less likely to change the result too much.

Pooling does not mean that exact location is unimportant. It means that for many tasks, knowing that a pattern exists in a small region is more useful than remembering its exact pixel location.

---

## 10. From Pixels to Meaning

CNN layers gradually combine simple patterns into more meaningful information.

```text
Pixels
→ edges and strokes
→ curves, corners, and loops
→ parts of a digit
→ predicted digit
```

For a handwritten 8, an early layer may detect short edges. Later layers may combine them into curves and loops. The final layers use these learned features to decide that the image is most likely an 8.

This is an intuition, not a guarantee that every individual learned filter has a simple human name. The important idea is that deeper layers can combine earlier visual information into more useful representations.

---

## 11. The Complete CNN Pipeline

A simple CNN for digit recognition can follow this pattern:

```text
Image
→ Convolution + ReLU
→ Pooling
→ Convolution + ReLU
→ Pooling
→ Classifier
→ Prediction
```

The convolution layers find visual patterns. Pooling layers make the information more compact. The final classifier uses the learned features to create one score for each possible digit.

```text
Scores for 0–9 → choose the largest score → predicted class
```

If the score for 8 is the largest, the model predicts 8.

---

## 12. How Does a CNN Learn?

CNNs learn with the same core story as the neural networks from the previous lesson:

```text
Predict
→ measure error
→ backpropagate
→ adjust parameters
→ repeat
```

During training, the model sees an image and its correct label. It makes a prediction, compares it with the correct digit, and calculates a loss.

Backpropagation sends information about the error backward through the network. The CNN then adjusts not only the final classifier weights, but also the filter values. Over many examples, filters become useful detectors for the task.

The model is not given a perfect edge filter or loop filter in advance. It learns patterns that help reduce its classification error.

---

## 13. The MNIST Dataset

For the coding part of this lesson, we use the **MNIST** dataset.

| Item | Description |
|---|---|
| Images | Handwritten digits from 0 to 9 |
| Image size | 28 × 28 grayscale pixels |
| Total examples | 70,000 labelled images |
| Training images | 60,000 |
| Test images | 10,000 |

Each example contains:

```text
Image pixels → input/features
Correct digit → label
```

The training images help the CNN learn useful filters and classifier weights. The test images are kept separate so that we can check whether the CNN can recognize new handwritten digits.

---

## Key Terms

| Term | Meaning |
|---|---|
| Computer Vision | AI that learns from images or video |
| Pixel | One small numerical part of an image |
| Grayscale | An image with one brightness value per pixel |
| RGB | A colour representation with red, green, and blue values per pixel |
| CNN | A neural network designed for image-like data |
| Filter / Kernel | A small pattern detector that moves across an image |
| Convolution | Applying a filter at many locations in an image |
| Feature Map | The output grid that shows where a filter responded strongly |
| ReLU | Activation function that keeps positive values and removes negative ones |
| Pooling | Reducing a feature map while keeping important information |
| Classifier | The final part of the network that chooses a class |
| MNIST | A dataset of handwritten digit images |

---

## Final Takeaway

CNNs are neural networks built for images. Instead of flattening an image immediately and losing its local structure, they learn small visual patterns first.

```text
Image → pixels → filters → feature maps → learned features → prediction
```

The most important idea is:

> A CNN learns which visual patterns are useful for a task, then combines simple patterns into more meaningful ones.

In the coding session, we will load MNIST, visualize its images, build a small CNN, train it, and test how well it recognizes unseen handwritten digits.
