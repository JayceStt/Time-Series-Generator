# Time Series Generator and FFT Filter

A Python signal-processing project for generating synthetic time-series signals, analyzing them in the frequency domain, and applying FFT-based filters.

This project started as a Jupyter Notebook for experimenting with sinusoidal time-series generation and was expanded into a standalone Tkinter desktop app with input controls and a full graphing interface.

The app allows users to generate signals from multiple sinusoidal components, optionally add white noise, apply high-pass, low-pass, or band-pass filters, and compare the original and filtered signals visually.

<img width="864" height="864" alt="Screenshot 2026-06-28 at 1 08 13 PM" src="https://github.com/user-attachments/assets/67f33fef-3fa6-4769-b1e1-5e67f439074d" />


---

## Overview

This project demonstrates basic signal generation, spectral analysis, and digital filtering concepts using Python, NumPy, Matplotlib, and Tkinter.

The user can build a custom time-series signal by choosing:

* Number of sinusoidal components
* Start and end time
* Sampling interval
* Frequencies
* Amplitudes
* Phases
* Optional white noise
* FFT filter type and cutoff values

After generating the signal, the program displays the results in both the time domain and frequency domain. The filtered signal can then be compared directly against the original signal.

---

## Features

* Generate a time series from multiple sine waves
* Choose random, set, or manual values for:

  * Frequency
  * Amplitude
  * Phase
* Add optional uniform white noise
* Plot the generated signal in the time domain
* Compute and plot the power spectral density
* Apply FFT-domain filters:

  * No filter
  * High-pass filter
  * Low-pass filter
  * Band-pass filter
* Reset the filtered signal back to the original signal
* Compare original and filtered signals visually
* View all graphs in one desktop window
* Save the graph window as a PNG image
* Build the app into a clickable executable using PyInstaller

---

## Desktop App Interface

The Tkinter version includes a control panel on the left and a graphing window on the right.

The graph window displays:

1. Original time-domain signal
2. Original frequency-domain power spectrum
3. Filter response
4. Time-domain comparison before and after filtering
5. Frequency-domain comparison before and after filtering
6. Generated sinusoid parameter table

This makes it easier to experiment with signal parameters and immediately see how the filter changes the signal.

---

## Sampling Configuration

The original notebook uses a fixed sampling interval:

```python
SampF = 0.01
```

This means the time step between samples is:

```text
Δt = 0.01 seconds
```

This corresponds to a sampling frequency of:

```text
fs = 1 / Δt = 100 Hz
```

The Nyquist frequency is therefore:

```text
f_Nyquist = 50 Hz
```

This is why generated random frequencies are commonly selected between 0 Hz and 50 Hz.

In the desktop app version, the sample interval `dt` is editable, but the same sampling logic applies.

---

## How It Works

### 1. Input Parameters

The user enters the number of sinusoidal components and the time range for the signal.

Frequency, amplitude, and phase values can be generated in three ways:

| Mode   | Description                                     |
| ------ | ----------------------------------------------- |
| Random | Generates values within a selected range        |
| Set    | Uses fixed or automatically stepped values      |
| Manual | Uses comma-separated values entered by the user |

Example manual frequency input:

```text
1, 11, 21
```

---

### 2. Signal Generation

Each sinusoidal component is generated using:

```python
A * sin(2πft + φ)
```

where:

| Symbol | Meaning   |
| ------ | --------- |
| `A`    | Amplitude |
| `f`    | Frequency |
| `t`    | Time      |
| `φ`    | Phase     |

The final signal is created by summing all sinusoidal components together.

Optional white noise can also be added to the signal.

---

### 3. Frequency Analysis

The Fast Fourier Transform is used to analyze the generated signal in the frequency domain.

The power spectral density is calculated from the FFT result and displayed in decibels:

```python
psd = abs(fft_series) ** 2 / n
psd_db = 10 * log10(psd)
```

This allows the frequency components of the generated signal to be visualized clearly.

---

### 4. Filtering

The project supports three main FFT-based filtering operations.

| Filter Type | Description                               |
| ----------- | ----------------------------------------- |
| High-pass   | Attenuates frequencies below a cutoff     |
| Low-pass    | Attenuates frequencies above a cutoff     |
| Band-pass   | Keeps frequencies within a selected range |

The filters are applied in the frequency domain by multiplying the FFT of the signal by a custom filter response.

The filtered signal is then converted back into the time domain using the inverse FFT.

---

## Example Workflow

1. Run the notebook or desktop app.
2. Enter the number of sine waves to generate.
3. Choose the time range and sample interval.
4. Select random, set, or manual values for frequency, amplitude, and phase.
5. Add optional white noise.
6. View the generated time-domain signal.
7. View the frequency-domain power spectrum.
8. Apply a high-pass, low-pass, or band-pass filter.
9. Compare the original and filtered signals visually.
10. Save the graph window if needed.

---

## Project Structure

```text
.
├── Time Series Generator and Filter for Sample rate equal to 0.01.ipynb
├── time_series_filter_ui.py
├── README.md
└── dist/
```

The notebook version is useful for step-by-step experimentation.

The `time_series_filter_ui.py` version is the standalone desktop app.

---

## Requirements

This project requires Python 3 and the following libraries:

```text
numpy
matplotlib
jupyter
```

For the desktop UI version, Tkinter is also required. Tkinter is included with most Python installations.

Install the main dependencies with:

```bash
python3 -m pip install numpy matplotlib jupyter
```

For building an executable, install PyInstaller:

```bash
python3 -m pip install pyinstaller
```

---

## Running the Notebook

Clone the repository:

```bash
git clone https://github.com/your-username/your-repo-name.git
```

Navigate into the project folder:

```bash
cd your-repo-name
```

Start Jupyter Notebook:

```bash
jupyter notebook
```

Then open:

```text
Time Series Generator and Filter for Sample rate equal to 0.01.ipynb
```

---

## Running the Desktop App

Run the Tkinter app with:

```bash
python3 time_series_filter_ui.py
```

This opens a desktop window with input controls and all graphs displayed together.

---

## Creating a Clickable Executable

### macOS

Install PyInstaller:

```bash
python3 -m pip install pyinstaller
```

Build the app:

```bash
python3 -m PyInstaller --onefile --windowed --clean --name "TimeSeriesFilter" time_series_filter_ui.py
```

The app will be created in:

```text
dist/TimeSeriesFilter.app
```

With a macOS icon:

```bash
python3 -m PyInstaller --onefile --windowed --clean --name "TimeSeriesFilter" --icon icon.icns time_series_filter_ui.py
```

---

### Windows

Install PyInstaller:

```bash
python -m pip install pyinstaller
```

Build the executable:

```bash
python -m PyInstaller --onefile --windowed --clean --name "TimeSeriesFilter" time_series_filter_ui.py
```

The executable will be created in:

```text
dist/TimeSeriesFilter.exe
```

With a Windows icon:

```bash
python -m PyInstaller --onefile --windowed --clean --name "TimeSeriesFilter" --icon icon.ico time_series_filter_ui.py
```

---

## Educational Purpose

This project was built as a signal-processing exercise to explore:

* Time-series generation
* Sinusoidal signal construction
* White noise
* Fourier transforms
* Power spectral density
* Frequency-domain filtering
* High-pass, low-pass, and band-pass filter behavior
* Basic desktop GUI development with Tkinter
* Packaging Python programs as standalone executables

It is intended as a learning project for understanding how signals can be created, analyzed, visualized, and filtered using Python.

---

## Main Technologies

* Python
* Jupyter Notebook
* Tkinter
* NumPy
* Matplotlib
* Fast Fourier Transform
* PyInstaller

---

## Author

Created by Jayce Scott.

---

## License

This project is open source and available for educational use.
