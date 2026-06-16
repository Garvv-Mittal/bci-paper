# FILTERING AND RESAMPALING DATA

--> Why do we need to filter EEG/MED data ??
Raw brain signals are contaminated by noise at multiple frequency bands like powerline hum, muscle artifacts, drift, and more. Filtering isolates the biologically relevant frequency range from that noise before any analysis or classification.
for example :-
1)Low-frequency drift --> Slow voltage wander below ~0.1 Hz from skin, sweat, movement
2)Powerline noise --> electrical interference standard 50hz
3)Muscle noise --> High-frequency contamination from facial/jaw muscles (>100 Hz)
4)Signal of Interst --> the brain activity you actually want to measure and analyze as opposed to all the noise and artifacts contaminating the recording like delta (0.5–4 Hz), alpha (8–12 Hz), gamma (>30 Hz)

# SIGNAL CLEANING OVERVIEW

Raw signals --> Highpass --> Lowpass --> Notch --> Clean Signal

# Types Of Filters
1)Highpass --> Passes high freqs, blocks low freqs below cutoff at f_low (f>f_low)
2)Lowpass --> Passes low freqs, blocks high freqs above cutoff f_high (f<f_high)
3)Bandpass --> Passes a band between f_low and f_high (f_low<f<f_high)
4)Notch(bandstop) --> Blocks a specific freq band.For example (50hz band to cancel powerline noise)

Note :-  a bandpass of 1–40 Hz is a very common starting point. It removes slow drift and high-frequency muscle noise while preserving all major brain rhythms (delta through gamma).
 
# Filter Types

1) Finite Impulse Ressponse (FIR)
Uses a finite window of past samples. Always stable. Can be made linear phase (zero distortion of timing).
pro : Phase-neutral, predictable, stable
cons : need a long filter (large order) hence needs more data at edges

2) Infinite impulse Response (IIR)
Feeds output back into input (recursive). Much shorter filter length but introduces phase distortion.Generally used in real time work where latency matter.
Pro : Computationally cheap, short filter
cons : Non-linear phase & can smear ERP(Event Related Potential) timing

3) Zero Phase Filtering
MNE applies FIR filters forwards then backwards (zero-phase). This doubles the effective order but eliminates any phase lag widely used for critical ERP timing accuracy.

raw.filter(l_freq=1.0, h_freq=40.0, method='fir', phase='zero') -->used to select method 'fir'

# Filtering in MNE-Python
All MNE data like (Raw, Epochs, Evoked) have a .filter() method. It modifies data in-place by default.

Basic syntax 
1)Bandpass: keep 1 – 40 Hz
raw.filter(l_freq=1.0, h_freq=40.0)

2)Highpass only (remove drift)
raw.filter(l_freq=0.1, h_freq=None)

3)Lowpass only (remove EMG) 
raw.filter(l_freq=None, h_freq=40.0)

4)Filter a subset of channels only
raw.filter(l_freq=1.0, h_freq=40.0, picks='eeg')

emg--> freq by muscle contraction 

# Key Parameters
1)l_freq --> Lower cutoff sets highpass edge (None = no highpass)
2)h_freq --> Upper cutoff sets lowpass edge (None = no lowpass)
3)method --> 'fir' (default) or 'iir'
4)phase	--> 'zero' (forward+backward) or 'minimum'
5)filter_length	 --> Length of FIR filter
6)picks	--> Which channels to filter ('eeg', 'meg', indices…)

# note
.filter() modifies Raw in-place. If we want call raw file call raw.copy().filter(...) first.


# NOTCH FILTERING (POWERLINE NOISE)
Powerline interference at 50 Hz (India) shows up as a sharp spike.The .notch_filter() method removes it without disturbing surrounding frequencies.
Remove 50 Hz powerline --> raw.notch_filter(freqs=50)
raw.notch_filter(freqs=[50, 100, 150, 200]) --> multiple harmonics explicitly 


# RESAMPLING: DOWNSAMPLE AND UPSAMPLE 
Raw EEG data is often sampled at high rates (1000–2400 Hz). Most brain signals of interest exist below 150 Hz. Downsampling saves memory and speeds up computation.

Nyqist Theorem --> the golden rule 
To represent a frequency f, you need a sampling rate of at least 2f. Sampling below 2f causes aliasing means high frequencies fold back and corrupt lower frequencies.

# NOTE
Always lowpass filter before downsampling. If you downsample without filtering first, frequencies above the new Nyquist limit will alias and corrupt your data. MNE's resample() does this anti-aliasing automatically.

# RESAMPLING METHODS
1) DOWNSAMPLING --> Reduce sampling rate. Fewer samples = smaller files, faster analysis.
raw.resample(sfreq=250) --> from 1000hz to 250hz 

2) UPSAMPLING --> Increase sampling rate. Generally,used when combining data recorded at different rates.
raw.resample(sfreq=1000) --> from 250hz to 1000hz

# ORDER OF ENTIRE OPERTIONS

LOAD DATA --> HIGHPASS FILTER (remove slow drift(l_freq 1hz)) --> NOTCH (removes powerline(50hz & harmonics)) --> LOWPASS FILTER(removes emg,h_freq4 40-100hz) --> RESAMPLE(Downsample after filtering) --> CLEAN SIGNAL 

# IMP SYNTAX

1) Bandpass 1–40 Hz -->raw.filter(1.0, 40.0)	
2) Highpass only --> raw.filter(0.1, None)	
3) Lowpass only --> raw.filter(None, 40.0)	
4) Notch 50 Hz (India) -->	raw.notch_filter(50)	
5) Downsample to 250 Hz --> raw.resample(250)	
6) Copy before filtering --> raw_filt = raw.copy().filter(...)
7) Use IIR for real-time --> raw.filter(..., method='iir')	
8) Filter EEG channels only --> raw.filter(..., picks='eeg')	