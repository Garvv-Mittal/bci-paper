# Pre-Read 1 — Adaptive Feature Normalisation for EEG
### Z-Score Normalisation & Relative Band Power

---

## The problem with raw EEG amplitudes

If you record EEG from the same person twice — even with the same cap and the same task — the absolute signal amplitudes will be different. Electrode impedance shifts as sweat builds up under the cap. Drowsiness suppresses low-frequency power. A slight cap rotation moves electrodes a few millimetres and changes coupling to the scalp. None of these changes reflect a real change in the brain state you care about, but they all move your raw numbers.

This creates a specific failure mode for BCI: a threshold or classifier that works well at the start of a session quietly degrades as the session goes on, and fails almost entirely when the user comes back the next day. The problem is worse for imagined speech than for motor imagery, because imagined speech signals are weaker and more variable to begin with.

The fix is to stop working with raw amplitudes and instead work with **normalized features** — representations that describe how much a signal has changed relative to that subject's own baseline, rather than its absolute value in microvolts or raw power units.

---

## Z-Score Normalisation

### What it is

For any feature $x$ (say, alpha-band power at electrode C3), the Z-score is:

$$z = \frac{x - \mu}{\sigma}$$

where $\mu$ and $\sigma$ are the mean and standard deviation of that feature during a **baseline period** — typically 30–60 seconds of eyes-open rest recorded at the start of the session, before any imagined speech.

The result tells you how many standard deviations above or below the subject's own resting state the current value is. A Z-score of +2 means "the signal is two standard deviations above what this person produces at rest" — regardless of whether their absolute resting power is 5 µV² or 50 µV².

### Why it helps

- **Within-session drift:** If a subject's overall power level drifts upward over an hour due to fatigue, both $\mu$ and $\sigma$ shift too when you update the baseline estimate. Z-scores computed against an updated baseline stay interpretable.
- **Cross-subject differences:** Some people simply have higher absolute EEG power than others — thicker skulls, more hair, better electrode contact. Z-scores remove this between-subject scaling, making it more realistic to train a classifier that generalizes across people.
- **Stable thresholds for CUSUM:** This is the practical reason Z-normalisation matters for our architecture. Our CUSUM onset detector works by accumulating evidence that the incoming signal has shifted away from baseline. If the baseline wanders unpredictably in raw units, the CUSUM statistic wanders with it — producing false alarms. Z-scored inputs keep the baseline near zero, so CUSUM increments that remain small during idle and grow during imagined speech become a reliable signal.

### How to compute it in practice

1. Record 30–60 seconds of resting EEG at the start of each session (eyes open, no task).
2. For each feature (each band, each channel), compute $\mu$ and $\sigma$ from this baseline.
3. During real-time operation, convert every new feature value to a Z-score using these estimates.
4. Optionally: update $\mu$ and $\sigma$ incrementally as the session continues (exponential moving average), to track slow within-session drifts without requiring the user to stop and re-baseline.

---

## Relative Band Power

### What it is

Instead of using the absolute power in a frequency band (e.g., raw alpha power in µV²), you use the ratio:

$$R_{\text{band}} = \frac{P_{\text{band}}}{P_{\text{total}}}$$

where $P_{\text{total}}$ is the sum of power across all bands of interest (e.g., 1–40 Hz).

### Why ratios are more stable than absolute values

When a subject's overall EEG amplitude increases — due to fatigue, electrode drift, or any other non-neural cause — all frequency bands tend to increase together. The absolute alpha power goes up, but so does total power, and the **ratio** stays roughly the same. This makes relative band power more robust to hardware-level and physiological confounds that affect all frequencies uniformly.

The key practical insight is that what we care about for imagined speech is not "is there a lot of alpha power?" but "is there *relatively more* alpha power than usual, compared to beta, gamma, and other bands?" That ratio reflects actual changes in brain rhythm composition rather than changes in signal gain.

### Typical usage

- Compute per-channel power in each EEG band (delta 0.5–4 Hz, theta 4–8 Hz, alpha 8–13 Hz, beta 13–30 Hz, gamma 30–40 Hz).
- Normalize each band by total power to get relative band powers.
- Use the resulting vector as your feature input — one number per band per channel per time window.

These ratios can then be Z-scored relative to the resting baseline (step 3 above), giving **Z-scored relative band power ratios** as the final feature input to our pipeline.

---

## How this feeds into our dual-stage architecture

Our front end operates as follows:

1. **Bandpass and artifact removal:** standard preprocessing (notch at 50 Hz, bandpass 1–40 Hz, ASR or ICA for artifact rejection).
2. **Feature extraction per window:** compute absolute band powers for each channel, then normalize each band by total power to get relative band power ratios.
3. **Baseline estimation:** use the resting period at session start to estimate $\mu$ and $\sigma$ for each relative band power ratio.
4. **Z-score normalization:** convert each ratio to a Z-score in real time.

The resulting Z-scored relative band power ratios are what feeds into:

- **Stage 1 — CUSUM change detector:** accumulates positive Z-score deviations from baseline to detect imagined speech onset. Because the baseline is anchored to zero and the variance is normalized, fixed thresholds on the CUSUM statistic become meaningful across sessions and subjects.
- **Stage 1 — DDM drift accumulator (if used):** the Z-score magnitude can be used as a proxy for instantaneous drift rate $v$, so stronger deviations from baseline produce faster evidence accumulation.
- **Stage 2 — Deep classifier (EEGNet / FBCNet / Conformer):** training on Z-scored features rather than raw signals makes the classifier more robust to the between-session and between-subject variability that would otherwise require it to memorize subject-specific gain levels rather than learning the actual signal structure.

The core point this pre-read is trying to establish for the team: working with raw EEG amplitudes is a choice, not a necessity, and it is generally the wrong one. Normalization is not a preprocessing detail — it is what makes the rest of the pipeline work reliably across sessions.
