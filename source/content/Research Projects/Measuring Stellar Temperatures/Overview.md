<font color="#7f7f7f">This is an overview of my personal research project on amateur stellar "spectroscopy" which allows one to find the effective temperature of a star simply by taking and analyzing photos of them. Below, you will see a quick intro to the project, background information on the math/physical principles, results, shortcomings, and next steps. </font>


# Introduction

## The Physics and Math

Objects emit electromagnetic radiation at various intensities across all wavelengths based on their temperature (See [blackbodies](https://www.britannica.com/science/blackbody)). The relationship between temperature and intensity at a given wavelength is modeled by the Planck Function:
$$
B_{\lambda}(\lambda,T)=\left(\frac{2hc^2}{\lambda^5}\cdot\frac{1}{\exp(\frac{hc}{\lambda k_{B}T})-1}\right) \qquad \qquad \qquad \qquad (1)

$$
Where *h* is Planck's constant, c is the speed of light, $k_B$ is the Boltzmann constant, T is the effective temperature of the body (star), and $\lambda$ is the wavelength of light. Plotting the intensity as a function of wavelength yields a blackbody curve as seen below:

![[STScI-J-article-Spectroscopy-Part3-ContinuousSpectra2.jpg]]
[webbtelescope.org](https://webbtelescope.org/contents/articles/spectroscopy-101--types-of-spectra-and-spectroscopy)

The particular shape and values of a blackbody curve depends entirely on the temperature of the (in this case) star, and so one can use this relationship to determine the temperature of a star by matching a blackbody curve to its function and basically reading off the temperature based on what most closely matched the observed data. As far as I am aware, professional astronomers do this by using diffraction gratings to spread the incoming light from a star into a spectrum, and measuring the intensity of the light with a spectrophotometer or similar device to measure the star's spectrum. The systems and equipment for accurate measurements are complicated, finely tuned, and obviously quite expensive.

Theoretically, you do not need to measure the entire continuous spectrum of a star to be able to find its blackbody curve. If you can measure the brightness of a star at a few known wavelengths, you could fit a curve through/around the measured points just the same. One could probably achieve this with a monochrome astronomy camera and a set of [narrowband filters](https://starizona.com/blogs/tutorials/narrowband-imaging). 

I have neither a dedicated astro camera, filters, or a spectrophotometer rig, and seeing as I am a university student (a demographic not well-known for their abundance of spare cash), I wasn't exactly prepared to buy any of those items for this project. Instead, the purpose of this project was, as the name implies, to attempt to measure the temperature of stars using basic and cheap equipment accessible to the amateur astronomer (and largely what I already had). 



