# OVERVIEW

MNE-Python is an open-source Python library for processing, analysing, and visualising MEG (Magnetoencephalography) and EEG (Electroencephalography) data.
By using data structures like: -

1)raw - it is like a continuous, uncut recording of our brain signal.
2)epochs - small segment of continuous signals cut around at any certain event.
3)evoked - it is used to average out signals after filtering out noise.
4)SourceEstimate - it is used to find which part of brain is passing that signal.

# PIPELINE
Loading raw data → Data Inspection → Preprocessing Data → Event Detection → Epochs → Time-Frequency Analysis → Evoked → Data Localisation

**### LOADING DATA** 
Loading data basically means raw uncut recording of our brain in form of signals. MNE natively uses the FIF format, but it does support other formats (like EDF, BDF, etc.). 
Steps of loading data :-

- sample_data_folder=mne.datasets.sample.data_path() --> it is used to check if the dataset is already in your local system if not then automatically download it from internet .
- raw = mne.io.read_raw_fif(sample_data_raw_file) -->it opens our MNE file, parses all metadata into a Raw object, and prepares it for the rest of the analysis pipeline, without loading the heavy signal data until you actually need it.
- print(raw.info) --> it provides identity card to our datasets with the informatons like - no of channels , Sampaling rate and filtyers etc.
 
**Raw.info?**

chs-->    All channel types — 204 Gradiometers, 102 Magnetometers, 60 EEG, 9 Stimulus, 1 EOG
sfreq--> Sampling frequency (150.2 Hz in sample data)
bads-->  channels marked as noisy/broken
highpass/lowpass--> Applied filter range (0.1–40 Hz here)
projs-->  SSP projectors (noise reduction vectors)
dig-->     Electrode digitisation positions (146 points)

Note
The info object is preserved across Raw, Epochs, and Evoked.It travels with the data through the entire pipeline.
    
# PREPROCESSING
Brain signals are easily contaminated by artifacts like eye blinks (EOG), heartbeats (ECG), muscle noise. ICA(independent componenet analysis) decomposes the signal into independent components so we can surgically remove artifacts.

ICA FLOWCHART

raw signals --> fit ica --> identify bad --> apply ica --> clean raw signal
(376 chs)        (20 comps)   comp[1,2]      

ica = mne.preprocessing.ICA(n_components=20, random_state=97, max_iter=800)---> #376 chs got decomposes and became 20 independent comps 
ica.fit(raw)
ica.exclude = [1, 2]  
ica.plot_properties(raw, picks=ica.exclude)  --> #It opens a detailed 4-panel diagnostic plot for each component we have flagged for removal letting us visually confirm they are genuinely artifacts before permanently deleting them from our data.

# DETECTING EVENTS
Events mark when something happened a stimulus was shown, a button was pressed. In the sample data, these are encoded as a dedicated STIM channel (STI 014).

events = mne.find_events(raw, stim_channel="STI 014") --> #It extracts all the important events/triggers from your EEG data.

NOTE:-
The (/) in key names like "auditory/left" enables hierarchical pooling querying "auditory" auto-selects both IDs 1 and 2.

# EPOCHS

Epoching slices the continuous raw recording into short windows as time locked to each event. 

epochs = mne.Epochs(
    raw, events,
    event_id=event_dict,
    tmin=-0.2, --># 200ms before event
    tmax=0.5, --># 500ms after event
    reject=reject_criteria, --># drop noisy epochs
    preload=True
)
reject_criteria -->it is used to automatically remove bad/noisy epochs based on signal amplitude.

NOTE:-
After epoching use equalize_event_counts() to balance trial counts across conditions it prevents bias in averages.

# TIME-FREQUENCY ANALYSIS

Instead of just looking at signal amplitude over time, TFR analysis try to find out how much power exists at each frequency, at each moment in time.

LET'S SEE HOW IT WORKS 

δ Delta 0.5–4 Hz --> Deep sleep, pathology
θ Theta 4–8 Hz --> Memory, drowsiness
α Alpha 8–13 Hz --> Idle motor cortex, relaxed
β Beta 13–30 Hz --> Active movement, attention
γ Gamma 30–80 Hz --> High cognition, binding

requencies = np.arange(7, 30, 3)  # 7–30 Hz
power = aud_epochs.compute_tfr(
    "morlet",        # Morlet wavelet method
    n_cycles=2,      # cycles per wavelet
    return_itc=False,
    freqs=frequencies,
    decim=3,
    average=True
)
power.plot(["MEG 1332"])

# EVOKED RESPONSES

- Averaging many epochs together cancels out random noise while preserving the brain's consistent response to stimuli. 
- The result is an ERP (Event Related Potential for EEG) or ERF (Event Related Field for MEG).

aud_evoked = aud_epochs.average() -->brain average response for audio stimulus
vis_evoked = vis_epochs.average() -->brain average response for visula stimulus

mne.viz.plot_compare_evokeds(
    dict(auditory=aud_evoked, visual=vis_evoked),
    legend="upper left",
    show_sensors="upper right",
)

aud_evoked.plot_joint(picks="eeg")--> plot_joint shows time and location of waveform
aud_evoked.plot_topomap(times=[0.0, 0.08, 0.1, 0.12, 0.2], ch_type="eeg") -->#plot_topomap shows only location at specific times

- n100 means negative peak at 100ms(approx) happens when brain notices a stimulus(visual/audio).
- p200 means positive peak at 200ms(approx)  happens when brain is analyzing the stimuli.
- p300 means positive peak at 300ms(approx) happens when brain is deciding or recognising it.


# INVERSE MODELING (SOURCE LOCALISATION)
Through inverse modeling we try to estimate where in the brain the signals originate from, using measurements recorded on the scalp or sensor measurements.
- Forward problem means if you find brain source find eeg signals.
- inverse problem means if you got eeg signals find brain source. this is the difficult one because different brain regions can produce similar signals and the signals get mixed before reaching electrodes.Basically ill-posed (no unique solution)  

load inverse operator

inverse_operator_file = (sample_data_folder / "MEG" / "sample" / "sample_audvis-meg-oct-6-meg-inv.fif")
inv_operator = mne.minimum_norm.read_inverse_operator(inverse_operator_file) -->#inverse operator is a precomputed modal containing head model with mathematical mapping 
snr = 3.0 --># signal to noise ration (high snr gives clean data )
lambda2 = 1.0 / snr**2 -->#regularization parameter helps to solve ill posed problem
stc = mne.minimum_norm.apply_inverse(
    vis_evoked, inv_operator, lambda2=lambda2, method="MNE" --># Converts sensor signals to estimated brain activity it contains STC(brain location in the form of vertices and activity over time 
)  

MNE --> Minimum Norm Estimation assumes brain uses minimum energy to produce signals . it is not normalized
dSPM --> Normalized MNE (better contrast then mne ). it depends on noise estimation . 
sLORETA --> it improves localization by smoothing active part(realistic but less precise). 
eLORETA --> It improves localization but creates more complex mapping of brain.




