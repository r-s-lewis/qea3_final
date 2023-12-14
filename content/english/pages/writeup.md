---
title: "QEA3 Final Project: Write Up"
meta_title: "Project Overview"
description: "Detailed explanation of the QEA3 Final Project focusing on advanced audio recognition technology."
image: "/images/project-image.png"
draft: false
---

# Proof of Concept:

This project looked into the possibility of controlling motion based on the pitch of someone’s voice over time. An algorithm that receives raw voice recording data, detects pitch of vocals, and then translates into the relevant units needed was developed. A DFT was used for pitch detection, where the audio recording was filtered to highlight the main frequencies in the clip. The target frequency within the range of a human voice was then detected and scaled for range of motion.

The Discrete Fourier Transform (DFT) in this application is used to take a signal from the time domain and transform it into the frequency domain to detect different pitches with various frequencies. It uses a set of equally spaced samples of a signal as input for transforming the data. 

The algorithm works fairly accurately most of the time, especially when the fundamental frequency has the highest amplitude. Small variations in vocal vibration and external noise cause the graphs to not be perfectly clear, but basic pitch can be properly detected. For certain audios, the model needs to have the timestep for each of the DFT’s to be scaled so that there is no overlap in frequencies for each DFT. When there is overlap, there are often large spikes and the dips in the graph, where pitch isn’t easily identifiable. But overall, the fundamental pitch detection is mostly accurate when audio is clear and the timestep is compatible with the audio. 


# Motion model:
When trying to find the frequencies of human voice from recordings, noise would often make the frequency of some time-steps incredibly large. A human’s voice frequency doesn’t exceed 350 Hz. With this knowledge, frequencies over 350 Hz were filtered out. In addition, the harmonics of the human voice had to be taken into account.  The human voice isn’t a pure tone. When speaking and singing, there is a fundamental frequency of vibration in addition to upper harmonics due to multiple vibrations in the vocal cords. When notes sung are really high pitch, or the pronunciation of a note is very pronounced, harmonics can have a higher amplitude than the fundamental frequency. The prosthetic hand can be programmed through servo motors which have a range of 0 to 180. Therefore, the voice frequency reader filters the data into a range from 0 to around 350 Hz. The top frequency of 350 Hz can be adjustable depending on normal conversational voice frequencies for different people, but that has not been encountered with the various recordings used for this project.

{{< image src="images/audio_data.jpeg" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title"  webp="false" >}}

# Algorithm development and description:

{{< image src="images/plots/DFT_with_frequencies_in_range.jpg" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title"  webp="false" >}}

This is an example of raw audio data with amplitude vs time.

{{< image src="images/plots/qea2_image_2.jpg" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title"  webp="false" >}}

Here is what the normal DFT with frequencies in range looks like. As you can see, the frequency with highest amplitude is at -120 and 120 Hz.

The Fourier transform dissects a waveform signal into a mix of different frequencies, enabling the conversion of a signal between the time and frequency domains. In MATLAB, the fft() function was employed to swiftly perform the Fourier transform on user data in the time domain. Additionally, fftshift() is utilized to reorganize the data, ensuring that the zero-frequency component is positioned at the center of the vector produced by fft().

From here, the audio file was spliced into individual designated time steps. It was found that a timestep around 0.2 seconds worked the best for human voices. The algorithm took in the audio file, split the audio into the chosen time step which was 0.2 seconds in this case, and then separately took the DFT of each. In each timestep of the audio, after taking the DFT, the frequency with the highest amplitude was recorded. 

The targeted frequency would occasionally be masked by a high harmonic frequency. Here is an example where the high frequency harmonic around -500 to 500 Hz overshadows the frequency at -120 and 120 Hz which is wanted. In order to consistently pick out the fundamental frequency, a threshold frequency value that the top frequency at each given timestamp cannot surpass was set. If it does, then it will search for the frequency with the highest amplitude within the given threshold numbers.

{{< image src="images/plots/pitch_graph.jpg" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title"  webp="false" >}}

This data of highest frequency vs time can be used to make a pitch graph. Below are two examples. The first is Anagha's full C major scale, and the second is Khoi's ascending C major scale.

{{< image src="images/plots/Anagha_C_Major_Scale.jpg" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title"  webp="false" >}}

<audio controls style="display: block; margin: 0 auto;">
    <source src="/audio/anagha2.m4a" type="audio/mpeg">
    Your browser does not support the audio element.
</audio>

{{< image src="images/plots/Khoi_Ascending_Scale_Frequency_vs_Time.jpg" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title"  webp="false" >}}

<audio controls style="display: block; margin: 0 auto;">
  <source src="/audio/khoi_scale.m4a" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>

As you can see, sometimes there are errors where the harmonic is below the highest voice frequency. This spike is not optimal for turning this audio data into control instructions for the prosthetic hand. In order to combat this, a control algorithm was implemented to max out large steps to a given value. This algorithm detected whether there was a large and unlikely spike in frequency between a note and the note played before. When singing, notes don’t jump up or down on a large scale since it is quite difficult for humans to do, and melodies often don’t have structure. If the frequency change was over a value of 50 Hz, an additional 20 Hz was added or subtracted to the previous frequency, and the scaling to servo inputs was applied on the altered frequency. 

{{< image src="images/plots/khio_servo.jpg" caption="" alt="alter-text" height="" width="" position="center" command="fill" option="q100" class="img-fluid" title="image title"  webp="false" >}}

As you can see, the controller data ranges from 0 to 180 which is exactly what the servo intakes. In Khoi’s case,  the max step was maxed out for the servo motor to be 10 ticks. This eliminates any large spikes and filters to data that is readable by servos.

# Next Steps:
Building on our successful proof of concept in using human voice control for a prosthetic hand, the next steps are crucial for bringing this innovative idea to fruition. Focus should be put on designing and developing the mechanical and electrical systems and carefully integrating the servo data for precise movement control. This involves selecting appropriate materials and components that ensure durability, responsiveness, and user comfort. Next, the algorithm would be optimized to process live audio feeds with minimal latency. This step is critical to ensure real-time responsiveness of the prosthetic hand, making it as intuitive and seamless as possible for the user. The goal is to create a prosthetic hand that not only functions effectively but also feels like a natural extension of the user’s body, enhancing their day-to-day life.

