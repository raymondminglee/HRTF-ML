# Personalizing HRTF Using Machine Learning

## Introduction
Virtual reality (VR) requires rendering accurate head-related transfer functions (HRTF) to ensure a realistic and immersive virtual auditory space. An HRTF characterizes how each ear receives sound from a certain location in space based on the shape of the head, torso, and pinnae, and provides a unique head-related impulse response (HRIR) for each given source location.  
Since HRTFs are person-specific and difficult to measure, machine learning have become a popular way to estimate personalized hrtf.  
In this page, I am going to explain the methodology I use for apply machine learning algorithm as a way to generate HRTFS, and aslo the use of ABX and Localization testing to validate its feasability.

## How Do We Hear Sound
Spatial Audio is creating 3D audio experience by using headphones, and one important aspect of it is being able to hear where the sound is coming from.
<img src="pic/3daudio.png?raw=true"/>
And it turns out human utilize two primary factors during sound localization, __Interaural Differences__ and __Spectral Cues__.

### Interaural Differences
Interaural differenses are just time and intensity differences between our right and left ear when we hear a sound. The time difference is call __Interaural Time Difference, ITD__, and the internsity difference is called Interaural __Intensity Difference, IID__. And naturally, they are heavily realted to one's head size.  
<img src="pic/idiff.png?raw=true"/>

### Spectral Cues
Besides ITD and IID, Spectral Cue also paly a big role in sound localization. As sound coming form different places and travelling to our ear cannal, our unique pinna shape will produce differenc reflections of the sound. The add up of all these reflections creates different spectral cues between our ears.
<img src="pic/spectral.png?raw=true"/>

### Head Realated Transfer Function
A pair of Head Related Impulse Response (HROR) incorporates all these aspects of sound localization. Its a pair of inpulse response of a paticular sound in a particular place. A Head Related Transfer Function (HRTF) is HRIR presented in the frequency domain. 
<img src="pic/hrir.png?raw=true"/>

## Acquiring HRTF
Due to the reason mention above, HRTFs are related to one's body shape, so each of us has unique set of HRTFs. We could measure HRTF by placing microphones in one's ear and measuring all its impulse response to all locations. Naturally this is expensive and time consuming.
<img src="pic/measure.png?raw=true"/>

So it comes to our mind that we could use Machine Learing to estimate personalized HRTF, 
<img src="pic/ml.png?raw=true"/>

## Dataset
We have the CIPIC HRTF Database as our training data, it contains
* 35 Subjects 
* 1250 measurements (25 azimuth, 50 elevation) 
* Anthropometric measures 
	* 17 Head and Torso Dimensions
	* Left Ear Dim
	* Right Ear Dim

## Previous Research
Previous research by Chun atc. (2017) has shown some promising result. While trying to replicate their findings using a Fully Connected Neural Net, althgou we could get __lower MSE (-26.8 dB)__, the estimated HRIR did not resemble the the actual one on its shape, so we reexamin our HRIR.

## Align HRIR
If we take a closer look at one pair of HRIR, we could see that it is acrually consists of two part that in some way correspond to what we have discuss before, interaural differences and spectral cue. So we could align all the HRIR with respect to its peak, thus, Isolateing these two factors out and estimate them seperately.
<img src="pic/align.png?raw=true"/>

## Mirrored Dataset
Now if we just look at the shape of HRIR as we have align them all, we could find something interesting. If we took subject 44's HRIR at same elevation but mirrowed azimuth angle, we can see that these two looks very similar.
Why is that? We conclude that it is because the shape of HRIR is heavily dependent on one's pinna shape, which shouldn't different much between the same person's left and rigth ear. So what we could do is mirrored our dataset by flipig the right ear data to the left side.
<img src="pic/mirror.png?raw=true"/>

## Two Seperate Models
So we have now isolate our HRIR into two core components and mirrored our dataset, we can go ahead and trained our models. For this study, we use a Fully Connected ANN to estimate the shape of our HRIR, and we use a Gradient Boost Tree to estimate ITD.
<img src="pic/2model.png?raw=true"/>

## Estimated Result
The neural network showed a significant improve on the HRIR shape.
<img src="pic/shaper.png?raw=true"/>

The estimation on ITD also demonstrating just a little bit of an error, mostly fall under just noticible differences.
<img src="pic/delay.png?raw=true"/>

And here is the complete recontructed HRIR from our inference.
<img src="pic/reconstruct.png?raw=true"/>

## Prensentation and Abstract
I presented our study on Acoustic Society of America (ASA) 177th Meeting, you can access to the recorded presentation [here](https://acousticalsociety.confex.com/acousticalsociety/2019/meetingapp.cgi/Index/Recording~1)

The abstract of this study is publish in The Journal of the Acoustical Society of America 145, 1883 (2019); [https://doi.org/10.1121/1.5101823](https://doi.org/10.1121/1.5101823)




