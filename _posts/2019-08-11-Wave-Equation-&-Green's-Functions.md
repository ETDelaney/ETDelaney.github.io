---
layout: post
title: Wave Equation and Green's Functions - an intro
---

For me at least, the introduction to the term 'Green's Function' was extremely confusing. The professors had written the wave equation on the board and talked about different solutions that satisfied it. Then they would spend an excruciating amount of time talking about using a delta source as input to get out the impulse response to the system (the Green's Function)... next thing they would write the wave equation as the Helmholtz equation and then talk about monochromatic non-sense and soon we would be further lost when they wrote out the second-order wave equation in terms of first-order series of equations that would instead return the time derivative of the Green's function. No distinctions were made between a spatial delta and a temporal delta... And soon we were off on talking about monopoles and dipoles, band-limited deltas, oxymoronic terminology, convolutions, reflectivity series, reciprocity, representation theorems, and everything just became garbage. 

  ![_config.yml]({{ site.baseurl }}/images/wapenaar-green-function.PNG)

A good nightmare for all aspiring physicist - the above figure is actually one of the gentler introductions to the Green's function. It has a familiar face in quantum mechanics and areas dealing with systems linear or linearized.

<hr>

But this is all too much. Let us try to tackle the problem in small amounts. The post is titled Wave Equation and Green's Functions, but let us see how far we get. For now at least, I will try to keep it high-level and discuss numerical solving the wave equation and caveats on Green's function modelling.


### The seismic wave equation

There are many wave equations and there are many ways to derive them. If we were to start with the 1D wave equation, we would have the following:

$$\rho(x)\partial d_t\partial d_t u(x,t) - \partial d_t(\mu(x,t)\partial d_t u(x,t) = f(x,t)$$

$$\cfrac{d}{dt}\cfrac{\partial L}{\partial \dot{q}} = \cfrac{\partial L}{\partial q}$$


<hr>

#### Add math equation

No problem, Jekyll allows you to add math equation!

To add inline math, we don't use `$` like in LaTeX since it can be conflicted with a lot of actual dollar sign so we have to use `\\(` and `\\)` as opening and closing bracket instead i.e. <br> `\\(\mathbf{y} = A \mathbf{x}\\)` will render as \\(\mathbf{y} = A \mathbf{x}\\)

For one-line equation, we can use the same as LaTeX that is <br>`$$\cfrac{d}{dt}\cfrac{\partial L}{\partial \dot{q}} = \cfrac{\partial L}{\partial q}$$`. It will render as

$$\cfrac{d}{dt}\cfrac{\partial L}{\partial \dot{q}} = \cfrac{\partial L}{\partial q}$$


### Steps to getting started:

$$
\begin{align*}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{align*}
$$

testing...
