---
layout: post
title:  "[ML02] Other Loss Functions: Stochastic Gradient Descent, Regularization"
date:   2024-08-01 18:00:00 +0530
categories: blogs
author: 'Aritra Mukhopadhyay'
---

<script src="https://www.desmos.com/api/v1.9/calculator.js?apiKey=dcb31709b452b1cf9dc26972add0fda6"></script>

Welcome back to our machine learning series! In our previous post, [[ML01] Introduction to Linear Regression and Gradient Descent]({{site.baseurl}}/blogs/2024/03/24/linear-regression-and-gradient-descent.html), we explored the fundamentals of machine learning and delved into the world of linear regression. We learned how to approach a **regression task**, where the goal is to predict a continuous output variable based on one or more input features. For example, predicting house prices (which can take any value within a range) based on related features is a classic regression task, whereas classifying an image as either belonging to a cat or a dog would be a classification task.

In that post, we introduced the concept of a loss function, which measures the difference between our model's predictions and the actual true values. We also implemented gradient descent to minimize this loss function and optimize our linear regression model. Specifically, we used the mean squared error (MSE) loss, defined as $$L = \frac{1}{m} \sum_{i=1}^{m} (y_i - \hat{y}_i)^2$$ but hinted that there are many other types of loss functions suitable for different machine learning tasks.

In this post, we'll build upon our understanding of gradient descent and explore two new topics: customized loss functions and regularization techniques. We'll start by introducing stochastic gradient descent, a variant of gradient descent that's particularly useful for large datasets. Then, we'll shift our focus to classification tasks, where the goal is to predict a categorical output variable based on input features. To tackle this type of problem, we'll introduce the cross-entropy loss function and discuss its importance in machine learning.

By the end of this post, you'll have a deeper understanding of how to choose and implement the right loss function for your specific machine learning task, as well as techniques to prevent overfitting and improve model generalization. Let's dive in!





<div id="calculator" style="width: 100%; height: 400px;"></div>

<script>
    var elt = document.getElementById('calculator');
    var calculator = Desmos.GraphingCalculator(
        elt,
        options={
            keypad: false,
            settingsMenu: false,
            showResetButtonOnGraphpaper: false,
            expressionsTopbar: false,
        },
    );

    calculator.setMathBounds({ left: -5, right: 5, bottom: -0.1, top: 1.1})

    calculator.setExpressions([
        {latex:'\\frac{1}{1 + e^{-(ax+c)}}', color:Desmos.Colors.PURPLE},
        {type: 'text', text: 'tweak the sliders $a$ and $c$ below to see how the above sigmoid function changes'},
        {latex:'a=2', sliderBounds: {min: -5, max: 5}},
        {latex:'c=0', sliderBounds: {min: -10, max: 10}},
    ]);
</script>

