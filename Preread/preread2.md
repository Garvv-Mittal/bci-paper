Filtering
- removes or attenuates (weaken) a part of a signal 
- filtering removes those artifacts (unwanted signals or noise) which are frequency specific, meaning if we define a cutoff value for frequency below or above which they are considered to be artifacts, they can be removed or weakened. 
- slow drifts (frequencies which are very low, caused due to external environment or skin conditions like sweating) can be detected using the plot() method. A long-time window (like 60 seconds) is chosen so that the slow changes can be seen easily
- filtering process to remove slow drifts:
  1.  From the plot we analyse the time period. As 1/time period = frequency, we find the drift frequency. Here the frequency was found to be 0.05Hz
  2.  We choose a cutoff value before which all frequencies are removed. Here we have tested 0.1Hz and 0.2Hz to check if they successfully remove all drift frequencies while providing the least damage to data.
  3. For doing this, a for loop is made. A high pass filter is applied (0.1 and 0.2) and then the graphs are plotted again. Then its checked which cutoff frequency is suitable for removing slow drifts.
  4. raw.filter() and create.filter() can be used to get filter parameters to show. this is crucial for determining the technical aspects of how the filter was built
- power line noise comes through the electrical systems (AC supply). In India, it’s typically 50 Hz. 
- to plot power lines a spectrum is used (signal vs power vs frequency) and not a raw plot (signal vs time) because power lines are messy oscillations which are easily seen on the PSD plot
- in the graph, spikes are observed at 50Hz 100Hz... these are called harmonics
- MEG channels are more susceptible to this kind of interference than EEG that is recorded in the magnetically shielded room. This is why we will select only meg
- a notch filter is used remove specific frequencies which makes it perfect for power line and its harmonics unlike high pass and low pass.
- a for loop is used to plot the before and after which helps in comparing them
- notch filters have tunable parameters
- Standard notch: cuts out frequencies
- spectrum_fit: models and subtracts sinusoidal noise


Resampling 
- Changing the sampling rate of your data. This is done to reduce how many data points per second we keep (downsampling).
- Aliasing: High-frequency signals can “fold back” into lower frequencies and corrupt your data.
Example: A 150 Hz signal sampled at 200 Hz, it gets misinterpreted as a lower frequency. This creates fake signals
- To prevent aliasing: Remove high frequencies before downsampling. This is done using a low-pass filter.
- raw.resample() is the built in feature for doing this 
- Nyquist frequency: To correctly represent a signal, your sampling rate must be at least twice the highest frequency present. It is used to overcome aliasing.
- MNE automatically applies a low-pass filter at 100 Hz (Nyquist)
- choose n_fft for Welch PSD to decide how accurate the frequency should be(Welch PSD is a method to estimate the power spectral density of a signal)
- fft: fast fourier transform
  1. Convert signal → frequency domain
  2. Modify it (resize spectrum)
  3. Convert back → time domain
- drawbacks of fft: assumes that the signals are periodi, looks at the whole wave at once
- polyphase is time-domain resampling method, unlike FFT (which works in frequency domain).
  1. add extra samples to increase the sampling rate temporarily
  2. apply low pass filter to remove high frequencies 
  3. remove samples 
- Resample before epoching - timing errors (jitter)
- Resample after epoching - edge artifacts in every epoch
- Best workflow to avoid the above 2 problems: Filter raw → epoch → downsample (not resample raw).