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

These effects or probably most like most noticeable when modelling in 2D, simply because the Green's function in 2D is not finite and has a tail that extends to infinite time. We need to extend the time propogation a bit longer to capture a significant part of this tail.

We still see some minor reverberations - and we might want to try to take them out by extending the time vector further, extending the absorbing boundaries, applying a taper, or exploiting the FFT and selectiively removing frequencies.