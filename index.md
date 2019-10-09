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



