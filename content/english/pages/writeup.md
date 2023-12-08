---
title: "QEA3 Final Project: Advanced Audio Recognition"
meta_title: "Project Overview"
description: "Detailed explanation of the QEA3 Final Project focusing on advanced audio recognition technology."
image: "/images/project-image.png"
draft: false
---

## Introduction
Welcome to our project on Advanced Audio Recognition. This project uses modern techniques to improve how audio commands are processed and understood.

## Project Description
Our goal is to improve voice command recognition systems. We use advanced algorithms for better accuracy and efficiency.

### Our Approach
We use MATLAB code to to analyze an audio files, specifically focusing on the frequencies present in the file and how they change over time.

## Technical Details
### Audio File Processing and Plotting
- Loading and Reading Audio File: The audio file is loaded and read into two variables, `y` (the audio data) and `fs` (the sampling frequency).
- Time and Sampling Calculations: It calculates the total time of the audio clip in seconds and sets up variables for the time (`time`), the sampling period (`T`), and a time vector (`t`).
- Audio Channel Selection: The audio data `y` is reduced to a single channel if it's stereo.
- Plotting the Waveform: The audio waveform is plotted against time using plot(`t, y,'b'`).

### Frequency Analysis
- Setting Parameters for Analysis: It defines parameters like time_step, threshold, and max_step for segmenting and analyzing the audio file.
- Segmentation and Fourier Analysis: The audio is divided into segments based on time_step. For each segment, it performs a Fast Fourier Transform (FFT) to convert the time-domain signal into its frequency-domain representation.
- Frequency Analysis Per Segment: In each segment, it identifies the highest frequency component that is below a defined threshold frequency. This is to isolate the fundamental frequency of a musical note.
- Plotting Frequency vs Time: A plot is generated to show how the identified frequency changes over time, which is useful for analyzing a scale or a melody.

### Distance Calculation
- Calculating 'Distance':The code calculates a 'distance' value for each segment, trying to relate the frequency to a spatial concept like height.
- Plotting 'Height' vs Time: Finally, we plot these 'distance' values against time, which represent how the 'height' changes over time.

### Fourier Transformations in Audio Processing
[Discuss how Fourier Transformations are used in your project to interpret audio data.]

```matlab
% MATLAB code snippet demonstrating a Fourier Transformation
t = 0:0.01:2*pi; % Time vector
y = sin(2*pi*10*t); % Example sine wave
Y = fft(y); % Fourier Transformation
f = (0:length(Y)-1)*100/length(Y); % Frequency range
plot(f, abs(Y)) % Plotting the spectrum
title('Fourier Transformation Example')
xlabel('Frequency (Hz)')
ylabel('Magnitude')