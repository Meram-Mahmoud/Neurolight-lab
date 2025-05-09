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

To convert raw intensity data to optical density


### 3. Motion Correction

Motion artifacts are common in fNIRS data and can significantly affect signal quality. To address these artifacts, several motion correction techniques can be applied:

### Common Techniques for Motion Artifact Correction

- **Wavelet Filtering**: Suppresses motion artifacts by removing outlier coefficients in the wavelet domain, typically based on interquartile range (IQR) thresholds.
  
- **Principal Component Analysis (PCA)**: Identifies and removes components associated with motion by decomposing the signal into orthogonal components and filtering out those attributed to noise.

- **Spline Interpolation**: Interpolates motion-contaminated segments of the signal using smooth spline curves.

- **Kalman Filtering**: Applies a recursive filter to estimate the underlying true signal while minimizing the impact of motion-induced noise.

### Specific Methods Mentioned

1. **PCA-Based Motion Correction**  
   - Uses PCA to detect and remove components related to motion.
   - Produces a corrected dataset by reconstructing the signal without the noisy components.

2. **Wavelet-Based Motion Correction**  
   - Utilizes a statistical threshold (e.g., IQR) to detect and eliminate motion artifacts in the wavelet-transformed data.
   - Helps preserve the integrity of the signal while reducing motion-induced noise.

### 4. Filtering Techniques

Frequency-Based Filters (Low pass, Band pass, High pass)

PCA Filters: Decompose the signal and remove high variance components using PCAFilter(nSV).

### 5. Baseline Correction
Methods like Bandpass Filtering, Baseline Subtraction, and GLM with Drift Correction are used for baseline correction.

### 6. Modified Beer-Lambert Law (MBLL)
To convert optical density to concentration changes of HbO (oxyhaemoglobin), HbR (deoxyhaemoglobin), and HbT (total haemoglobin)


### 7. Short Separation Channel
Use short-separation channels for GLM to regress out the component of the LS signal correlated with SS signal.

### Final Preprocessing Notes
Preprocessing steps vary depending on the quality of the raw data and the specific study objectives.

## Statistical Analysis

### Core Questions in fNIRS Analysis

- Which brain areas were activated?
- Are there differences between conditions or groups?

---

### Statistical Models

#### 1. Block Averaging
- Ideal for simple, block-based experimental designs.
- Allows visual inspection of hemodynamic responses before applying more complex models.

#### 2. Generalized Linear Models (GLM)
- A robust statistical model used to estimate task-related activations.
- Based on the model:

  \[
  Y = X.β + ε
  \]

  - **Y**: Observed data  
  - **X**: Design matrix (predictors)  
  - **β**: Parameters to estimate  
  - **ε**: Error term (assumed white noise)

##### GLM Assumptions
- Task responses are deterministic (non-stochastic).
- Errors (noise) are independently and identically distributed (i.i.d.).
- Predictors in X are not linearly dependent on each other.

##### GLM Steps
1. **Pre-whitening**  
   - Removes autocorrelation using autoregressive models.

2. **Setup Design Matrix and Response Variable**  
   - Define X (stimuli/events) and Y (recorded signal).

3. **Solve using Ordinary Least Squares (OLS)**  

### Linear Mixed Models (LMMs)

**Linear Mixed Models (LMMs)** are an extension of Generalized Linear Models (GLMs) that incorporate **random effects**, making them suitable for analyzing hierarchical or repeated-measures data.

#### When to Use:
- When modeling **individual variability** across subjects.
- When accounting for both **fixed effects** (e.g., task conditions) and **random effects** (e.g., subject-specific responses).

#### Key Features:
- **Random intercepts per subject**: Each subject has their own baseline level.
- **Random slopes per condition**: Each subject may respond differently to different conditions.

#### Benefits:
- Improves **statistical power** by reducing unexplained variance.
- Enhances **generalizability** by accounting for subject-level variability.
