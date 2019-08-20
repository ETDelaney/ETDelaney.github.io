---
layout: post
title: Modelling Interstation Correlations - Part 2
---
Some of the discussion in a previous post - particularly within the section: Correlating displacement-displacement, vel-vel, acc-acc - were a bit verbose and convoluted (and wrong). Let us simplify everything and distill out the most important message from modelling interstation correlations. Namely, how do we model correlations that have been recorded from devices that measure displacement, velocity, or acceleration? And how do the equations look if use 1st-order or 2nd-order system of equations to solve the wave equation - if our forward model outputs $$\dot{G}$$ instead of $$G$$ when given an impulse?

<hr>

### Theory for modelling interstation correlations
As a quick reminder, the acoustic approximation of the frequency-domain representation theorem (i.e., a physical model useful for describing a single wave (scalar) type in an isotropically elastic medium) is given as follows:

\begin{equation}
  u(x,\omega) = \int{G(x,\xi,\omega)} s(\xi,\omega) d\xi
  \label{eq:repr-theory-disp}
\end{equation}

If we also assume a 1D medium, $$u(x, \omega)$$ is the traverse particle displacement recorded at $$x$$, $$G(x,\xi,\omega)$$ is the impulse response, and $$s(\xi,\omega)$$ are sources in the domain located at various locations $$\xi$$ with various frequency content.

If we choose to record random noise $$N(\xi,\omega)$$, then equation \eqref{eq:repr-theory-disp} may be written as:

\begin{equation}
  u(x,\omega) = \int{G(x,\xi,\omega)} N(\xi,\omega) d\xi
  \label{eq:repr-theory-disp-noise}
\end{equation}

Noting that we may correlate wavefields at two different receivers, such that:

\begin{equation}
  C(x_i,x_k) = u(x_i)u^*(x_k)
  \label{eq:two-rec-correlation}
\end{equation}

We can then plug equation \eqref{eq:repr-theory-disp-noise} into \eqref{eq:two-rec-correlation} to arrive at:

\begin{equation}
  C(x_i,x_k) = \int \int G(x_i,\xi) G^*(x_k,\xi') N(\xi) N^\*(\xi') d\xi d\xi'
  \label{eq:disp-disp-correlation-unsimplified}
\end{equation}


Assuming the noise sources to be uncorrelated - i.e., the phenomena that generates the noise sources is much more spatially localized (wind waves whose wavelengths are on the order of 10s of meters) than the seismic wavefields (on the order of 100s of meters) that propogates outwards such that we can say $$N(\xi) N^*(\xi') = S(\omega)\delta(\xi-\xi_0)$$, we can simplify equation \eqref{eq:disp-disp-correlation-unsimplified} to:

\begin{equation}
  C(x_i,x_k) = \int G(x_i,\xi) G^*(x_k,\xi) S(\xi) d\xi
  \label{eq:general-correlation-two-rec-simplified}
\end{equation}


Being clever, we do not need to explicitly say $$C(x_i,x_k)$$ but rather note that all correlations can be modelled for all other receiver pairs: $$C(x,x_k)$$ - which means that if we have 172 receivers and wish to model the correlations between receiver pairs 001-002, 001-003, 001-004, ..., 001-172, we model all 171 correlations in 2 calcuations - equation \eqref{eq:general-correlation-two-rec-simplified} can be further generalized to:

\begin{equation}
  C(x,x_k) = \int G(x,\xi) G^*(x_k,\xi) S(\xi) d\xi
  \label{eq:general-correlation-multi-rec-simplified}
\end{equation}

where $$x$$ defines all locations for the receivers.

A further benefit with equation \eqref{eq:general-correlation-multi-rec-simplified} is had to make no further assumptions when modelling the correlation wavefield - and we do not even have to say that the corrrelation wavefields are Green's functions: they are correlations from which we can now build an inverse modelling theory to actually image earth structure using correlated wavefields. By choosing the appropriate misfit, we can also make the forward model sensitive to imaging noise sources.

<hr>

### Velocity-stress formulation
However, when we consider the velocity-stress formulation, our representation theory will take the form:

$$v(x,\omega) = \dot{u}(x,\omega) = \int\dot{G}(x,\xi,\omega) s(\xi,\omega) d\xi $$

And \eqref{eq:general-correlation-multi-rec-simplified} will have to be rewritten to:

$${C}(x,x_k) = \int \dot{G}(x,\xi) \dot{G}^*(x_k,\xi) S(\xi) d\xi$$

And should more explicitly be denoted as:

\begin{equation}
{C}(x,x_k)_{vv} = \int \dot{G}(x,\xi) \dot{G}^*(x_k,\xi) S(\xi) d\xi
\label{eq:general-correlation-multi-rec-vel-vel}
\end{equation}

as this is a velocity-velocity correlation:

$$\dot{u}(x_i)\dot{u}^*(x_k)$$

When we look again back at \eqref{eq:general-correlation-multi-rec-simplified}, we see that this is generated from the displacement-stress formulation and gives a displacement-displacment correlation, explicitly:

\begin{equation}
  C(x,x_k)_{uu} = \int G(x,\xi) G^*(x_k,\xi) S(\xi) d\xi
  \label{eq:general-correlation-multi-rec-disp-disp}
\end{equation}

As you can see, if our machine happens to be built using the velocity-stress formulation, there is no way to manipulate the results from equation \eqref{eq:general-correlation-multi-rec-vel-vel} to arrive at the results found in equation \eqref{eq:general-correlation-multi-rec-disp-disp}: Trying to integrate $${C}(x,x_k)_{vv}\$$ will only give you $$\int {C}(x,x_k)_{vv}\ dt$$ and not change $${vv}$$ to $${uu}$$. So how can we arrive at $${C}(x,x_k)_{uu}$$ using the velocity-stress formulation?

<hr>
### Modelling displacement-displacement correlations with the velocity-stress formulation

To do this, we should instead realize that if:

$$C(x,x_k)_{uu} = \int G(x,\xi) G^*(x_k,\xi) S(\xi) d\xi$$

then:

\begin{equation}
\dot{C}(x,x_k)_{uu} = \int \dot{G}(x,\xi) {G}^*(x_k,\xi) S(\xi) d\xi
  \label{eq:general-correlation-multi-rec-disp-disp-velform}
\end{equation}

This follows from observing the changes in the representation theorems already discussed earlier:

$$u(x,\omega) = \int{G(x,\xi,\omega)} s(\xi,\omega) d\xi$$

$$\dot{u}(x,\omega) = \int\dot{G}(x,\xi,\omega) s(\xi,\omega) d\xi $$

And thus to arrive at $${C}(x,x_k)_{uu}$$ using the velocity-stress formulation, all you need to do is integrate the results from equation \eqref{eq:general-correlation-multi-rec-disp-disp-velform}:

$${C}(x,x_k)_{uu} = \int \dot{C}(x,x_k)_{uu}\ dt = \int \int \dot{G}(x,\xi) {G}^*(x_k,\xi) S(\xi) d\xi dt$$

This integration can be done on-the-fly (when interating overy each timestep: $$u = u + v*dt$$ for each timestep $$dt$$) or afterwards.

The only caveat is that in order to compute $${G}^*(x_k,\xi)$$, you must first compute $${G}(\xi,x_k)$$ by not insterting $$\delta(t)\delta(x-x_k)$$, but rather $$H(t)\delta(x-x_k)$$, where $$H(t)$$ is the heavy-side step function (the integral of a delta function). This is a numerical trick to get $$G$$ and not $$\dot{G}$$ when using the velocity-stress formulation.

<hr>
### Modelling acceleration-acceleration correlations with the velocity-stress formulation

Likewise, if we need to model acceleration-acceleration correlations, we should realize that if:

$$C(x,x_k)_{aa} = \int \ddot{G}(x,\xi) \ddot{G}^*(x_k,\xi) S(\xi) d\xi$$

then given that our machinery is the velocity-stress formulation:

\begin{equation}
\int {C}(x,x_k)_{aa}\ dt = \int \dot{G}(x,\xi) \ddot{G}^*(x_k,\xi) S(\xi) d\xi
  \label{eq:general-correlation-multi-rec-acc-acc-velform}
\end{equation}

And thus to arrive at $${C}(x,x_k)_{aa}$$ using the velocity-stress formulation, all you need to do is take the derivative of the the results from equation \eqref{eq:general-correlation-multi-rec-acc-acc-velform}:

$${C}(x,x_k)_{aa} = \partial_t \int {C}(x,x_k)_{aa}\ dt = \partial_t \int \dot{G}(x,\xi) \ddot{G}^*(x_k,\xi) S(\xi) d\xi$$

The derivative can be taken on-the-fly or afterwards.

Note that here, in order to get $$\ddot{G}^*(x_k,\xi)$$, we must first compute $$\ddot{G}(\xi,x_k)$$ by inserting $$\partial_t\delta(t)\delta(x-x_k)$$ as the initial source-time function. This is dipole source. Then apply reciprocity and conjugation to arrive at $$\ddot{G}^*(x_k,\xi)$$.

<hr>
### Summary

Let us now summarize our results on how to model these different types of correlation functions based on which forward modelling engine we use:

#### Velocity-Stress formulation

$${C}(x,x_k)_{uu} = \int \dot{C}(x,x_k)_{uu}\ dt = \int \int \dot{G}(x,\xi) {G}^*(x_k,\xi) S(\xi) d\xi dt$$

$${C}(x,x_k)_{vv} = {C}(x,x_k)_{vv} = \int \dot{G}(x,\xi) \dot{G}^*(x_k,\xi) S(\xi) d\xi $$

$${C}(x,x_k)_{aa} = \partial_t \int {C}(x,x_k)_{aa}\ dt = \partial_t \int \dot{G}(x,\xi) \ddot{G}^*(x_k,\xi) S(\xi) d\xi$$

Where the source-time function to compute $${G}(\xi,x_k)$$ $$\dot{G}(\xi,x_k)$$, $$\ddot{G}(\xi,x_k)$$ is respectively $$H(t)\delta(x-x_k)$$, $$\delta(t)\delta(x-x_k)$$, and $$\partial_t\delta(t)\delta(x-x_k)$$.


#### Displacement-Stress formulation

$${C}(x,x_k)_{uu} = {C}(x,x_k)_{uu} = {G}(x,\xi) {G}^*(x_k,\xi) S(\xi) d\xi $$

$${C}(x,x_k)_{vv} = \partial_t \int {C}(x,x_k)_{vv}\ dt = \partial_t \int {G}(x,\xi) \dot{G}^*(x_k,\xi) S(\xi) d\xi$$

$${C}(x,x_k)_{aa} = \partial_t^2 \int\int {C}(x,x_k)_{aa}\ d^2t = \partial_t^2 \int {G}(x,\xi) \ddot{G}^*(x_k,\xi) S(\xi) d\xi$$

Where the source-time function to compute $${G}(\xi,x_k)$$ $$\dot{G}(\xi,x_k)$$, $$\ddot{G}(\xi,x_k)$$ is respectively $$\delta(t)\delta(x-x_k)$$, $$\partial_t\delta(t)\delta(x-x_k)$$, and $$\partial_t^2\delta(t)\delta(x-x_k)$$.