# PAPER 2

FILTERING (removing unwanted noise from signal) AND RESAMPLING DATA (changing sampling data —> to reduce size)

IMPORTED LIBRARIES :

mne, numpy , matplotlib—> plotting graph ,os ——> file handling.

how code will go 

import libraries — load dataset —— select data file —— read raw data  ——-crop and select new channels(magnetometers→ record brain activity ,stim channels → records stimulus signals) ——- load data into memory.

RESULT :  RAW MEG DATA FILE WAS LOADED AND SIMPLIFIED FOR FURTHER USE.

BACKGROUND AND FILTERING 

a filter is used to remove parts of signals. it work by affecting specific frequency ranges in the signal.filter can remove frequencies above, below the cutoff value or can reduce certain unwanted signal components.

REPAIRING ARTIFACTS BY FILTERING

the artifacts which are restricted to narrow frequency range can be repaired by filtering the data.

SLOW DRIFT

Raw brain data sometimes has slow, unwanted  waves —→ slow drift(these are not real brain signals - //noise) they make the data looks unstable.

detect them by plotting a long section of data, turn off the dc shift correction so drifts are clearly visible.

POWER LINE NOISE

Raw brain data sometimes has persistent oscillations (repeated waves) at a fixed frequency ———> power line noise  (this is not a brain signal - //environmental artifact) it makes the data look noisy. 

we can detect it by spectrum plot.MEG channels are most affected by this than EEG.

RESAMPLING

EEG and MEG data are recorded at high sampling rates ——> resampling when high frequency signals are not needed ——> downsample to save time.

downsample—>reducing sampling rate of data. ———→ downsampling without filtering leads to aliasing(fake frequency appear).

MNE-Python automatically applies lowpass filter before resampling → no need to do it manually

Resampling before epoching (on Raw) —→ causes jitter in event timing → affects all future epochs

Resampling after epoching —→ introduces edge artifacts on every epoch.