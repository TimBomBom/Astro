<font color="#7f7f7f">This document details my personal research project on amateur stellar "spectroscopy" which allows one to find the effective temperature of a star simply by taking and analyzing photos of them. This post is written in the same general structure as a scientific paper (sans abstract) but a lot more casual. Below, you will see a quick intro to the project, basic background information on the mathematical/physical principles, results, shortcomings, and next steps. Feel free to skip to Methods if you don't want to read through the intro. </font>

# Introduction
%%Maybe yap about importance of measuring star temp and such%%
## The Physics and Math

Here is an intro to the concepts on which I base much of the calculations and theory. Objects emit electromagnetic radiation at various intensities across all wavelengths based on their temperature (See [blackbodies](https://www.britannica.com/science/blackbody)). The relationship between temperature and the intensity ("brightness") at a given wavelength is modeled by the Planck Function:
$$
B_{\lambda}(\lambda,T)=\left(\frac{2hc^2}{\lambda^5}\cdot\frac{1}{\exp(\frac{hc}{\lambda k_{B}T})-1}\right) \qquad \qquad \qquad \qquad (1)

$$
Where *h* is Planck's constant, c is the speed of light, $k_B$ is the Boltzmann constant, T is the effective temperature of the body (star), and $\lambda$ is the wavelength of light. Plotting the intensity as a function of wavelength yields a blackbody curve as seen below:

![[STScI-J-article-Spectroscopy-Part3-ContinuousSpectra2.jpg]]
<font color="#7f7f7f">Figure 1 - A diagram showing how the intensity and peak wavelength of a blackbody curve changes with temperature. Taken from </font>[webbtelescope.org](https://webbtelescope.org/contents/articles/spectroscopy-101--types-of-spectra-and-spectroscopy)

<br>

The particular shape and values of a blackbody curve depends entirely on the temperature of the object (in this case a star), and so one can use this relationship to determine the temperature of said star by matching a blackbody curve to its function and basically reading off the temperature based on what most closely matched the observed data. From what I know, professional astronomers do this by using diffraction gratings or prisms to spread the incoming light from a star, and sample the intensity of the light along the wavelengths with a spectrophotometer or similar device to get the star's spectrum. The systems and equipment for accurate measurements are complicated, finely tuned, and obviously quite expensive. 

A version of this is possible for the amateur astronomer to perform using a [star analyzer grating](https://www.rspec-astro.com/download/StarAnalyser100manual.pdf) to obtain the spectrum of a star which can then be analyzed with various free softwares. The Astronomical Amateur has a good [video](https://www.youtube.com/watch?v=GmpftaDJTWs) on the workflow for this so-called "[slitless spectroscopy](https://en.wikipedia.org/wiki/Slitless_spectroscopy_)" on his YouTube channel. The benefits to this method are you can get both the temperature and elemental composition of stars based on the absorption features. The downside, however, is that these grating filters cost upwards of $280 CAD, and the workflow is long and relatively tedious. Pairing this with a mono camera like in the video will skyrocket the cost even further.

%%Theoretically, you do not need to measure the entire continuous spectrum of a star to be able to find its blackbody curve. If you can measure the brightness of a star at a few known wavelengths, you can fit a curve through/around the measured points. One could probably achieve this with a monochrome astronomy camera and a set of [narrowband filters](https://starizona.com/blogs/tutorials/narrowband-imaging). This is already much more accessible method for the amateur, but not *this* amateur.%%

I have neither a dedicated astro camera, filters, or a professional observatory, and seeing as I am a university student (a demographic not well-known for their abundance of spare cash), I wasn't exactly prepared to spend thousands of dollars on any of those items for this project. Instead, the purpose of this project was to see if would be possible to measure the temperature of stars but under the restriction of using basic and cheap equipment, largely what I already owned. 


<br>

## Inspiration

So where did the idea to do this project come from? Well I learned about the calculations and theory behind the measurement of star temperatures explained in [[Overview#The Physics and Math|The Physics and Math]] from my first-year astronomy classes and thought it would be a cool thing to do one day, though at the time I did not know exactly how I would go about doing it. 

One night, whilst taking pictures of Polaris for an astronomy assignment, I looked closer and noticed that the [diffraction spikes](https://en.wikipedia.org/wiki/Diffraction_spike) created by my Newtonian's vanes had a very pronounced rainbow effect; the spikes were colourful!

![[polaris with rainbow spikes.png]]
<font color="#7f7f7f">Figure 2 - A stacked and auto-stretched photo of Polaris. The rainbow in the spikes is clearly visible.</font>

![[joshsastro lupi nova.png]]

<font color="#7f7f7f">Figure 3 - A processed photo of the V462 Lupi Nova by</font> [joshsastro](https://app.lightbucket.co/astronomers/joshsastro)<font color="#7f7f7f">. The rainbow effect is demonstrated dramatically, though the saturation was likely enhanced in the editing process. </font>


This phenomenon has been noted on some forums, but not much attention is given to it since it isn't considered an invasive artefact/effect and isn't really bothering anybody. Anyway, the reason why this happens is because incoming light diffracts around the vanes holding the secondary mirror, causing the path to bend slightly and cause interference patterns. The angle by which the light bends depends on the wavelength, and so the different colours of light from the star disperse into a rainbow. This effect is used as a sort of crude diffraction grating to separate the light into their various colours (most importantly, without the price tag of said gratings). This means that the colours in the spike contain information about the colour of the star, and it was established in [[Overview#The Physics and Math|The Physics and Math]] that the colour of a star can tell you its temperature.

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
<center><font color="#7f7f7f">Figure 4 - A photo of my humble setup.</font></center>

<br>

## Data Acquisition

For all stars analyzed, 10-15 subs were taken using the intervalometer set to 10" @ ISO 800. Photos were taken in broadband (that is, no filters like UHC, skyglow, etc. were used). The sequences were stacked in DeepSkyStacker and calibrated with darks, biases, and flats. The purpose of stacking multiple photos was to reduce noise in the relatively faint spikes that would skew the results. 

## Pre-Processing

Not every camera is created equal, and as such, each sensor and its filters are sensitive to light differently. Before analyzing the diffraction spikes, it was critical to ensure that the colours were accurate. The stacked images were brought into [SIRIL](https://siril.org/), plate solved, and then calibrated via [Spectrophotometric Colour Calibration](https://siril.readthedocs.io/en/latest/processing/color-calibration/spcc.html) ("SPCC") to account for the response of my camera. The white point reference was set to "Average Spiral Galaxy" and the camera information was set to the closest equivalent Canon model(s) which in this case was the EOS 60D sensor, and EOS 350D blocking filter. No atmospheric corrections were made.

![[SPCC window.png]]

<font color="#7f7f7f">Figure 5 - The SPCC window in SIRIL showing the parameters used. My camera, the EOS 100D was not available in the list of OSC sensors, but the 60D was close enough so it was used.</font>

<br>

![[SPCC comparison.png]]
<font color="#7f7f7f">Figure 6 - A comparison of Betelgeuse before and after colour calibration. A strong red colour cast was present before calibration, but was successfully removed after applying SPCC. A gradient remained, but it did not impact the results significantly.</font>

## Analysis

### Getting the Numbers

After calibration, it came time to measure the intensity of the colours along a spike. My telescope was not manufactured perfectly, and the thickness and structure of each vane varied slightly from each other. As such, each diffraction spike was a bit different in thickness and sharpness which affected the outcome of the measurements. From my testing, the spike running roughly NNE-SSW along the celestial grid provided by SIRIL yielded the best results, so it was used for all analyses. The analysis itself was done by SIRIL's [Intensity Profile](https://siril.readthedocs.io/en/latest/Intensity-Profiling.html) tool set to Color.


![[Star with Celestial grid.png]]
<font color="#7f7f7f">Figure 7 - A photo of Betelgeuse after SPCC ready to be analyzed. The celestial grid and compass are displayed, and a line is drawn over the "accurate spike" with the intensity plot tool. </font>

<br>

![[Betelgeuse intensity profile.png]]

<font color="#7f7f7f">Figure 8 - The plot generated by SIRIL superimposing the graphs of brightness values of the red, green, and blue pixels along the cut (spike) for Betelgeuse. The large feature in the middle represents the round center of the star itself which is fully saturated, and on either side, repeating peaks and valleys represent the repeating bands of colour in the spikes.</font> 

The intensity plot was saved as a .dat file for the next step. At this point, our work in SIRIL is finished, and it is now over to the [[Java Program]] to interpret the data.  


### Interpreting the Data

From the intensity plot/dat file, there are three values for each colour channel we are interested in:

#### 1 - The absolute maximum of the channel (the "Plateau")
This is the value of the flat part of the large middle feature.

#### 2 - The local maximum of the first band (the "Peak") 
This is the value of the spike after the Plateau, representing the first and brightest repeating band of colour in the diffraction spike.

#### 3 - The local minimum between the Plateau and the Peak (the "Trough") 
Fairly self-explanatory; the dip before the first Peak after the large middle feature


![[annotated plot.png|500]]

<font color="#7f7f7f">Figure 9 - A plot of Aldebaran's red intensity profile with the three points of interest labelled.</font>

<br>

To automatically detect these points, the program implements a crude first derivative by applying the secant between each point and the point in front:

$$
\Delta y=\frac{y_{n+1}-y_{n}}{x_{n+1}-x_{n}} \qquad \qquad \qquad \qquad (2)
$$

It detects the local max/min by checking when the slope changes signs and reports the value at that position. However, the plot is noisy, and may dip up or down, tricking the simple algorithm with a false local min/max. To combat this, [convolution](https://en.wikipedia.org/wiki/Kernel_(image_processing)#Convolution) was applied to smooth out the graph, and all calculations were based off the smoothed version. The convolution algorithm does change the values a bit from the original, but the difference between the raw and smoothed versions have minimal impact on the final temperature value.

![[raw plot.png|500]] ![[smoothed plot.png|500]]

<font color="#7f7f7f">Figure 10 - A) The raw intensity profile of Aldebaran straight from SIRIL with no smoothing applied. Multiple dips and turnarounds in the slope are present which would trick the program into reporting a wrong local max. This is particularly noticeable in the green channel. B) The same intensity profile after being run through convolution to smooth out the graph and eliminate the noise.</font>

<br>

Once these points have been obtained, the following operation is performed to yield a value for each channel:

$$
\text{Max}_{{<channel>}} = \frac{Peak-Trough}{Plateau}+Trough \qquad \qquad \qquad \qquad (3)
$$

This was done to compensate for the sensitivity of the sensor. As you could see in the intensity plot of Fig. 8, the pixels are saturated (as evidenced by the flat cutoff of the plateau), yet they do not all max out at 1. Instead, the R, G, and B channels each max out at unique values which remain approximately consistent between stars. As such, the sensitivity of each channel had to be adjusted for. After this, we are left with the intensity of the red, green, and blue light from the star. Then, the ratios between the channels are taken. For example:

$$
\text{R}_{RG} = \frac{Max_{R}}{Max_{G}} \qquad \qquad \qquad \qquad (4)
$$
This yields the ratio between the intensity of red to green light coming from the star. This is repeated for Green/Blue and Red/Blue.

<br>

### Estimating Temperature

Three values for the wavelengths of red, green, and blue were selected to be 700, 550, and 478 nm respectively (roughly in the middle of the [ranges for each colour](https://en.wikipedia.org/wiki/Visible_spectrum#Spectral_colors)). Then, for some temperature, the Planck function was evaluated for the theoretical wavelengths and then ratios were once again made between these theoretical intensities as in eq. 4. For example:

$$
Th_{RG}=\frac{B_{\lambda}(700,T)}{B_{\lambda}(550,T)} \qquad \qquad \qquad \qquad (5)
$$

Where $B_{\lambda}(\lambda,T)$ is the Planck function from eq. 1, and "Th" being the <u>Th</u>eoretical/predicted ratio between the channels. This is once again performed for Red/Green, Green/Blue, and Red/Blue. 

The program then evaluates the [RMSE](https://en.wikipedia.org/wiki/Root_mean_square_deviation) between the observed ratios and theoretical at the guessed temperature by the following calculation:

$$
\sqrt{ \frac{(R_{RG}-Th_{RG})^2+(R_{GB}-Th_{GB})^2+(R_{RB}-Th_{RB})^2}{3} } \qquad \qquad(6)
$$




The initial guess starts at around *T = 100 K* and evaluates the RMSE via eq. 6,  then checks if the error for *T = 101 K* is smaller. If so, 101 K becomes the new guess, and the process repeat for *T = 102 K* and so on. This goes until the error is minimized, at which point the process stops and the final temperature estimate is returned.

<br>

The ratios between channels was taken because I had no way of calibrating my setup to the units of intensity that the Planck function works in. Evaluating equation 1 for some temperature and wavelength would yield a large number like 2.9x10<sup>12</sup> W m<sup>-3</sup>. However, I am not measuring in such units, I am simply reading how bright each pixel is, often yielding numbers ranging from 0-1. Trying to fit a Planck curve to such totally incompatible measurements is not going to get us anywhere. Instead, the intensities of the channels were divided by each other so that they could be compared relative to each other (i.e. the red signal is 2.11x brighter than the blue, etc). This eliminates units and allows us to compare the measured and theoretical values.


![[Desmos(1) 1.gif]]
<font color="#7f7f7f">Figure 11 - A gif of a Planck curve (eq. 1) showcasing how the intensity of each colour changes with temperature. The scale is on the order of 10<sup>12</sup>, however the ratio between colour intensities (in this case it would be Th<sub>channels</sub> from eq. 5) can be matched as closely as possible with the ratios from eq. 4, and the temperature read off. </font>



---

# Results

From the stars tested so far, this method vastly exceeded expectations. The accuracy for mid-temperature (F,G,K class) stars was remarkable,  yielding errors <1% (Table 1). This was a pleasant surprise, and confirmation that it was indeed possible to perform a basic form of stellar spectroscopy with cheap amateur gear.

<br>

<font color="#7f7f7f">Table 1.  A list of tested stars with the measured temperature compared to their literature values along with a %Error calculation.</font>

|  **Star**  | **Measured (K)** | **Actual (K)** | **Error (%)** | Within ± Range? |
| :--------: | :--------------: | :------------: | :-----------: | :-------------: |
| Aldebaran  |       3904       |   3900 ± 50    |     0.10      |       Yes       |
|  Polaris   |       6052       |   6015 ± 50    |     0.62      |       Yes       |
| Betelgeuse |       3616       |   3600 ± 25    |     0.44      |       Yes       |

---

# Shortcomings & Next Steps

## Data

The main priority moving forward is to collect more data on more stars. While it is cool that the model worked for the three stars tested, it doesn't speak much to its repeatability or reliability as a measurement method. More stars will need to be surveyed to prove it wasn't a fluke and to understand the range of temperature values for which this method is accurate. I had attempted to measure very hot stars such as Alnitak, Pollux, Rigel, and Bellatrix, however it did not yield good results. It seems that there is an upper bound on effective temperature, past which the method is no longer accurate. It is unclear, however, what that bound is. Therefore, expanding the dataset is crucial to both proving consistency and understanding the limits of the method. 

Below are a list of stars I would like to survey next and add to the table in [[#Results]]:

|       **Star**       | **Temperature (K)** |
| :------------------: | :-----------------: |
|      γ Draconis      |        3964         |
|       Arcturus       |        4286         |
|       Antares        |        3660         |
| ε Coronae Borealis\* |        4408         |
|       δ Boötis       |        4810         |
|         Sadr         |        5790         |
|        Rigel         |        12100        |
\*Note that ε Coronae Borealis is fairly faint (V=4.28), and so it may not yield diffraction spikes that are prominent/bright enough for good analysis.

I am also hoping to try Rigel or at least any of the very hot stars again since, upon further examination, the sub-frames for all the hot stars seem to be rather blurry due to poor tracking. I cannot rule out the possibility that many of their results were marred because of bad data. The tool may still not necessarily be very accurate at high temperatures, but the error may be lessened a bit.


## Analysis

From my observations, small variations in how I draw the line for the intensity profile along the spike can have a pretty substantial impact on the results. To work around this, I might try splitting the image into separate B&W images for each colour channel, and performing a Tri-Profile (mono) profile. This profile draws parallel lines offset from the one drawn by a number of pixels. This could be used to generate a few profiles along the same spike which have small variations between them. The coordinates would be copied and performed again for all colour channels, and then the results averaged with a ± SD or something.

This is just an idea of what I will do moving forward, and will update the page with how it went when I get around to doing it.