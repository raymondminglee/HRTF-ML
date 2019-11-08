# Personalizing HRTF Using Machine Learning

## Introduction
Virtual reality (VR) requires rendering accurate head-related transfer functions (HRTF) to ensure a realistic and immersive virtual auditory space. An HRTF characterizes how each ear receives sound from a certain location in space based on the shape of the head, torso, and pinnae, and provides a unique head-related impulse response (HRIR) for each given source location.  
Since HRTFs are person-specific and difficult to measure, machine learning has become a popular way to estimate personalized hrtf.  
On this page, I am going to explain the methodology I use to apply the machine learning algorithm as a way to generate HRTFS, and also the use of ABX and Localization testing to validate its feasibility.

## How Do We Hear Sound
Spatial Audio is creating a 3D audio experience by using headphones, and one important aspect of it is being able to hear where the sound is coming from.  

<img src="pic/3daudio.png?raw=true"/>  

And it turns out humans utilize two primary factors during sound localization, __Interaural Differences__ and __Spectral Cues__.
<br><br>
### Interaural Differences
Interaural differences are just time and intensity differences between our right and left ear when we hear a sound. The time difference is called __Interaural Time Difference, ITD__, and the intensity difference is called Interaural __Intensity Difference, IID__. And naturally, they are heavily related to one's head size.  

<img src="pic/idiff.png?raw=true"/>
<br><br>

### Spectral Cues
Besides ITD and IID, Spectral Cue also paly a big role in sound localization. As sound coming form different places and traveling to our ear canal, our unique pinna shape will produce different reflections of the sound. The add up of all these reflections creates different spectral cues between our ears.  

<img src="pic/spectral.png?raw=true"/>
<br><br>

### Head-Related Transfer Function
A pair of Head-Related Impulse Response (HROR) incorporates all these aspects of sound localization. Its a pair of impulse response of a particular sound in a particular place. A Head-Related Transfer Function (HRTF) is HRIR presented in the frequency domain.  

<img src="pic/hrir.png?raw=true"/>
<br><br>

## Acquiring HRTF
Due to the reason mention above, HRTFs are related to one's body shape, so each of us has a unique set of HRTFs. We could measure HRTF by placing microphones in one's ear and measuring all its impulse response to all locations. Naturally, this is expensive and time-consuming.  

<img src="pic/measure.png?raw=true"/>

So it comes to our mind that we could use Machine Learning to estimate personalized HRTF.  

<img src="pic/ml.png?raw=true"/>
<br><br>

## Dataset
We have the CIPIC HRTF Database as our training data, it contains
* 35 Subjects 
* 1250 measurements (25 azimuths, 50 elevations) 
* Anthropometric measures 
    * 17 Head and Torso Dimensions
    * Left Ear Dim
    * Right Ear Dim
<br><br>

## Previous Research
Previous research by Chun etc. (2017) has shown some promising results. While trying to replicate their findings using a Fully Connected Neural Net, although we could get __lower MSE (-26.8 dB)__, the estimated HRIR did not resemble the actual one on its shape, so we reexamine our HRIR.
<br><br>

## Align HRIR
If we take a closer look at one pair of HRIR, we could see that it actually consists of two parts that in some way correspond to what we have discussed before, interaural differences and spectral cue. So we could align all the HRIR with respect to its peak, thus, Isolating these two factors out and estimate them separately.  

<img src="pic/align.png?raw=true"/>
<br><br>

## Mirrored Dataset
Now if we just look at the shape of HRIR as we have aligned them all, we could find something interesting. If we took subject 44's HRIR at the same elevation but mirrored azimuth angle, we can see that these two look very similar.
Why is that? We conclude that it is because the shape of HRIR is heavily dependent on one's pinna shape, which shouldn't differ much between the same person's left and right ear. So what we could do is mirrored our dataset by flipping the right ear data to the left side.  

<img src="pic/mirror.png?raw=true"/>
<br><br>

## Two Separate Models
So we have now isolated our HRIR into two core components and mirrored our dataset, we can go ahead and train our models. For this study, we use a Fully Connected ANN to estimate the shape of our HRIR, and we use a Gradient Boost Tree to estimate ITD.  

<img src="pic/2model.png?raw=true"/>
<br><br>

## Estimated Result
The neural network showed a significant improvement on the HRIR shape.  

<img src="pic/shaper.png?raw=true"/>


The estimation on ITD also demonstrating just a little bit of an error, mostly fall under just noticeable differences.  

<img src="pic/delay.png?raw=true"/>

And here is the complete reconstructed HRIR from our inference.  

<img src="pic/reconstruct.png?raw=true"/>
<br><br>

## Presentation and Abstract
I presented our study on the Acoustic Society of America (ASA) 177th Meeting, you can access to the recorded presentation [here](https://acousticalsociety.confex.com/acousticalsociety/2019/meetingapp.cgi/Index/Recording~1)

The abstract of this study is published in The Journal of the Acoustical Society of America 145, 1883 (2019); [https://doi.org/10.1121/1.5101823](https://doi.org/10.1121/1.5101823)

PS: Since it is still an ongoing study, I will post the code and unfinished materials after I finished this project. 




