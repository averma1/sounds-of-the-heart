# Sounds-of-the-Heart

Sounds Generator is the generative model originally based off the Dance Diffusion model, edited by Ploy Pruekcharoen and Anika Verma. We use the model to create the piece in Max MSP titled Sounds of the Heart.

## How To Run the Code
Download the ploy-and-anika_sound-generator.ipnyb file and follow the steps listed in the notebook. You can download the /sounds/normal.wav and /sounds/arrhythmic.wav to use in the generation part of the notebook. For interpolation, you can download the other environmental sounds such as the jazz, bird-chirp, and storm files.

## Data
We have provided the normal and arrhythmic heartbeat wav files that were used in the generation, along with the sound files used for interpolation. If you have your own ECG files you would like to use, follow the instructions in the ecg-to-wav.ipnyb to first convert the ECG files to .wav files. For the datasets we used, the MIT-BIH Arrhythmia Database can be found [here](https://www.physionet.org/content/mitdb/1.0.0/). The MIT-BIH Normal Sinus Rhythm Database can be found [here](https://www.physionet.org/content/nsrdb/1.0.0/).

## Sound
To use the interpolation function of the sound, download the environmental sounds in the /sounds folder, or use your own .wav files after uploading them.

## Architecture and Analysis
Dance Diffusion is a diffusion model, which is a denoising model. Diffusion models consist of two steps: forward diffusion and reversing. The first process maps data to noise, so it starts with a data sample and iteratively generates noisier samples using Gaussian diffusion. So at each step, we add Gaussian noise to the data. The second step undoes the forward diffusion and performs denoising iteratively. This generates data by converting the random noise values into realistic data.

maestro-150k is one of six models that is included with Dance Diffusion. It was trained on a subset of piano clips from the [MAESTRO](https://magenta.tensorflow.org/datasets/maestro) dataset, which contains MIDI and audio files of virtuousic piano compositions. 

The model we used is a based off CRASH, which uses a Convolutional 1D neural network. Dance Diffusion further uses U-Net as its backbone. If we had access to the original model, we may have been able to play with changing the input layers and sizes, learning rate, and epochs, rather than using the standard U-Net off-the-shelf that was included in Dance Diffusion.

Since we used a pre-trained model, there were many things we could not tweak. The model we ended up choosing for our audio-visual aspect, maestro-150k, was originally trained on virtuosic piano excerpts and MIDI piano music. Our original plan was to use MIDI files. We were going to use [Music Generator] (https://github.com/llSourcell/Music_Generator_Demo) but ran into problems with our MIDI output when we converted the heartbeat files to MIDI. Our output was inconsistent and occasionally silent, and did not resemble heartbeats at all. If we had gotten this to work or had used other input data that translated to MIDI better, we may have been able to take advantage of the fact that our model was trained paritally on MIDI data and may have gotten better results that way. Instead, we had to change the model we used fairly last-minute.

## Quantifying and Scoring the Model
Since we were working with a pre-trained model, it was difficult to quantify and identify how we should score our results. What makes a good heartbeat when we don't know the accuracy of the model? We relied on the graphs we created of the heartbeats that were generated using the model and compared them to ECG graphs. This was easier to see for the arrhythmic heartbeats, which was our focus, since the arrhythmic heartbeats were not as evenly spaced as the regular heartbeats. Additionally, we could hear 'clicks' with some of the generated heartbeats, which is a common sign of mitral valve prolapse. We found some of the PTB dataset, which we didn't use, contained data with patients who had hypertrophy, of which mitral valve prolapse is a type. We compared these and found the rhythms to be similar when we plotted them. Though we didn't have time to get to this, we're curious to see how different the generation may be if we categorized the arrhythmic heartbeat data prior to generation. Would the generation be able to pick up on the nuances that vary with each condition? Our guess is that we would need more data and would have to use a model we train ourselves rather than a pre-trained model.
