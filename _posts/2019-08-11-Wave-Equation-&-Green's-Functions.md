---
layout: post
title: Wave Equation and Green's Functions
---

For me at least, the introduction to the term 'Green's Function' was extremely confusing. The professors had written the wave equation on the board and talked about different solutions that satisfied it. Then they would spend an excruciating amount of time talking about using a delta source as input to get out the impulse response to the system (the Green's Function)... next thing they would write the wave equation as the Helmholtz equation and then talk about monochromatic non-sense and soon we would be further lost when they wrote out the second-order wave equation in terms of first-order series of equations that would instead return the time derivative of the Green's function. No distinctions were made between a spatial delta and a temporal delta... And soon we were off on talking about monopoles and dipoles, convolutions, reflectivity series, representation theorems, reciprocity, and everything just became garbage. 

This all too much. Let us try to tackle the problem in small amounts. The post is titled Wave Equation and Green's Functions, but let us see how far we get. For now at least, I will try to keep it high-level and discuss numerical solving the wave equation and caveats on Green's function modelling.


  ![_config.yml]({{ site.baseurl }}/images/wapenaar-green-function.PNG)




### Steps to getting started:
