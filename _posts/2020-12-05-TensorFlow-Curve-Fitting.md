---
layout: post
title: Curve Fitting Using Neural Networks With TensorFlow
published-on: 2020-12-05T14:45:00Z
tags: Neural-Networks TensorFlow
excerpt_separator: <!--more-->
---

This post explains how to build a neural network model using TensorFlow to perform nonlinear fitting.
<!--more-->
The goal is to create a neural network model that estimates a nonlinear function with added noise $x(t)$.

In this example, $x(t)$ will be given by

$$x(t) = sin(20t) + 3t + v(t)$$,

where $v(t)$, the noise, is zero-mean Gaussian with variance 0.3.

```python
t = np.random.uniform(0, 1, size=(n))
v = np.random.normal(0, 0.3, size=(n))
x = np.sin(20*t) + 3*t + v
```

Here, $t$ defines a set of $n$ points between 0 and 1.

## Build Model

Now, we define our model.

The first layer is the input layer containing a single neuron with a linear activation function.

The next layer is a hidden layer of some number of neurons (a sufficiently large number of neurons). We can add more hidden layers to hopefully achieve smoother results. In this case, we will use a total of 3 hidden layers each with 128 neurons. Each of those hidden layer neurons will have the ReLU activation function. Feel free to adjust these parameters to see how the training results change.

The final layer is the output layer which, as in the input layer, contains a single neuron with a linear activation function.

```python
model = keras.Sequential()
model.add(layers.Dense(1, input_shape=[1]))
model.add(layers.Dense(128, activation = 'relu'))
model.add(layers.Dense(128, activation = 'relu'))
model.add(layers.Dense(128, activation = 'relu'))
model.add(layers.Dense(1))
```

## Train Model

We are ready to train the model.

For the case of curve fitting, the most appropriate loss function would be the mean squared error (MSE).

We will also be using the Adam optimization algorithm to update the weights in our network. We can certainly use stochastic gradient descent, but [sources](https://machinelearningmastery.com/adam-optimization-algorithm-for-deep-learning/) seem to state that Adam is a much better replacement to stochastic gradient descent.

```python
model.compile(loss='mse', optimizer='adam')
```

Let's see what happens after just a few epochs:

```python
model.fit(t, x, epochs=3)
y_pred = model.predict(t)
```

![1](/assets/img/20201205-1.png)

Clearly the result is not a sine wave as we would expect, but the resulting line does cut through the sine wave that we would expect to see.

After training the model for a few more epochs, we see the following result:

![2](/assets/img/20201205-2.png)

As you can see, the result shows the line bent on the lower end as a sign of progress from training. This means that the longer we train the model, the more we will see the red line try to "bend" itself as the way to fit itself to the curve.

The model will need to run a sufficient number of epochs in order to compute the weights that will give us the best fit line for our curve. Using the model above, I found that 700 epochs were enough to reach convergence.

![3](/assets/img/20201205-3.png)

Here is a plot that shows the computed loss (MSE) per epochs during the training:

![4](/assets/img/20201205-4.png)

Tying all of the above together, the complete code to achieve our trained model is provided below.

```python
import numpy as np
import plotly.graph_objects as go # for plotting
from tensorflow import keras
from tensorflow.keras import layers
n = 300
t = np.random.uniform(0, 1, size=(n))
v = np.random.normal(0, 0.3, size=(n))
x = np.sin(20*t) + 3*t + v
model = keras.Sequential()
model.add(layers.Dense(1))
model.add(layers.Dense(128, activation = 'relu'))
model.add(layers.Dense(128, activation = 'relu'))
model.add(layers.Dense(128, activation = 'relu'))
model.add(layers.Dense(1))
model.compile(loss='mse', optimizer='adam')
model.fit(t,x, epochs=700)
y_pred = model.predict(t)
fig = go.Figure(
    data=[
        go.Scatter(
            x=t,
            y=x,
            mode="markers",
            name="x"
        ),
        go.Scatter(
            x=t,
            y=y_pred.flatten(),
            mode="markers",
            name="y_pred"
        ),
    ],
    layout=go.Layout(
        xaxis_title='t',
        yaxis_title='y',
    )
)
fig.show()
```

## Test Model

When the training is over, we will test the model by first defining a new line that will be used to estimate the nonlinear function:

```python
t1 = np.random.uniform(0, 1, size=(100))
```

This is similar to the $t$ defined before but of a smaller size.

Then, we feed $t_1$ to the model.

```python
y1_pred = model.predict(t1)
```

Here is a plot of the output points after feeding $t_1$ to the model:

![5](/assets/img/20201205-5.png)

Visually we can see that the curve does not look entirely like a smooth sine wave as some peaks are rather sharp. The model seems to have converged in this fashion as a result of computing the MSE after each epoch. From this, we conclude that the red waveform you see is the best fit line (that is, after 700 epochs during training) that estimates the nonlinear function.

## Remark

In the above example, both the training and testing use input values ranging from 0 to 1. What would you expect the result to look like if we were to feed a larger range to the model?

Defining a new set of input values over a larger range (from -1 to 2) which is fed to the model, we receive the following result:

![6](/assets/img/20201205-6.png)

The result is due to the fact that we did not train the model for points outside of the range $[0,1]$. In order to have the ends of the curve bend in the same way as the points between 0 and 1, we need to have our training set include all points between -1 and 2.

