<font color="#7f7f7f">This is an overview of my personal research project on amateur stellar "spectroscopy" which allows one to find the effective temperature of a star simply by taking and analyzing photos of them. Below, you will see a quick intro to the project, basic background information on the mathematical/physical principles, results, shortcomings, and next steps. </font>

> [!tip]
> You can collapse sections by hovering over headers and clicking on the little arrow icon to show/hide the contents under that header. You can use this if you get bored of reading the background preamble to skip to the methods/results, or alternatively use the table of contents on the right side of the screen. 

# Introduction

## The Physics and Math

Objects emit electromagnetic radiation at various intensities across all wavelengths based on their temperature (See [blackbodies](https://www.britannica.com/science/blackbody)). The relationship between temperature and intensity at a given wavelength is modeled by the Planck Function:
$$
B_{\lambda}(\lambda,T)=\left(\frac{2hc^2}{\lambda^5}\cdot\frac{1}{\exp(\frac{hc}{\lambda k_{B}T})-1}\right) \qquad \qquad \qquad \qquad (1)

$$
Where *h* is Planck's constant, c is the speed of light, $k_B$ is the Boltzmann constant, T is the effective temperature of the body (star), and $\lambda$ is the wavelength of light. Plotting the intensity as a function of wavelength yields a blackbody curve as seen below:

![[STScI-J-article-Spectroscopy-Part3-ContinuousSpectra2.jpg]]
[webbtelescope.org](https://webbtelescope.org/contents/articles/spectroscopy-101--types-of-spectra-and-spectroscopy)

<br>

The particular shape and values of a blackbody curve depends entirely on the temperature of the (in this case) star, and so one can use this relationship to determine the temperature of a star by matching a blackbody curve to its function and basically reading off the temperature based on what most closely matched the observed data. As far as I am aware, professional astronomers do this by using diffraction gratings to spread the incoming light from a star into a spectrum, and measuring the intensity of the light with a spectrophotometer or similar device to measure the star's spectrum. The systems and equipment for accurate measurements are complicated, finely tuned, and obviously quite expensive.

Theoretically, you do not need to measure the entire continuous spectrum of a star to be able to find its blackbody curve. If you can measure the brightness of a star at a few known wavelengths, you could fit a curve through/around the measured points just the same. One could probably achieve this with a monochrome astronomy camera and a set of [narrowband filters](https://starizona.com/blogs/tutorials/narrowband-imaging). This is already much more accessible method for the amateur, but not *this* amateur.

I have neither a dedicated astro camera, filters, or a spectrophotometer rig, and seeing as I am a university student (a demographic not well-known for their abundance of spare cash), I wasn't exactly prepared to spend thousands of dollars on any of those items for this project. Instead, the purpose of this project was, as the name implies, to attempt to measure the temperature of stars using basic and cheap equipment accessible to the amateur astronomer (and largely what I already had). 


## Equipment

Beyond what I had already, I did not plan on buying any new equipment or filters or anything of the sort for this research project. Speaking of what I already had, I was using a Skywatcher Virtuoso GTi 150p Newtonian reflector (not an *amazing* telescope, but **very** affordable), and my camera was a Canon Rebel SL1, a 12 year old DSLR (at the time of writing this). The telescope has a GoTo mount which allows for tracked exposures.

<br>
<img src="telescope setup.jpg" 
        width="300" 
        style="display: block; margin: 0 auto" />
<center><font color="#7f7f7f">A photo of my humble setup.</font></center>

<br>
<br>

## Inspiration

So where did the idea to do this project come from? Well I learned about the calculations and theory behind the measurement of star temperatures explained in [[Overview#The Physics and Math|The Physics and Math]] from my astronomy classes and thought it would be a cool thing to do one day, though at the time I did not know exactly how I would go about doing it. 

One night, whilst taking pictures of Polaris for an astronomy assignment, I looked closer and noticed that the [diffraction spikes](https://en.wikipedia.org/wiki/Diffraction_spike) from my Newtonian's vanes had a very pronounced rainbow effect; the spikes were colourful!

![[Pasted image 20250627012120.png]]
<font color="#7f7f7f">A stacked and auto-stretched photo of Polaris. The rainbow in the spikes is clearly visible.</font>

![[Pasted image 20250627013009.png]]
<font color="#7f7f7f">A processed photo of the V462 Lupi Nova by</font> [joshsastro](https://app.lightbucket.co/astronomers/joshsastro)<font color="#7f7f7f">. The rainbow effect is demonstrated dramatically, though the saturation was likely enhanced in the editing process. </font>


I did some digging, and it is a known phenomenon on some forums, just not super discussed since it isn't considered an invasive artefact/effect and isn't really bothering anybody. Anyway, the reason why this happens is because incoming light diffracts around the vanes holding the secondary mirror, causing the path to bend slightly and cause interference patterns. The angle of diffraction depends on the wavelength of the light (I think this holds in this case), and so the light from the star is dispersed into a rainbow, similar but not exactly like a prism would. The important thing to note is the light from the star itself is being split up and separated. This means that the colours in the spike contain information about the colour of the star and it was established in [[Overview#The Physics and Math|The Physics and Math]] that the colour of a star can tell you its temperature.

When I saw the rainbow diffraction spikes, I thought it just might be possible to analyze the colours in them to find the effective temperature of the stars.