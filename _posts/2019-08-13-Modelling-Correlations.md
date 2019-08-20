---
layout: post
title: Modelling Interfometric Correlation Functions
---

There is a far superior means of modelling interferometric wavefields than just attempting to simulate hours-on-hours of random noise (not only does such an inefficient method take time, but it also breaks memory load and is limited to high-frequency approximations of the wave equation). By exploiting the Representation Theory, we can model $$R-1$$ finite-frequency correlation wavefields with two calculations, where $$R$$ is the number of receivers in your array. And from there, it is just one additional calculation to swap out the frequency spectrum to model additional correlation wavefields and dispersion effects (especially relevant for surface wave modelling). Not only does this avoid severe limitations in Green's Function retrieval due to a lack of homogeneity in the noise source distribution, but also gives us more flexibility when attempting to image noise sources and structure (and their trade-offs) using inteferometric wavefields.


<hr>

### Theory for modelling interstation correlations

The acoustic approximation of the frequency-domain representation theorem (i.e., a physical model useful for describing a single wave (scalar) type in an isotropically elastic medium) is given as follows:

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

### Velocity-stress formulation and the Representation Theory

Recall however that we use a velocity-stress formulation for our forward modelling - e.g., in 1D where the wavefield propagates in the x-direction with energy polarized transversely in the y-direction:

$$\rho(x)\partial_t \dot{G}_y(x,t) - \partial_x \sigma_{yx}(x) = \delta_y(x,t)$$

$$\partial_t \sigma_{yx}(x) - \mu(x) \partial_x \dot{G}_y = 0$$

which means that instead of (as would be the case for the displacement-stress formulation):

$$u(x,\omega) = \int G(x,\xi,\omega) s(\xi,\omega) d\xi $$

our representation theory has taken the form:

$$\dot{u}(x,\omega) = \int\dot{G}(x,\xi,\omega) s(\xi,\omega) d\xi $$

<hr>

### Being pendantic

By properties of the delta function, it goes without saying that $$s(\xi,\omega)$$ can actually be written as:

$$s(\xi,\omega) = \delta(\xi)\hat{s}(\xi,\omega)$$

where $$\delta(\xi)$$ encapsulates the physical point-source density with units $$\frac{[N]}{[m]}$$ and $$\hat{s}(\xi,\omega)$$ encodes the unitless phase and amplitude spectra.

Let us now consider the case where we choose to inject monopole at one point in our domain - in the time domain, this would be written as:

$$s(\xi,t) = \delta(\xi-\xi_0) \cdot \delta(t-t_0)$$

And into the frequency domain:

$$s(\xi,\omega) = \mathcal{F}\{\delta(t-t_0) \cdot \delta(x-x_0)\} = \delta(\xi-\xi_0) \cdot 1$$

Hence, $$\hat{s}(\xi,\omega) = 1$$ for a monopole injection. With that we can proceed to showing when a monopole is injected into the velocity-stress formulation, we do not recover the Green's function, but the time-derivative of the the Green's function:

$$\dot{u}(x,\omega) = \int\dot{G}(x,\xi,\omega) s(\xi,\omega) d\xi $$

$$ = \int\dot{G}(x,\xi,\omega) \delta(\xi) \hat{s}(\xi,\omega) d\xi $$

$$ = \int\dot{G}(x,\xi,\omega) \delta(\xi- \xi_0) \cdot 1 d\xi $$

$$ = \dot{G}(x,\xi_0,\omega) $$

Naturally, if we choose to extend this beyond one monopole source, but to all monopole sources in the domain located at $$\xi$$ and recorded at $$x$$, we arrive at:

$$\dot{G}(x,\omega) = \int\dot{G}(x,\xi,\omega) \delta(\xi,\omega) d\xi $$

where we have chosen to not explicity state the spectra for all monopoles are necessarily $$1$$.

<hr>

### Integrating the monopole


Thus, in order to make our forward modelling engine such that we do not get the $$dG/dt$$, but $$G$$, we need to integrate the monopole source to arrive at the heavy-side step function:

$$H(x,t) = \int^\infty_\infty \delta(t-t_0) dt \cdot \delta(x-x_0)$$


$$u(x,\omega) = \int \dot{G} H(\xi,\omega) s(\xi,\omega) d\xi $$


This leaves us with two options:

* 1) We must integrate our source term before we inject

* 2) We could instead integrate on-the-fly such that we go from particle velocity to particle displacement (e.g., u = u + v*dt for each time-step)

<hr>

### Correlating displacement-displacement, vel-vel, acc-acc
(some dubious things in this section - refer to a later blog post that corrects and simplifies this section and distills the most important aspects of modelling interstation correlations)

The above is not an issue if we wish to model correlated velocity fields: $$\dot{u}(x)\dot{u}^*(x_k)$$ as we can rewrite the governing equation as:

$$\dot{u}(x,\omega) = \int{\dot{G}(x,\xi,\omega)} N(\xi,\omega) d\xi $$

$$C(x,x_k)_{vv} = \int \dot{G}(x_i,\xi) \dot{G}^*(x_k,\xi) S(\xi) d\xi $$


However, when we look at our forward model framework, this has some implications if we wish to model correlated displacement fields or acceleration fields, respectively:

* $$u(x_i)u^*(x_k)$$
* $$\ddot{u}(x)\ddot{u}^*(x_k)$$

Recall again the governing equation for interstation correlations:

$$C(x,x_k) = \int G(x,\xi) G^*(x_k,\xi) S(\xi) d\xi $$

* 1) For modelling displacement correlations in the velocity-stress formulation, we must first insert a heavy-side step function to get $$G(\xi,x_k)$$ as opposed to $$\dot{G}(\xi,x_k)$$

* 2) After multiplying by the $$S(\xi)$$, we need to integrate $$G^*(x_k,\xi) S(\xi)$$ before injecting into the second calculation.

* 3) This implies a forward modelling engine as such that to go from modelling correlated velocity fields:

$$C(x,x_k)_{vv} = \int \dot{G}(x,\xi) \dot{G}^*(x_k,\xi) S(\xi) d\xi $$

to displacement fields, we must instead use this form of an engine:

$$C(x,x_k)_{uu} = \int \dot{G}(x,\xi) \frac{1}{-j\omega}\dot{G}^*(x_k,\xi)H(\xi)S(\xi) d\xi $$

Likewise for correlating acceleration fields, we must emulate this form of an engine:

\begin{equation}
  C(x,x_k)_{aa} = \int \dot{G}(x,\xi)\cdot{-j\omega}\dot{G}^*(x_k,\xi)\partial_t\delta(\xi)S(\xi) d\xi
  \label{eq:aa-correlation}
\end{equation}


I refer you to equation \eqref{eq:aa-correlation}.

<hr>

### Overkill?

But another question to ask is whether we need to go through all this trouble? If we want to get displacement correlations, we just need to make sure that we integrate on-the-fly when we run the first calculation and make sure to integrate on-the-fly again when we run the second calculation.

For acceleration correlations, we could just do a finite-difference scheme on-the-fly as well and just do it again for the second calculation.

<hr>

### Symmetry

Are correlated wavefields supposed to symmetric or anti-symmetric? In Green's function retrieval theory, there has been discussion about the asymmtery. In previous anaylses of correlated wavefields and their modelling, I perhaps did not take full ownership on the dimensions and just took derivatives or integrals to force symmetry - this may not have been the brightest idea - and even from looking at real data, it is hard to tell if the acausul and causal sides are symmetric:

  ![_config.yml]({{ site.baseurl }}/images/correlations-asymmetry-1.PNG)

  ![_config.yml]({{ site.baseurl }}/images/correlations-asymmetry-2.PNG)


Clearly, it is not fully obvious if the data suggests symmetry. Nonetheless, instead of worrying about, let us just make sure that we have adhered to the correct units via a short aside into dimensional analysis.

<hr>

### Dimensional analysis

#### Displacement-displacement correlations

##### Displacement-stress formulation

Looking at:

$$u(x,\omega) = \int{G(x,\xi,\omega)} N(\xi,\omega) d\xi $$

we can note that $$u(x)$$ has units $$[m]$$, $$N(\xi)$$ is a forcing term with units $$\frac{[N]}{[m]}$$, and $$d\xi$$ gives us $$[m]$$ - this then implies that $$G(x,\xi)$$ must have units $$\frac{[m]}{[N]}$$ to balance out the LHS and RHS:

$$[m] = \frac{[m]}{[N]} \frac{[N]}{[m]} [m]$$

As discussed above, if we then continue onto computing the displacement-displacement correlation, we arrive at:

$$C(x,x_k)_{uu} = \int\int G(x,\xi) G^*(x_k,\xi') N(\xi) N^*(\xi') d\xi d\xi'$$

from which we can note the units necessarily as:

$$[m][m] = \frac{[m]}{[N]} \frac{[m]}{[N]} \frac{[N]}{[m]} \frac{[N]}{[m]} [m][m]$$

##### Power-spectral density units

Moving onto invoking the simplification - where our noise sources are uncorrelated:

$$N(\xi) N^*(\xi') = S(\omega)\delta(\xi-\xi_0)$$

we see that the units are simply:

$$\frac{[N]}{[m]} \frac{[N]}{[m]} = \frac{[N]}{[m]} \frac{[N]}{[m]}$$

which shows us that $$\delta(\xi-\xi_0)$$ necessarily also has units $$\frac{[N]}{[m]}$$, even though it will integrate to $$1$$ and cancel out:

$$C(x,x_k) = \int\int G(x,\xi) G^*(x_k,\xi') S(\xi')\delta(\xi'-\xi'_0) d\xi d\xi' $$

$$C(x,x_k) = \int G(x,\xi) G^*(x_k,\xi) S(\xi) d\xi $$

Re-analyzing the units for the two previous equations, we can notice that in the simplification we go from:

$$[m][m] = \frac{[m]}{[N]} \frac{[m]}{[N]} \frac{[N]}{[m]} \frac{[N]}{[m]} [m][m]$$

to:

$$[m][m] = \frac{[m]}{[N]} \frac{[m]}{[N]} \frac{[N]}{[m]} {[N]} [m]$$

Thus, $$S(\xi)$$ - the power-spectral density - has units $$\frac{[N^2]}{[m]}$$, which differs to that of $$S(\omega)$$ before the simplifcation occurs.



##### Velocity-stress formulation

In this formulation, we now have:

$$\dot{u}(x,\omega) = \int\dot{G}(x,\xi,\omega) s(\xi,\omega) d\xi $$

whose units for $$\dot{u}(x)$$, $$N(\xi)$$, and $$d\xi$$ are respectively $$\frac{[m]}{[s]}$$, $$\frac{[N]}{[m]}$$, and $$[m]$$, which means that $$\dot{G}(x)$$ has units $$\frac{[m]}{[N][s]}$$. This is consistent as $$\frac{dG(x)}{dt}$$ would be the units of $$G$$ divided by $$s$$.

We thus note the units for the Representation Theorem for velocity-stress formulation as:

$$\frac{[m]}{[s]} = \frac{[m]}{[s][N]} \frac{[N]}{[m]} [m]$$

Remember: write some notes here on integrals and derivatives

This means


#### A little stuck on dimensional analysis - 1D analysis:

Looking at:

$$u(x,\omega) = \int{G(x,\xi,\omega)} N(\xi,\omega) d\xi $$

we can note that $$u(x)$$ has units $$[m]$$, $$N(\xi)$$ is a forcing term with units $$\frac{[N]}{[m]}$$, and $$d\xi$$ gives us $$[m]$$ - this then implies that $$G(x,\xi)$$ must have units $$\frac{[m]}{[N]}$$ to balance out the LHS and RHS:

$$[m] = \frac{[m]}{[N]} \frac{[N]}{[m]} [m]$$


Does that make sense that the Green's function has units - or am I missing something here?

#### Furthemore:

If we note:

$$C(x,x_k) = \int\int G(x,\xi) G^*(x_k,\xi') N(\xi) N^*(\xi') d\xi d\xi'$$

we can write out the units as:

$$[m][m] = \frac{[m]}{[N]} \frac{[m]}{[N]} \frac{[N]}{[m]} \frac{[N]}{[m]} [m][m]$$

However, things seem to go weird if we say that:

$$N(\xi) N^*(\xi') = S(\omega)\delta(\xi-\xi_0)$$

and that:

$$C(x,x_k) = \int G(x,\xi) G^*(x_k,\xi) S(\xi) d\xi $$

then we have unit issues, since $$S(\xi)$$ has units $$\frac{[N^2]}{[m^2]}$$ which are only partially cancelled by $$d\xi$$:

$$[m][m] \neq \frac{[m]}{[N]} \frac{[m]}{[N]} \frac{[N^2]}{[m^2]} [m]$$


we can note that $$C(x,x_k)$$ has the units $$[m][m]$$, that $$G^*(x_k,\xi) S(\xi)$$ would have the units $$[m]$$ if we put it through the displacement-stress formulation, and that $$d\xi$$ gives us the other needed $$[m]$$ to balance the LHS and RHS of the equation:

$$[m][m] = [m][m]$$