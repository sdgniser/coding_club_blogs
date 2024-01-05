---
layout: post
title:  "Why Neural Network"
date:   2023-11-25 
categories: jekyll update
author: 'Shriman Keshri'
---

# Neural Networks and ReLU

Hello, everyone. I recently gave a talk on neural networks at the NISER Coding Club. The address was intended for a general audience, so I avoided using technical jargon.

In this post, I will convey the same information, but before I begin, I would like to point out that this blog contains some exercises designed for you to absorb the material fully. If you are ready, let's start.

## ReLU

The ReLU function is defined as: 

$$
\sigma(x) = \max(0,x)
$$

![img]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1701949023644_0.png)

### Examples of ReLU

Let's explore variations of the ReLU function through some examples. I encourage you to practice by drawing the graph on paper before checking against the provided plots.

$$\sigma(x+1)$$

![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1701949545078_0.png)

$$\sigma(x-1)$$

![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1701949568750_0.png)

$$\sigma(x-1) +1$$

![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1701949589726_0.png)

$$\sigma(-x)$$

![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1701949673223_0.png)

$$\sigma(-x-1)$$

![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1702131097800_0.png)

### Some More Examples

$$\sigma(x)- \sigma(x-1)$$

![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1702131123809_0.png)

$$\sigma(x-0) -\sigma(x-1) + \sigma(x-2) - \sigma(x-3)$$


![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1702131227572_0.png)

$$\sigma(x-0) -\sigma(x-1) + \sigma(x-2) - \sigma(x-3) -\sigma(x-4) +\sigma(x-6)$$

![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1702131355502_0.png)


### Some Exercise
This Exercise will make the above Idea concrete in your mind, and you will have no problem understanding the next part of the blog.
1. Plot the graph of the following function
	$$-\sigma(x-1)$$
2. Plot the graph of the following function
	$$\sigma(x-2)$$
3. Plot the graph of the following function
	$$-\sigma(x-3)$$
4. If you can plot the above, try poling the combination of the above two:
	$$\sigma(x-2) -\sigma(x-3)$$
Very good.

## Neural network.
![]({{site.baseurl}}/assets/img/why_neural_network.assets/500px-Colored_neural_network.svg%5B1%5D.png)

I took the above picture from the Wikipedia [page](https://en.wikipedia.org/wiki/Artificial_neural_network). If you look here, you will get many things about its application, model, and a lot more, but now we will look at a particular example of a neural network and see how it works. Let's begin.
This is how a simple neuron looks; if we write it mathematically, it's a function $$y:\mathbb{R}\to\mathbb{R}$$.
$$y(x) = \sigma(Wx + B)$$
Where $$\sigma$$ is the ReLU function that we looked at above.So $$\sigma(x)$$ will look like:

![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1702371328933_0.png)


### Examples
We will repeat the above example and see how they look as neural networks.

$$\sigma(x+1)$$

![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1702371370889_0.png)
![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1701949545078_0.png)

$$\sigma(x-1)$$

![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1702371449963_0.png)
![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1701949568750_0.png)

$$\sigma(x-1) +1$$

This one is a little tricky.
![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1702371602267_0.png)
The above Neural Network represents $$\sigma(1 \times x-1) + \sigma(0 \times x +1) = \sigma(x-1) +1$$
![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1701949589726_0.png)

$$\sigma(-x)$$

![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1702371805523_0.png)
![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1701949673223_0.png)

$$\sigma(-x-1)$$

![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1702371833558_0.png)
![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1702131097800_0.png)
		
### Some More Examples

$$\sigma(x)- \sigma(x-1)$$

![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1702372167844_0.png)
![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1702131123809_0.png)

$$\sigma(x-0) -\sigma(x-1) + \sigma(x-2) - \sigma(x-3)$$

![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1702372498180_0.png)
![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1702131227572_0.png)

$$\sigma(x-0) -\sigma(x-1) + \sigma(x-2) - \sigma(x-3) -\sigma(x-4) +\sigma(x-6)$$

![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1702372797118_0.png)
![image.png]({{site.baseurl}}/assets/img/why_neural_network.assets/image_1702131355502_0.png)

### Some Exercise
This Exercise will make the above Idea concrete in your mind, and you will have no problem understanding Neural networks.
1. Plot the graph of the following function
$$-\sigma(x-1)$$
2. Plot the graph of the following function
$$\sigma(x-2)$$
3. Plot the graph of the following function
$$-\sigma(x-3)$$
4. If you can plot the above, try poling the combination of the above two:
$$\sigma(x-2) -\sigma(x-3)$$

Very good.
