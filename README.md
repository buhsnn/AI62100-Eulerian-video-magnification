# Eulerian Video Magnification

**Course:** AI621 – Computational Image Generation and Manipulation  
**Semester:** Fall 2025  
**Student:** Bushra Monika Hossain   

## Overview

This project implements **Eulerian Video Magnification (EVM)**, a technique introduced by researchers at MIT (Wu et al., 2012) to reveal subtle variations in videos that are invisible to the human eye.

Instead of tracking motion explicitly, the method:

1. Decomposes each frame using a **Laplacian pyramid**
2. Applies **temporal band-pass filtering** to isolate specific frequencies
3. **Amplifies** the filtered signal
4. Reconstructs the video with the amplified variations

This technique can reveal phenomena such as:

- subtle **blood flow changes in skin color**
- **breathing motion**
- tiny physical vibrations

## Project Goals

The objective of this assignment was to:

- Reproduce results from the original **MIT Eulerian Video Magnification paper**
- Implement the full pipeline in Python
- Experiment with parameter tuning
- Apply the method to a **custom video recording**

## Pipeline

The implementation follows the main steps described in the paper.

### 1. Color Space Transformation

Frames are converted from **RGB → YIQ**.

- **Y channel**: luminance (used for motion magnification)
- **I/Q channels**: chrominance (used for color amplification)

This separation allows independent amplification of motion or color variations.

### 2. Laplacian Pyramid

Each frame is decomposed into a **multi-scale Laplacian pyramid**.

This separates spatial frequencies and allows amplification of specific motion scales.

Key idea:

Gaussian Pyramid → downsample frames  
Laplacian Pyramid → difference between adjacent Gaussian levels

### 3. Temporal Filtering

For each pyramid level, the pixel time series is filtered using a **Butterworth band-pass filter**.

This isolates specific frequency bands such as:

- heart rate
- breathing motion
- slow physical vibrations

### 4. Signal Amplification

The filtered signal is amplified by a factor **α** and added back to the original signal.

Careful parameter tuning is required to avoid amplifying noise.

### 5. Reconstruction

Finally, the Laplacian pyramid is collapsed to reconstruct the amplified video frames.

---

## Experiments

Three video sequences were processed.

### Face Sequence

Goal: amplify subtle **blood flow color variations**.

Parameters:

- Channels: **IQ**
- Frequency band: **0.83–1.0 Hz**
- Amplification: **α = 100**

Result:

Subtle pulsations in skin color become visible, synchronized with the subject's heartbeat.

---

### Baby Sequence

Goal: amplify **breathing motion**.

Parameters:

- Channel: **Y**
- Frequency band: **2.33–2.67 Hz**
- Amplification: **α = 150**

Result:

The baby’s chest movement becomes visibly exaggerated, revealing the breathing rhythm.

---

### Custom Experiment – Coffee Video

A custom video of a **cup of coffee** was recorded.

Expected result:

Reveal tiny surface vibrations on the liquid.

Observed result:

Instead of clear ripples, the algorithm mainly amplified **contrast and lighting variations** around the cup.

This illustrates a known limitation of EVM:

> When the signal-to-noise ratio is low, the algorithm may amplify noise or illumination changes instead of real motion.

---

## Results

Examples of amplified effects include:

- visible **color pulsations** on the face
- amplified **breathing motion**
- contrast amplification in the coffee experiment

The full project report with detailed explanations and visual results can be found here:

**Report:**  
`Report_EVM.html`

---

## Repository Structure

```
.
├── data/
│   ├── face.mp4
│   ├── baby2.mp4
│   └── coffee.mp4
│
├── output/
│   ├── output_face.mp4
│   ├── output_baby2.mp4
│   └── output_coffee.mp4
│
├── Eulerian_Video_Magnification.ipynb
├── Report_EVM.html
└── README.md
```


---

## Key Challenges

Several issues were encountered during the implementation:

**Memory consumption**

Processing Laplacian pyramids for all frames requires significant RAM.

**Noise amplification**

Small errors or lighting variations can easily be amplified.

**Parameter sensitivity**

The results depend heavily on:

- frequency band selection
- amplification factor
- pyramid depth

---

## Reference

Wu, H.-Y., Rubinstein, M., Shih, E., Guttag, J., Durand, F., & Freeman, W. (2012).  
*Eulerian Video Magnification for Revealing Subtle Changes in the World.*

MIT CSAIL.

---

## Notes

This project was implemented **for educational purposes** as part of the AI621 course.  
External code was not used, in accordance with the assignment policy.
