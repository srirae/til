
## What is a Neural Audio codec?
Neural Audio codec is an audio compression engine works using deep learning. It takes in huge file of raw audio, shrink it to tiny sequence of numbers (codes) for storage or transmission, and then recreate the orginal sound from those numbers on the other side.

[Raw audio wave] &rarr; ENCODER &rarr; [Numbers / Tokens] &rarr; DECODER &rarr; [Reconstructed Waveform]

## Structure of a neural audio codec

##### The Encoder (Compressor)
&rarr; Made up of: A deep neural network made of 1D Convolutional Layers
&rarr; Acts as: Feature extractor
&rarr; How it works: The encoder takes a window of these raw samples, squishes them into a shorter sequence called *latent vector* (a summary of representation of pitch, volume, timbre, and content for that frame)

##### The Codebook / Quantizer (The Translator)
&rarr; Made up of: A lookup table (directory) filled with learned vector representations using a method with Residual Vector Quantization (RVQ)
&rarr; What it does: Converts smooth, continous math vectors into discrete, fixed integers

##### The Brain (The AI model)
The AI models takes the quantsized integers and outputs the discrete integer codebook indices (the quantsized numbers).

##### The Decoder
Does the entire loop from Quantsized discrete array to the unflitered raw audio sample


## Random notes
##### 1D Convolutional Layer
A 1D convolutional layer is like a small sliding window (called a kernel or filter) with a fixed width that moves down the tap from left to right. As it slides, it takes a look at a tiny snippet of the audio, performs a calculation, writes down a new feature and takes a step forward.

* **The Filter (Weights)**: The filter contains a set of learned numbers (weights) that detects specific patterns or templates in the audio wave.
* **The Dot Product (Multiplication & Sum)**: At any given position, the filter overlaps with a few audio samples. It multiplies each audio sample by its corresponding filter weight and adds them all together into a single summary number.
* **Sliding (The Stride)**: 
  * The filter shifts across the audio line one step (or multiple steps) at a time.
  * At each new position, it repeats the dot product calculation.
  * This sliding action creates a brand-new, shorter line of numbers that maps out where the templates were found.

##### Residual Vector Quantization (RVQ)
Lets say our Target vector = [3.5, 2.1]. Overall, audio codecs have around 1 to 32 codebooks according to the target bitrate, audio quality, file sizes and more. The popular default is 8 codebooks. Adding more codebooks adds more quality but forces the AI to process more text-like tokens per second

* **Codebook 1 (The Coarse Outline)**: It will look inside Codebook 1 (2048 enteries). 2048 is not the fixed number of enteries, it depends on the computer architecture such as 2^10 = 1024 [Meta's ENCodec and SoundStream] uses it, or 2^11 - 2048 [Kyutai's Mimi or Descript's DAC]. The Codebook 1 finds the closest vector target [3.5, 2.1] which then says "Entry #412 is the closest match we have [3.0, 2.0]". After that index number *412* is saved. The left overs (The residuals) are moved on.

Target[3.5, 2.1] - Match[3.0,2.0] = [0.5, 0.1]

* **Codebook 2 (The medium Details)**: It takes the left over residue [0.5, 0.1] and looks into the Codebook 2. Codebook 2 will say "Entry #89 is the closest match: [0.4,0.1]". After that we will save the index number 89. As we go through more codebooks the error will reduce.

Final Codebook Indices = [412, 89, 1043, 12, 512, 881, 301, 74]

The reason there are multiple codebooks is because fitting all the entries in one codebook will make the computer run out of memory instantly. By making 8 smaller codebooks of 2048 entries each (8 x 2048), the model can mix and match them to create trillions of unique combinations with less compute power
