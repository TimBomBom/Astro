<font color="#7f7f7f">This document details my personal research project on amateur stellar "spectroscopy" which allows one to find the effective temperature of a star simply by taking and analyzing photos of them. This post is written in the same general structure as a scientific paper (sans abstract) but a lot more casual. Below, you will see a quick intro to the project, basic background information on the mathematical/physical principles, results, shortcomings, and next steps. Feel free to skip to Methods if you don't want to read through the intro. </font>

# Introduction
%%Maybe yap about importance of measuring star temp and such%%
## The Physics and Math

Here is an intro to the concepts on which I base much of the calculations and theory. Objects emit electromagnetic radiation at various intensities across all wavelengths based on their temperature (See [blackbodies](https://www.britannica.com/science/blackbody)). The relationship between temperature and the intensity ("brightness") at a given wavelength is modeled by the Planck Function:
$$
B_{\lambda}(\lambda,T)=\left(\frac{2hc^2}{\lambda^5}\cdot\frac{1}{\exp(\frac{hc}{\lambda k_{B}T})-1}\right)

$$
Where *h* is Planck's constant, c is the speed of light, $k_B$ is the Boltzmann constant, T is the effective temperature of the body (star), and $\lambda$ is the wavelength of light. Plotting the intensity as a function of wavelength yields a blackbody curve as seen below:

![[STScI-J-article-Spectroscopy-Part3-ContinuousSpectra2.jpg]]
[webbtelescope.org](https://webbtelescope.org/contents/articles/spectroscopy-101--types-of-spectra-and-spectroscopy)

<br>

The particular shape and values of a blackbody curve depends entirely on the temperature of the object (in this case a star), and so one can use this relationship to determine the temperature of said star by matching a blackbody curve to its function and basically reading off the temperature based on what most closely matched the observed data. From what I know, professional astronomers do this by using diffraction gratings or prisms to spread the incoming light from a star into a spectrum, and measure the intensity of the light with a spectrophotometer or similar device to get the star's spectrum. The systems and equipment for accurate measurements are complicated, finely tuned, and obviously quite expensive. 

This is possible for the amateur astronomer to perform using a [star analyzer grating](https://www.rspec-astro.com/download/StarAnalyser100manual.pdf) to obtain the spectrum of a star which can then be analyzed with various free softwares. The Astronomical Amateur has a good [video](https://www.youtube.com/watch?v=GmpftaDJTWs) on the workflow for this so-called "[slitless spectroscopy](https://en.wikipedia.org/wiki/Slitless_spectroscopy_)" on his YouTube channel. The benefits to this method are you can get both the temperature and elemental composition of stars based on the absorption features. The downside, however, is that these grating filters cost upwards of $280 CAD, and the workflow is long and relatively tedious. Pairing this with a mono camera like in the video will skyrocket the cost even further.

%%Theoretically, you do not need to measure the entire continuous spectrum of a star to be able to find its blackbody curve. If you can measure the brightness of a star at a few known wavelengths, you can fit a curve through/around the measured points. One could probably achieve this with a monochrome astronomy camera and a set of [narrowband filters](https://starizona.com/blogs/tutorials/narrowband-imaging). This is already much more accessible method for the amateur, but not *this* amateur.%%

I have neither a dedicated astro camera, filters, or a professional observatory, and seeing as I am a university student (a demographic not well-known for their abundance of spare cash), I wasn't exactly prepared to spend thousands of dollars on any of those items for this project. Instead, the purpose of this project was to see if would be possible to measure the temperature of stars but under the restriction of using basic and cheap equipment, largely what I already owned. 


<br>

## Inspiration

So where did the idea to do this project come from? Well I learned about the calculations and theory behind the measurement of star temperatures explained in [[Overview#The Physics and Math|The Physics and Math]] from my astronomy classes and thought it would be a cool thing to do one day, though at the time I did not know exactly how I would go about doing it. 

One night, whilst taking pictures of Polaris for an astronomy assignment, I looked closer and noticed that the [diffraction spikes](https://en.wikipedia.org/wiki/Diffraction_spike) created by my Newtonian's vanes had a very pronounced rainbow effect; the spikes were colourful!

![[polaris with rainbow spikes.png]]
<font color="#7f7f7f">A stacked and auto-stretched photo of Polaris. The rainbow in the spikes is clearly visible.</font>

![[joshsastro lupi nova.png]]

<font color="#7f7f7f">A processed photo of the V462 Lupi Nova by</font> [joshsastro](https://app.lightbucket.co/astronomers/joshsastro)<font color="#7f7f7f">. The rainbow effect is demonstrated dramatically, though the saturation was likely enhanced in the editing process. </font>


This phenomenon has been noted on some forums, but not much attention is given to it since it isn't considered an invasive artefact/effect and isn't really bothering anybody. Anyway, the reason why this happens is because incoming light diffracts around the vanes holding the secondary mirror, causing the path to bend slightly and cause interference patterns. The angle of diffraction depends on the wavelength of the light (I think this holds in this case), and so the light from the star is dispersed into a rainbow. The important thing to note is the light from the star itself is being split up and separated. This means that the colours in the spike contain information about the colour of the star, and it was established in [[Overview#The Physics and Math|The Physics and Math]] that the colour of a star can tell you its temperature.

When I saw the rainbow diffraction spikes, I thought it just might be possible to analyze the colours in them to find the effective temperature of the stars.

>[!important]
>The particular workflow, calculations, and procedure of this project are based on the analysis of diffraction spikes. Non-Newtonian telescopes like refractors or SCTs will likely not work for this as they do not produce spikes. Furthermore, the code and exact steps are heavily tailored to my setup which has its own quirks that not every Newtonian telescope user will have. If you want to try this method, you will probably have to spend a while tweaking the settings for your equipment. This is more of a presentation of what worked for me and not a tool ready to work out of the box. 

<br>
<br>

---

# Materials and Method
With the background info out of the way, it's on to the actual scientific part of this post:

## Equipment

Beyond what gear I already owned, I did not plan on buying any new equipment or filters or anything of the sort for this research project. Speaking of what I already had, I was using a Skywatcher Virtuoso GTi 150p, and my camera was an unmodified Canon Rebel SL1/EOS 100D. The telescope has a GoTo mount which allows for tracked exposures.

<br>
<img src="telescope setup.jpg" 
        width="300" 
        style="display: block; margin: 0 auto" />
<center><font color="#7f7f7f">A photo of my humble setup.</font></center>

<br>

## Data Acquisition

For all stars analyzed, 10-15 subs were taken using the intervalometer set to 10" @ ISO 800. Photos were taken in broadband (that is, no filters like UHC, skyglow, etc. were used). The sequences were stacked in DeepSkyStacker and calibrated with darks, biases, and flats. The purpose of stacking multiple photos was to reduce noise in the relatively faint spikes that would skew the results. 

## Pre-Processing

Not every camera is created equal, and as such, each sensor and its filters are sensitive to light differently. Before analyzing the diffraction spikes, it was critical to ensure that the colours were accurate. The stacked images were brought into [SIRIL](https://siril.org/), plate solved, and then calibrated via [Spectrophotometric Colour Calibration](https://siril.readthedocs.io/en/latest/processing/color-calibration/spcc.html) ("SPCC") to account for the response of my camera. The white point reference was set to "Average Spiral Galaxy" and the camera information was set to the closest equivalent Canon model(s) which in this case was the EOS 60D sensor, and EOS 350D blocking filter.

![[SPCC window.png]]

<font color="#7f7f7f">The SPCC window in SIRIL showing the parameters used. My camera, the EOS 100D was not available in the list of OSC sensors, but the 60D was close enough so it was used.</font>

<br>

![[SPCC comparison.png]]
<font color="#7f7f7f">A comparison of Betelgeuse before and after colour calibration. A strong red colour cast was present before calibration, but was successfully removed after applying SPCC. A gradient remained, but it did not impact the results significantly.</font>

## Analysis Pt. 1 - Getting the Data

After calibration, it came time to measure the intensity of the colours along a spike. My telescope was not manufactured perfectly, and the thickness and structure of each vane varied slightly from each other. As such, each diffraction spike was a bit different in thickness and sharpness which affected the quality of the result. From my testing, the spike running roughly NNE-SSW along the celestial grid provided by SIRIL yielded the best results, so it was used for all analyses. The analysis itself was done by SIRIL's [Intensity Profile](https://siril.readthedocs.io/en/latest/Intensity-Profiling.html) tool set to Color.


![[Star with Celestial grid.png]]
<font color="#7f7f7f">A photo of Betelgeuse after SPCC ready to be analyzed. The celestial grid and compass are displayed, and a line is drawn over the "accurate spike" with the intensity plot tool. </font>

<br>

![[Betelgeuse intensity profile.png]]

<font color="#7f7f7f">The plot generated by SIRIL superimposing the graphs of brightness values of the red, green, and blue pixels respectively along the cut (spike) for Betelgeuse. The large feature in the middle represents the round center of the star itself which is fully saturated, and on either side, repeating peaks and valleys represent the repeating bands of colour in the spikes.</font>

The intensity plot is saved as a .dat file for the next step. At this point, our work in SIRIL is finished, and it is now over to the Java program to interpret the data.  
%%Put a link to the Java program page when you upload it%%
## Analysis Pt. 2 - Interpreting the Data

From the intensity plot/dat file, there are three values for each colour channel we are interested in:

1. The absolute maximum of the channel (dubbed the "Plateau")
	 <font color="#7f7f7f">This is the value of the flat part of the large middle feature. </font>
2. The local maximum of the first band (dubbed the "Peak")
	<font color="#7f7f7f">This is the value of the spike after the Plateau, representing the first, brightest repeating band of colour in the diffraction spike  </font>
3. The local minimum between the Plateau and the Peak (dubbed the "Trough")
	 Fairly self-explanatory; the dip before the first Peak

![[annotated plot.png|500]]
<font color="#7f7f7f">A plot of Aldebaran's red intensity profile with the three points of interest labelled.</font>

<br>

To automatically detect these points, the program implements a crude first derivative by applying the secant between each point and the point in front:

$$
\Delta y=\frac{y_{n+1}-y_{n}}{x_{n+1}-x_{n}}
$$

It detects the local max/min by checking when the slope changes signs and reports the value at that position. However, the data are noisy, and may dip up or down, tricking the simple algorithm with a false local min/max. To combat this, convolution is applied to smooth out the graph, and all calculations are based off the smoothed version.

![[raw plot.png|500]] 
![[smoothed plot.png|500]]
