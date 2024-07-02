---
layout: post
title:  "Linear Regression and Gradient Descent"
date:   2024-03-24 18:00:00 +0530
categories: jekyll update
author: 'Aritra Mukhopadhyay'
---

<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>

<style>
  .slider-container {
    margin: 20px 0;
  }
  label {
    display: inline-block;
    width: 100px;
  }
</style>

- [What is Machine Learning?](#what-is-machine-learning)
- [Linear Regression](#linear-regression)
  - [Why Learning to do a Linear Fit is enough?](#why-learning-to-do-a-linear-fit-is-enough)
  - [Shift to Matrix Notation and Concept of Batch Processing](#shift-to-matrix-notation-and-concept-of-batch-processing)
  - [The Loss Function](#the-loss-function)
- [Gradient Descent](#gradient-descent)


Although this will be a walk through the concept of Linear Regression mostly from point of view of Machine Learning, we will also touch upon the mathematical aspects of it and it's uses in Statistical data analysis (which is mostly relevant to our lab experiments).

This is the first talk in the series of ML. So, let us start with the very basics.

## What is Machine Learning?

> In a very crude way, Machine Learning can be thought of as a very detailed study of **the art of curve fitting**.

We have some data, we have some intuitions about the nature of the data (we have a model), and we try to fit the data into the model.

Let us take an example, and the idea will be clearer. Suppose the task is to make a decision given some situation.

For example, suppose the question is: **"Whether I am going to play Badminton today or not"**. Now, the decision depends on a lot of factors. For example, the day of the week, the time of the day, the number of people coming to play, todays weather, etc.

So, we have a lot of factors which influence our decision. Our decision is going to be based on these factors. We can say that our decision would be a function of these factors.

$$\text{decision} = f(factor_0, factor_1, factor_2, ...)$$

Given the value of these factors, if we have the function, we can easily take the decision. Now to actually do it, we have to express (encode) all these factors in terms of some numbers.

For example, we can easily put the day and time in terms of numbers; weather might be difficult, but we can still think of some aspect which is relevant to us, for example, the temperature, or the humidity, or how much sunny it is etc.

Now, we have some factors and answers; both encoded in terms of numbers. Let us say we have $n$ such factors, and we encode them as $x_0, x_1, x_2, \ldots, x_{n-1}$. Similarly we encode the answer/decision as $y$ (say).

So according to the motivation above, here we can write that:

$$y = f(x_0, x_1, x_2, \ldots, x_{n-1})$$

Now the only thing we need to know is the function $f$. If somehow we can get hold of that, given some new situation (set of factors) we can easily return the new decision.

## Linear Regression

Now let us make an Assumption:

> **$f$ is a linear function**

To put it mathematically, say $f$ is of the form:

$$f(x_0, x_1, x_2, \ldots, x_{n-1}) = b + w_0x_0 + w_1x_1 + w_2x_2 + \ldots + w_nx_n$$

**Note:** Linear function does not mean that the function is a straight line. Sure it is a straight line in 2D, but in 3D it is a ~~straight line~~ plane, in higher dimensions it is called a hyperplane.

So here we have the function $f$ in terms of the weights $b, w_0, w_1, w_2, \ldots, w_{n-1}$. Now, we have to find the optimum values of these weights so that the function fits the data best.

### Why Learning to do a Linear Fit is enough?

In practice, we often prefer to work with linear functions. This is because they are easy to handle and have been extensively studied. You might wonder, what if the data isn't linear? Well, in many cases, we can apply simple transformations to the data to make it linear.

Let's illustrate this concept with an example. Suppose we have a dataset that follows an exponential pattern:

$$y = ae^{bx}$$

Now, let's take the logarithm of both sides:

$$\log y = \log a + bx$$

By doing so, if we plot the graph between "$\log y$" and "$x$", we'll obtain a straight line. This means we can fit a linear model to the data with just some transformations, and subsequently, extract the values of $a$ and $b$ from the slope and intercept of the line.

### Shift to Matrix Notation and Concept of Batch Processing

As we move forward with our linear fitting function, it's essential to introduce matrix notation and the concept of batch processing. This shift is motivated by the fact that modern computing architectures, such as Graphics Processing Units (GPUs), are designed based on Single Instruction, Multiple Data (SIMD) principles. This means they can perform the same operation on multiple data points simultaneously, making them more efficient for large-scale computations.

To illustrate this concept, let's consider a table. In this table each row represents an example, and the columns contain feature values and the last column contains the true output value. The table looks like this:

| $x_0$ | $x_1$ | $x_2$ | ... | $x_{n-1}$ | y |
|:---:|:---:|:---:|:---:|:---:|:---:|
| 1.2 | 3.4 | 5.6 | ... | 2.4 | 1.5 |
| 7.8 | 9.0 | 11.2 | ... | 4.2 | 0.9 |
| ... | ... | ... | ... | ... | ... |
| ... | ... | ... | ... | ... | ... |
| $m^{th}$ | row | filled | with | similar | values |

Now, let's define the matrices $X$, $W$, $B$, and $Y$:

- $X$ $\in \mathbb{R}^{m \times n}$: a matrix containing feature values, where each row represents an example and each column represents a feature.
- $W$ $\in \mathbb{R}^{n \times 1}$: a matrix containing the weights, where each row corresponds to a feature and the single column contains the weight values.
- $B$ $\in \mathbb{R}^{1 \times 1}$: a scalar bias term.
- $Y$ $\in \mathbb{R}^{m \times 1}$: a matrix containing the predicted output values, where each row represents an example and the single column contains the corresponding output value.

Now, Our fitting function for each example $i$ in the dataset was:

$$y_i = f(x_0^{(i)}, x_1^{(i)}, x_2^{(i)}, \ldots, x_{n-1}^{(i)}) = b + w_0x_0^{(i)} + w_1x_1^{(i)} + w_2x_2^{(i)} + \ldots + w_{n-1}x_{n-1}^{(i)}$$

With these matrices defined, we can rewrite our linear fitting function in matrix notation as:

$$\textbf{Y} = \textbf{X}\textbf{W} + \textbf{B}$$

This compact representation not only enables us to perform efficient computations on large datasets using modern computing architectures, but also saves us from writing lengthy equations for each example in the dataset.

### The Loss Function

Now, we have to define the notion of "best fit". How do we know that a line fits the data best? We have to define a notion of "error" or "loss". We have to define a function which will tell us how far the line is from the data. We will call this function the **loss function**. More loss means the line is far from the data, less loss means the line is close to the data.

One commonly used loss function for regression tasks is the **Mean Squared Error (MSE) Loss**, which measures the average squared difference between predicted and actual values.

$$L = \frac{1}{m} \sum_{i=1}^{m} (y_i - \hat{y}_i)^2$$

where $L$ is the loss, $y_i$ is the true output value for the $i^{th}$ example, and $\hat{y}_i = b + w_0x_{i0} + w_1x_{i1} + w_2x_{i2} + \ldots + w_{n-1}x_{i(n-1)}$ is the predicted output value from the model.
Now, let's represent the MSE Loss using matrix notation:

$$L = \frac{1}{m} sum(\textbf{Y} - \hat{\textbf{Y}})^2$$

The MSE Loss calculates the average of the squared differences between the true values $\textbf{Y}$ and the predicted values $\hat{\textbf{Y}}$.

In the image below, we can see how the MSE Loss is calculated for a single example. In the image The red dots represent the data points and the purple dots represent the points on the fitted line who have the same x value  $d_i = y_i - \hat{y}_i$.

![MSE Loss](IMG_0011.jpg)

The MSE Loss is useful for regression tasks because it penalizes large differences between predicted and actual values more heavily than small differences. This encourages the model to produce predictions that are close to the actual values.

Note that the MSE Loss is not suitable for classification tasks, where we need a different type of loss function. Classification tasks fall under logistic regression. For example, in classification tasks, we often use the **Cross-Entropy Loss**, which measures the difference between predicted probabilities and actual labels. We will explore this topic further in another discussion.

Note that, the loss function depends on the $\textbf{Y}$ and $\hat{\textbf{Y}}$. And the $\hat{\textbf{Y}}$ depends on the features $X$, the weights $W$ and the bias $B$. Among these, the features $X$ and the true output values $\textbf{Y}$ are given to us and hence are fixed constants. Hence the loss function is a function of the weights $W$ and the bias $B$.

## Gradient Descent

Now that we have the loss function which is a measure of how good the fit is. And we also know that the minimum value of the loss function is the best fit. So, we have to find the values of the weights $W$ and the bias $B$ for which the value of the loss function is the minimum. We also know that the loss function is a quadratic function. Hence it is a parabola (a U-shaped curve) with a single minima

### Why not maxima-minima

Now, to find the minima of a function we generally use techniques like maxima-minima where we set the derivative of the function to zero and solve for the variables $W$ and $B$. We could have very well taken the same route here. And it might have worked too. But there is a subtle issue with that approach.

Maxima minima requires us to equate the derivative of the function to zero and solve for the variables. And most efficient ways of solving such systems of linear equations on a computer generally involv inverting matrices. Matrix inversion is computationally intensive, especially for large matrices. The computational complexity of inverting an \( n \times n \) matrix is \( O(n^3) \), which becomes impractical for very large models and datasets. Additionally, matrix inversion can be numerically unstable if \( X^T X \) is not well-conditioned (i.e., it has a large condition number).

Below is a comparison of Gradient Descent and Maxima-Minima, the pros and cons of each approach:

| **Criteria**            | **Gradient Descent**                                                                 | **Maxima-Minima**                                                 |
|-------------------------|--------------------------------------------------------------------------------------|-------------------------------------------------------------------|
| **Scalability & Memory Efficiency** | More scalable and memory-efficient for large datasets as it iteratively updates parameters without requiring large matrix storage or inversion. | Less scalable and memory-efficient due to the need for storing and inverting large matrices. |
| **Solution Accuracy**   | May not converge to the exact global minimum, and can get stuck in local minima. | Provides an exact solution, assuming the matrix is well-conditioned. |
| **Computational Intensity & Stability** | Computationally less intensive per iteration but requires many iterations; generally stable but depends on learning rate and initialization. | Computationally intensive due to matrix inversion and can be numerically unstable if the matrix is ill-conditioned. |
| **Process Nature**      | Iterative process, which may require many iterations to converge.                    | One-step process if the system of equations is solved directly. |

Given these considerations, gradient descent is often preferred in machine learning applications for its scalability and efficiency in handling large datasets, despite the fact that it may not always reach the exact minimum and can be iterative in nature.


### Intuition Behind Gradient Descent

Imagine you are at the top of a mountain and you want to reach the bottom. You don't know the exact path, but you can feel the slope under your feet. If you always take a step in the direction where the slope is steepest, you will eventually reach the bottom. This is the intuition behind gradient descent.

In mathematical terms, the gradient is a vector that points in the direction of the steepest increase of the function. By moving in the opposite direction of the gradient, we move towards the minimum value of the function.

### Gradient Descent Algorithm

The gradient descent algorithm involves the following steps:

1. **Initialize the Parameters**: Start with some initial values for the weights $W$ and the bias $B$. These can be random values or zeros.
2. **Compute the Gradient**: Calculate the gradient of the loss function with respect to the weights $W$ and the bias $B$.
3. **Update the Parameters**: Update the weights $W$ and the bias $B$ by moving in the direction opposite to the gradient. This can be done using the following update rules:
  $$W := W - \alpha \frac{\partial L}{\partial W}$$
  $$B := B - \alpha \frac{\partial L}{\partial B}$$
  where $\alpha$ is the learning rate, a small positive number that controls the step size.
4. **Repeat**: Repeat steps 2 and 3 until the loss function converges to a minimum value or until a certain number of iterations is reached.

### Gradient Descent in Matrix Notation

In matrix notation, the gradient descent update rules can be written as:
$$\textbf{W} := \textbf{W} - \alpha \frac{\partial L}{\partial \textbf{W}}$$
$$\textbf{B} := \textbf{B} - \alpha \frac{\partial L}{\partial \textbf{B}}$$

Where $\frac{\partial L}{\partial \textbf{W}}$ and $\frac{\partial L}{\partial \textbf{B}}$ are the gradients of the loss function with respect to the weights and bias, respectively.

### Choosing the Learning Rate

Choosing the right learning rate $\alpha$ is crucial for the convergence of the gradient descent algorithm. If the learning rate is too small, the algorithm will take tiny steps and converge slowly. If the learning rate is too large, the algorithm might overshoot the minimum and fail to converge.

A common practice is to start with a relatively large learning rate and gradually decrease it as the algorithm progresses. This technique is known as **learning rate annealing**.





<div class="slider-container">
  <label for="alpha">Alpha:</label>
  <input type="range" id="alpha" min="-0.1" max="3.1" step="0.1" value="1.5">
  <span id="alpha-value">1.5</span>
</div>

<div class="slider-container">
  <label for="epochs">#Epochs:</label>
  <input type="range" id="epochs" min="1" max="20" step="1" value="10">
  <span id="epochs-value">10</span>
</div>

<div id="plot"></div>







### Types of Gradient Descent

There are several variants of gradient descent, each with its own trade-offs:

1. **Batch Gradient Descent**: Uses the entire training dataset at a time to compute the gradients. This can be slow for large datasets but provides accurate gradient estimates.
2. **Stochastic Gradient Descent (SGD)**: Uses a single training example to compute the gradients. This can be much faster but introduces more noise in the gradient estimates.
3. **Mini-Batch Gradient Descent**: Uses a small subset (mini-batch) of the training dataset to compute the gradients. This strikes a balance between the efficiency of SGD and the accuracy of batch gradient descent.

In today's discussion we have covered the basics of linear regression and gradient descent, we also touched, but chose not to discuss in detail on certain topics like the types of gradient descent, how to handle classification tasks, the coding aspect of it, etc. We will cover these topics in future discussions.




<script>
    // Get slider elements
    const alphaSlider = document.getElementById('alpha');
    const epochsSlider = document.getElementById('epochs');
    const alphaValue = document.getElementById('alpha-value');
    const epochsValue = document.getElementById('epochs-value');

    // Initialize Plotly graph (empty for now)
    Plotly.newPlot('plot', [{
      x: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
      y: [2, 4, 1, 5, 3, 7, 6, 8, 9, 5],
      type: 'scatter'
    }], {
      title: 'Test Data Plot',
      xaxis: { title: 'X Axis' },
      yaxis: { title: 'Y Axis' }
    });

    // Function to be called when alpha slider is adjusted
    function foo_alpha(value) {
        // Implement your logic here
        console.log('Alpha changed:', value);
        // Update the graph here
    }

    // Function to be called when #epochs slider is adjusted
    function foo_nep(value) {
        // Implement your logic here
        console.log('#Epochs changed:', value);
        // Update the graph here
    }

    // Event listeners for sliders
    alphaSlider.addEventListener('input', function() {
        const value = parseFloat(this.value);
        alphaValue.textContent = value.toFixed(1);
        foo_alpha(value);
    });

    epochsSlider.addEventListener('input', function() {
        const value = parseInt(this.value);
        epochsValue.textContent = value;
        foo_nep(value);
    });
</script>