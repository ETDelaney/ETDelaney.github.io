---
layout: post
title: Avoiding interfometry artifacts when modelling correlations
---
When modelling correlation functions, we are prone to artifacts that may be related to the absorbing boundaries, the failure to satisfy the Courant criteria, not sampling the wavefield at least 10 times per wavelength (you need to do much better than Nyquist here), not discretizing the frequency vector properly (if you are doing on-the-fly Fourier transforms), etc. Most of these are easy to spot - however, I encountered an artifact that was a bit strange:

  ![_config.yml]({{ site.baseurl }}/images/correlation-model-short-time.png)

The acausal part is from receiver pair 036-001, whereas the causal is from 001-036. The reason for the lack of symmetry is due to a strong noise source presence north of the array - at the location of the Oseberg C platform - see http://www.geo.uu.nl/~fichtner/papers/2017_Delaney_Geophysics.pdf:

  ![_config.yml]({{ site.baseurl }}/images/source-structure-layout-SWIM.png)


There are two obvious problems here. We see that the acausal part suffers from low frequency noise and that there is some strange negative polarity artifact not present when looking at the correlation 001-036.

These problems simply result from not allowing the Green's function to propogate long enough before you convolve it with the noise source distribution. Simply increase the time vector to enable the Green's function to propagate for a long enough time... problem solved:

  ![_config.yml]({{ site.baseurl }}/images/correlation-model-longer-time.png)

These effects or probably most like most noticeable when modelling in 2D, simply because the Green's function in 2D is not finite and has a tail that extends to infinite time. We need to extend the time propogation a bit longer to capture a significant part of this tail. Let us look at the modelled Green's function at an early stage and a later stage of propagation:

  ![_config.yml]({{ site.baseurl }}/images/green-function-initial.PNG)

  ![_config.yml]({{ site.baseurl }}/images/green-function-later.PNG)

We see that perhaps the problem stems from two issues: the fact that the absorbing boundary creates a reflection when the Green's function is still strong and that we cut off the Green's function right during the time this reflection is interacting with the source at the platform. By extending the time duration we can allow the low frequency dip in amplitude from the absorbing boundary to return back to zero:

  ![_config.yml]({{ site.baseurl }}/images/2D-green-function-dispersive.png)

This very dispersive Green's function is not a concern once it is convolved by the power spectral density of the noise source, which is only around 1-2 Hz.

Returning back to the correlations, we still see some minor reverberations - and we might want to try to take them out by extending the time vector further, extending the absorbing boundaries, applying a taper, or exploiting the FFT and selectiively removing frequencies.

The easiest solution though may be to extend the domain such that the Green's function does not interact with the absorbing boundary until a bit later. However, the model velocity is about 360 m/s and with a frequency spectrum from 0.55-0.95 Hz, we are dealing with maximum contriubtion of about 650 m. The boundaries are about a km away, which is more than the rule-of-thumb of about 1.5x the wavelength from the boundary.

So, a taper is probably more than good enough.