# NeuroLight Lab: fNIRS Data Analysis Pipeline

Welcome to **NeuroLight Lab**, a comprehensive project that showcases a full pipeline for analyzing functional Near-Infrared Spectroscopy (fNIRS) data. This project is designed for researchers and students working with brain imaging data and provides a hands-on walkthrough from raw data to statistical modeling and interpretation.

---

## Project Overview

This repository demonstrates how to:
- Load and explore `.nirs` files
- Preprocess raw fNIRS signals
- Correct motion artifacts
- Apply robust statistical models including:
  - **Block Averaging**
  - **Generalized Linear Models (GLMs)**
  - **Linear Mixed Models (LMMs)**

Whether you're just getting started or refining your own fNIRS analysis pipeline, this project bridges the gap between theory and practice with detailed examples.

---

# fNIRS Data Exploration

This guide walks you through the process of exploring fNIRS (functional Near-Infrared Spectroscopy) data, including understanding the data structure, discovering the source-detector layout, calculating distances between optodes, and visualizing the raw signals.

## 1. Load the Data

To begin, load the `.nirs` file using the `matfile` function in MATLAB. Here's an example for loading data for a specific subject:


![Alt Text](path_to_image)

---

## 2. Check the Structure of the Data

Once the file is loaded, check the structure to understand its contents:


![Alt Text](path_to_image)

The file will contain several variables, including:

d: Raw light intensity (channels × time)

t: Time vector (in seconds)

s: Stimulus vector (1 = event on)

aux: Auxiliary signals (e.g., accelerometer data)

ml: Measurement list (channel & wavelength mapping)

SD: Source-detector geometry (layout and wavelengths)

systemInfo: Metadata about the acquisition device

For preprocessing, the main variables of interest are d, t, s, and aux for signals and events, and SD and ml for channel and optode information.

## 3. Discover Optode Placement and Wavelengths

To explore the source-detector layout and wavelengths, you can use the following code:


![Alt Text](path_to_image)

## 4. Visualizing Optodes
To visualize the optodes and the layout of the fNIRS channels in 3D, you can use visualization tools in MATLAB. This will help you understand the relative positioning of the optodes in the setup.
![Alt Text](path_to_image)






## Preprocessing Steps

> **Note**: Familiarity with digital signal processing (DSP) is recommended.

### 1. Visual Inspection
Identify noisy channels or time windows to exclude from analysis.

> 📷 _Insert screenshot of raw fNIRS signals_  
> ![Raw Signals](images/raw_signals.png)

### 2. Convert to Optical Density
```matlab
dod = hmrIntensity2OD(d); 
