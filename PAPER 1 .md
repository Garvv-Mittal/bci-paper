# PAPER 1

MNE PYTHON IS A TOOL TO ANALYSE BRAIN SIGNALS.

EEG —— Electroencephalography ( a technique in which electrical activity of brain is measured )

MEG —— Magnetoencephalography (  a technique in which magnetic activity of brain is measured )

2 libraries imported for the project are NumPy( no. and arrays ) and MNE ( to analyse brain data )

WORKING:-

LOAD THE DATA —> SPLIT INTO SMALL PARTS( EPOCHING ) ——>AVERAGE THE DATA ——> PLOT GRAPH  ——>ESTIMATE BRAIN ACTIVITY.

LOADING DATA

**Loading Data in MNE-Python**

**Dataset Loading ——**mne.datasets.sample.data_path()——>gets dataset path (download if not present).

**Selecting the Data File——**sample_audvis_filt-0-40_raw.fif ———>contain EEG/MEG data 

---

**Reading the File ——** mne.io.read_raw_fif()——>loads data into raw object.

GRAPH- 1st EEG vs frequency 

               2nd GRADIOMETER vs frequency 

               3rd MEG vs frequency 

 PREPROCESSING OUTCOME :

ICA(INDEPENDENT COMPONENT ANALYSIS) IS USED TO CLEAN UP THE UNWANTED DATA. 

WHEN 2 COMPONENTS ARE IDENTIFYED AS ARTIFACTS. EX- COMPONENTS #1 AND #2 ——>

THEY ARE MARKED (EXCLUDED) ——>LOAD DATA INTO RAM ——> SUBTRACTED FROM SIGNAL ——>MAKE A COPY (COMPARISON).

EXCLUDE- parameter - remove selected components. they number the components need to be removed.

apply()- Method - when we decide which components need to be remove apply() removes it from real data.

load_data()- apply() needs full data so before apply() all the data is loaded into ram with this method.

q) how we detect component 1 and 2

DETECTING EXPERIMENTAL EVENTS:

STIM CHANNELS - they record signals from stimulus computer which comes as trigger which mark important events like stimulus start , types of stimulus , button press.

COMBINE STIM CHANNELS - merges multiple stim channels into one where different voltage value represents different event type.

STIM CHANNEL used is  called  STI 014  ——> we pass this channel to mne.find_events (find the time and identity of stimulus event)  

RESULT:: numPy array of 3 column 

1 column- sample number ; 2 column- blank ; 3 column - event id.

EVENT DICTIONARY: used to extract epochs from continuous data.

/ - helps in grouping conditions.

plot_events - function like this is used to visualise the distribution of events.

GRAPH UNDERSTANDING::  TIME VS EVENTS

X- axis — time 

Y-axis — event id

dot is the event 

EPOCHING CONTINIOUS DATA 

The raw object and the events array are needed to create an epoch object ———>which we create with epoch class constructor(specify some data quality constraints): 

which reject any epoch where peak-to-peak signal amplitude is beyond reasonable limit——> done with a rejection dictionary.

epoch creation- event_id→ uses event dictionary, tmin and tmax→ define time around event

preload=true→ loads data into memory.

EQUALIZING EVENT COUNT -Used to make the number of epochs equal across different condition,Extra epochs are randomly removed,Done using equalize_event_counts()

SELECTING CONDITION-Used to separate data based on type of stimulus

- aud_epochs → auditory data.
- vis_epochs → visual data.

TIME-FREQUENCY ANALYSIS-

1.Used to analyze signal in both time and frequency

2.Done using  mne.time_frequency module

3.Compute induced power for auditory epochs

4.Method used: Morlet wavelets (To analyze EEG/MEG signals in **both time and frequency,**Auditory signals **change over time**, so simple frequency analysis is not enough).

ESTIMATING EVOKED CONDITIONS

Evoked response is obtained by averaging epochs of same condition, done separately for auditory and visual data.

Used to compare responses of auditory vs visual stimuli using mne.viz function.

more detailed view of each evoked object using  plot_joint and plot_topomap  for detailed view——>Focus on EEG channels.

Evoked objects can be combined to compare conditions.using  mne.combine_evoked.

INVERSE MODELING 

estimate the origin of brain [activities.In](http://activities.In) MNE python we can do this by - dynamic statistical parametric mapping, dipole fitting, beamformers, minimum norm estimation (MNE).

MNE—> uses a linear inverse operator to project EEG+MEG  sensor measurement to source space.