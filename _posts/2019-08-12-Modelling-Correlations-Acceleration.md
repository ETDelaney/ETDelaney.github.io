---
layout: post
title: Modelling Interfometric Correlation Functions
---

There is a far superior means of modelling interferometric wavefields than just attempting to simulate hours-on-hours of random noise (not only does such an inefficient method take time, but it also breaks memory load and is limited to high-frequency approximations of the wave equation). By exploiting the Representation Theory, we can model $$R-1$$ finite-frequency correlation wavefields with two calculations, where $$R$$ is the number of receivers in your array. And from there, it is just one additional calculation to swap out the frequency spectrum to model additional correlation wavefields and dispersion effects (especially relevant for surface wave modelling). Not only does this avoid severe limitations in Green's Function retrieval due to a lack of homogeneity in the noise source distribution, but also gives us more flexibility when attempting to image noise sources and structure (and their trade-offs) using inteferometric wavefields.


<hr>

### Theory for modelling interstation correlations

The 1D-acoustic/elastic approximation of the frequency-domain representation theorem is given as follows:

$$u(x,\omega) = \int{G(x,\xi,\omega)} s(\xi,\omega) d\xi $$

where $$u(x, \omega)$$ is the traverse particle displacement recorded at $$x$$, $$G(x,\xi,\omega)$$ is the impulse response, and $$s(\xi,\omega)$$ are sources in the domain located at various locations $$\xi$$ with various frequency content.

If we choose to record random noise $$N(\xi,\omega)$$, then our equation may be written as:

$$u(x,\omega) = \int{G(x,\xi,\omega)} N(\xi,\omega) d\xi $$

Noting that we may correlate wavefields at two different receivers, such that:

$$C(x_i,x_k) = u(x_i)u^*(x_k)$$

We can then note that:

$$C(x_i,x_k) = \int\int G(x_i,\xi) G^*(x_k,\xi') N(\xi) N^*(\xi') d\xi d\xi'$$

Assuming the noise sources to be uncorrelated - i.e., the phenomena that generates the noise sources is much more spatially localized (wind waves whose wavelengths are on the order of 10s of meters) than the seismic wavefields (on the order of 100s of meters) that propogates outwards such that we can say $$N(\xi) N^*(\xi') = S(\omega)\delta(\xi-\xi_0)$$, we can simplify the above to:

$$C(x_i,x_k) = \int G(x_i,\xi) G^*(x_k,\xi) S(\xi) d\xi $$


The benefit of this setup is that we do not need to explicitly say $$C(x_i,x_k)$$ but rather note that all correlations can be modelled for all other receiver pairs: $$C(x,x_k)$$ - which means that if we have 172 receivers and wish to model the correlations between receiver pairs 001-002, 001-003, 001-004, ..., 001-172, we model all 171 correlations in 2 calcuations:

$$C(x,x_k) = \int G(x,\xi) G^*(x_k,\xi) S(\xi) d\xi $$

where $$x$$ defines all locations for the receivers.

A further benefit is had to make no further assumptions when modelling the correlation wavefield - and we do not even have to say that the corrrelation wavefields are Green's functions: they are correlations from which we can now build an inverse modelling theory to actually image earth structure using correlated wavefields. By choosing the appropriate misfit, we can also make the forward model sensitive to imaging noise sources.

<hr>

### Velocity-stress formulation and the Representation Theory 

Recall however that we use a velocity-stress formulation for our forward modelling - e.g., in 1D where the wavefield propagates in the x-direction with energy polarized transversely in the y-direction:

$$\rho(x)\partial_t \dot{G}_y(x,t) - \partial_x \sigma_{yx}(x) = \delta_y(x,t)$$

$$\partial_t \sigma_{yx}(x) - \mu(x) \partial_x \dot{G}_y = 0$$

which means that instead of (as would be the case for the displacement-stress formulation):

$$u(x,\omega) = \int G(x,\xi,\omega) s(\xi,\omega) d\xi $$

our representation theory has taken the form:

$$\dot{u}(x,\omega) = \int\dot{G}(x,\xi,\omega) s(\xi,\omega) d\xi $$

and to make this pedantic:

$$\dot{G}(x,\omega) = \int\dot{G}(x,\xi,\omega) \delta(\xi,\omega) d\xi $$

Thus, in order to make our forward modelling engine compatible with the forward modelling, we would need to modify the equation such that:

$$u(x,\omega) = \int \dot{G} H(\xi,\omega) s(\xi,\omega) d\xi $$

where $$H(x,t) = \int^\infty_\infty \delta(t-t_0) dt \cdot \delta(x-x_0)$$, aka, the Heavy-side step fuction.

This leaves us with two options:

* 1) We must integrate our source term before we inject

* 2) We could instead integrate on-the-fly such that we go from particle velocity to particle displacement (e.g., u = u + v*dt for each time-step)

<hr>

### Correlating displacement-displacement, vel-vel, acc-acc

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

$$C(x,x_k)_{aa} = \int \dot{G}(x,\xi)\cdot{-j\omega}\dot{G}^*(x_k,\xi)\partial_t\delta(\xi)S(\xi) d\xi $$

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



$$C(x,x_k) = \int G(x,\xi) G^*(x_k,\xi) S(\xi) d\xi $$

we can note that C(x,x_k) has the units $$[m][m]$$, that $$G^*(x_k,\xi) S(\xi)$$ would have the units $$[m]$$ if we put it through the displacement-stress formulation, and that $$d\xi$$ gives us the other needed $$[m]$$ to balance the LHS and RHS of the equation:

$$[m][m] = [m][m]$$

##### Velocity-stress formulation

If we continune to use


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


