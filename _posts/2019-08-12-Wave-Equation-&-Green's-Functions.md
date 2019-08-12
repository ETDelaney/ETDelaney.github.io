---
layout: post
title: Wave Equations and Green's Functions - an intro
---

For me at least, the introduction to the term 'Green's Function' was extremely confusing. The professors had written the wave equation on the board and talked about different solutions that satisfied it. Then they would spend an excruciating amount of time talking about using a delta source as input to get out the impulse response to the system (the Green's Function)... next thing they would write the wave equation as the Helmholtz equation and then talk about monochromatic non-sense and soon we would be further lost when they wrote out the second-order wave equation in terms of first-order series of equations that would instead return the time derivative of the Green's function. No distinctions were made between a spatial delta and a temporal delta... And soon we were off on talking about monopoles and dipoles, band-limited deltas, oxymoronic terminology, convolutions, reflectivity series, reciprocity, representation theorems, and everything just became garbage. 

  ![_config.yml]({{ site.baseurl }}/images/wapenaar-green-function.PNG)

A good nightmare for all aspiring physicist - the above figure is actually from a work whose authors (Kees Wapenaar and Guss Berkout) give one of the gentler introductions to the Green's function and seismic wave propagation theory. Outside of exploration seimsmology, Green's function are a familiar face in quantum mechanics and areas dealing with systems linear or linearized - and in wave phenomena in general (e.g., medical imaging, EM, etc.).


But this is all too much. Let us try to tackle the problem in small amounts. The post is titled Wave Equations and Green's Functions, but let us see how far we get. For now at least, I will try to keep it high-level and discuss numerical solving the wave equation and caveats on Green's function modelling.

<hr>

### The seismic wave equation - 1D

There are many wave equations and there are many ways to derive them. If we were to start with a 1D elastic wave equation that is descriptive of tranverse waves, we would have the following:

$$\rho(x)\partial^2_t u(x,t) - \partial_x \mu(x) \partial_x u(x,t) = f(x,t)$$

where $$u(x,t)$$ is particle displacement, $$\rho(x)$$ and $$\mu(x)$$ are respectively density and the elastic modulus, and $$f(x,t)$$ is our source time function. You will notice that it is written with everything multiplied by -1 when compared with the equation in the figure above.

Here, the wave propagates in the x-direction, but the displacement is perpendicular to this propagation. Let us make it explicit and say displacement is polarized in the y-direction - due to a y-directional forcing term:

$$\rho(x)\partial^2_t u_y(x,t) - \partial_x \mu(x) \partial_x u_y(x,t) = f_y(x,t)$$

In such cases where we assume fluids or gases, the elastic modulus is necessarily 0 - and the material is no longer resistive to shear forces and is only capable of transmitting energy in the same direction in which the wave propagates. Such phenomena may be written in the form of a scalar or acoustic equation. It cannot be derived directly from the above and we ought to start at a less simplified form of the 3D elastic wave equation that accounts for longitudinal waves (which we will address in a later blog post). You could also work this equation out through physical arguments (again, something for later). Anyways, the equation boils down to:

$$\partial^2_t p(x,t) - c(x)^2\partial^2_x p(x,t) = s(x,t)$$

where $$p(x,t)$$ is the pressure field, $$c(x)$$ is the acoustic velocity (given $$c(x) = \frac{\kappa(x)}{\rho(x)}$$ where $$\kappa(x)$$ is the bulk modulus), and $$s(x,t)$$ is the source time function in terms of pressure field (and not a forcing term). A forcing term is similar to your plucking a string, whereas a pressure field is induced by an air blast (or simply talking). By simply turning all $$x$$ into vectors and rewriting $$\partial_x$$ to $$\nabla$$, we may readily transform the above equation to higher dimensions.

There are funny properties when dealing with particle displacements or pressure waves - i.e., if you tried to record the pressure wave at the sea surface due to reflected waves generated from an airgun, you would get nothing (this is the reason to tow the receivers lower) - and even though the pressure field may be said to be negligible on the surface of the earth (a free surface), the particle displacement from an earthquake is maximized (i.e., violent shaking) as there is no mass to resist the vertical transverse wave components.

<hr>

### Displacement-stress versus velocity-stress fomulation - 1D and 2D

#### 1D:

The 1D elastic equations written above are in terms of a displacement-stress formulation. We may instead take the derivative with respect to time of the following:

$$\rho(x)\partial^2_t u_y(x,t) - \partial_x \mu(x) \partial_x u_y(x,t) = f_y(x,t)$$

...to arrive at a 1st-order system of equations (having replaced $$u_y$$ with $$v_y$$):

$$\rho(x)\partial_t v_y(x,t) - \partial_x \sigma_{yx}(x) = f_y(x,t)$$

$$\partial_t \sigma_{yx}(x) - \mu(x) \partial_x v_y = 0$$

#### 2D:

In 2D, let us say we wish to measure SH-waves - source and receiver lie in the plane and the waves are perpendicular to this plane. This may be a good enough approximation to surface waves. Let us say our source and receiver lie in the plane where waves propagate in the XZ-plane, with energy polarized in the y-direction. The source would actually be a line source that goes out of the XZ-plane infinitely and we are dealing with cylindrical waves. Should we actually care about this when we know in nature we are actually using point sources?

Anyways - in terms of the displacement-stress formulation, we would have:

$$\rho(x,z)\partial^2_t u_y(x,z,t) - (\partial_x [\mu(x,z) \partial_x u_y(x,z,t)] + \partial_z [\mu(x,z) \partial_z u_y(x,z,t)]) = f_y(x,z,t)$$

All these variables clutter up the equation, so let's remove them:

$$\rho\partial^2_t u_y - (\partial_x [\mu \partial_x u_y] + \partial_z [\mu \partial_z u_y]) = f_y$$

And if we write this in terms of velocity-stress:

$$\rho\partial_t v_y - (\partial_x \sigma_{xy} + \partial_z \sigma_{zy}) = f_y$$

...where I have left out the second equation in the first-order coupling.


<hr>

### Green's Functions

As stated in my rant above - some professors would often say the Green's function is the response of the system to the an impulse, which is the definition - but leaves us with little intuition. In our case here, the system is the wave equation - which is linear in terms of source terms. I.E., if I double the force of my source, the displacements also double; if I hit the ground two seconds later, the wavefield will also arrive two seconds later than if I hit the ground earlier. What this means is that... once I know what the impulse response is for that given system - then I can predict the response for any other source input. This is the point at which you can write your modelled wavefield in terms of a representation theorem - which shows us the linear mapping of the source to the receiver is linked through the Green's function kernel. Again, Green's functions are wonderful for modelling.

It is important to note that the wave equation is not linear in its material parameters - density, bulk modulus, etc.... a doubling of density will not result in some halving or doubling for the displacement field (or that this will somehow halve the arrival time). Small changes in material properties result in certain linear behaviors that allow us to linearize the wave equation in terms of model parameters - but again, this is way outside the purpose of the Green's function. Bottomline - if your material parameters change (and hence your linear system), you need to re-calculate your Green's functions.

In practical terms, an explosion is a bandlimited delta. This means that the shot record that is recorded is the bandlimited Green's function for the given source-receiver-Earth setup. For extremely simplied problems (1D) - the reflectivity series derived from sonic and velocity logs can also be considered the Green's Function. Bandlimiting this Green's Function by convolving it with a wavelet (e.g., a 20 Hz Ricker) will give us a synthetic which we can compare with the bandlimited seismic - i.e., the seismic-to-well tie.

Anyways, in terms of equations, it is easy - but may not be helpful for neophytes:

#### Acoustic case:

For any source term - we get a pressure wavefield $$p$$, such as:

$$\partial^2_t p(x,t) - c^2(x)\partial^2_x p(x,t) = s(x,t)$$

But now, for a delta pulse, we get the Green's function:

$$\partial^2_t G(x,t) - c^2(x)\partial^2_x G(x,t) = \delta(x,t)$$

It is probably better to explicitly write the delta in terms of its delta nature in both space and in time and ensure that:

$$\int^\infty_\infty \int^\infty_\infty \delta(x-x_0)\delta(t-t_0) dx dt = 1 $$

Numerically, this means that if you have spatially divided your gridding such that dx = 10, and dy = 10, then you better divide your delta source-time function by 100.

And thus:

$$\partial^2_t G(x,t) - c(x)^2\partial^2_x G(x,t) = \delta(x-x_0)\delta(t-t_0)$$

Once we have found the Green's function, we can thus model any response by invoking the representation theorem:

$$u(x,t) = \int\int G(x,t) s(x,t) dxdt $$

You should notice that in the case where $$s(x,t)$$ is a delta, the integrals drop out and we have $$u(x,t) = G(x,t)$$. The power in the representation theorem is that if we can write equations in this form, we can numerically model the waveform $$u(x,t)$$ by inserting whatever we want into $$s(x,t)$$ - where $$G(x,t)$$ thus contains the Earth information. It is a linear map from s to u. Such observations will be helpful in modelling correlation functions for seismic interferometry.

In such a case where we have a homogeneous medium, there exist anayltical solutions to the Green's function for 1D, 2D, and 3D. We can compare these to our numerical solutions and see if we are modelling the physics appropriately.

#### Elastic case:

Again, just replace terms to arrive at:

$$\rho(x)\partial^2_t G_y(x,t) - \partial_x \mu(x) \partial_x G_y(x,t) = \delta_y(x,t)$$

And for the velocity-stress formulation:

$$\rho(x)\partial_t \dot{G}_y(x,t) - \partial_x \sigma_{yx}(x) = \delta_y(x,t)$$

$$\partial_t \sigma_{yx}(x) - \mu(x) \partial_x \dot{G}_y = 0$$

<hr>

### Important numerical tricks with the Representation Theorem:

Notice how in the velocity-stress formulations, we do not get the Green's function, but the time derivative of the Green's function. If our theory asks that we should use $$\dot{G}$$, this is okay - or if we are measuring particle velocity. But if we want to actually get the Green's function (with units of displacement), we should note that if have a system that give us:

$$v(x,t) = \int\int \dot{G} s(x,t) dxdt $$

and thus:

$$\dot{G}(x,t) = \int\int \dot{G} \delta(x,t) dxdt $$

Then we should modify it such that:

$$G(x,t) = \int\int \dot{G} H(x,t) dxdt $$

where $$H(x,t) = \int^\infty_\infty \delta(t-t_0) dt \cdot \delta(x-x_0)$$, aka, the Heavy-side step fuction.

Thus, we could arrive at what we desire with the velocity-stress formulation in terms of displacement by noting:

$$u(x,t) = \int\int \dot{G} H(x,t) s(x,t) dxdt $$

Which then implies that when you are plugging your source-time function into the velocity-stress formulation, if you to get your wavefield in terms of displacements, you should integrate your injection:

$$\rho(x)\partial_t G_y(x,t) - \partial_x \sigma_{yx}(x) = H_y(t)\delta(x-x_0)$$

Of course, you could just get the velocity field and then integrate on-the-fly via u = u + v*dt per time-step.

#### Acceleration and dipoles?

However, what about if your measurements are done with an accelometer and you need to model accelerations? To get $$\ddot{G}$$ using the stress-velocity formulation, simply inject a dipole source:

$$\partial_t \delta(t-t_0) \delta(x-x_0) $$

Some people enjoy confusing others with monopole and dipole terminology - this is simply delta and the time-derivate of a delta, respectively.

Numerically, you can model a dipole with a string of zeroes, and then giving the first index a 1 and the second index a -1. Nonetheless, you could always just take the derivative of your velocity wavefields on-the-fly or afterwards.

<hr>

Well, if you got this far impressive! I will definitely be referring back to this - as a good reference guide whenever the waters get muddied and the physics is lost in the terminology. But this is just the tip of the iceberg when it comes to this topic - more reference material around these topics, which should eventually culminate towards addressing seismic interferometry... to follow!
