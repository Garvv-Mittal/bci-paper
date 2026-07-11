# Pre-Read 2 — Evidence Accumulation Models for BCI
### CUSUM vs Drift–Diffusion (DDM / SDE)

---

## Why classification alone is not enough

Most EEG-BCI papers frame decoding as a single-shot problem: take a fixed-length EEG window, pass it through a model, get a class label. This works well in controlled lab settings where the system already knows the trial start time — someone presses a button, or a screen cue appears, and the system knows to start listening.

Our problem is different. We are building a system where the user can start imagined speech at any moment, without pressing anything. The system has to run continuously, watch a stream of EEG, and figure out on its own when a word or command has been imagined. That is a fundamentally different kind of problem — not single-shot classification, but **continuous online decision-making**.

Evidence-accumulation models are designed for exactly this. Instead of committing to a decision after a fixed window, they let confidence build up incrementally: each new data point nudges an internal counter up or down, and a decision is made only when that counter crosses a threshold. The threshold controls the trade-off between speed (low threshold → fast, more errors) and caution (high threshold → slow, fewer errors). This speed-accuracy trade-off is something we can tune directly, rather than hoping the model figures it out implicitly.

---

## CUSUM: Cumulative Sum Change Detection

### The idea

CUSUM (Cumulative Sum) is a classical statistical method for detecting the moment a signal shifts away from a known baseline — a "change point." The key quantity it tracks is:

$$S_t = \max\left(0,\ S_{t-1} + (x_t - \mu_0 - k)\right)$$

where:
- $x_t$ is the incoming feature value at time $t$ (e.g. Z-scored alpha band power).
- $\mu_0$ is the expected feature value at baseline (idle, no imagined speech).
- $k$ is a slack parameter — roughly half the expected magnitude of the shift you are trying to detect.

When $S_t$ crosses a threshold $h$, CUSUM declares a change has occurred — in our case, that imagined speech has started.

### What it does in practice

During idle periods, $x_t \approx \mu_0$, so the increment $(x_t - \mu_0 - k)$ is typically negative or near zero. The $\max(0, \cdot)$ clause resets $S_t$ to zero whenever it would go negative, which means the counter does not accumulate evidence for a change that is not there.

When imagined speech begins, $x_t$ starts shifting consistently above $\mu_0$. The increments turn positive, and $S_t$ ramps upward. How fast it reaches $h$ depends on the magnitude of the shift — large, clean imagined speech signals trigger faster than weak or noisy ones.

This makes CUSUM a natural fit for detecting the "brain switch" from idle to active imagined speech. It is fast, lightweight, and has no learned parameters — just $\mu_0$, $k$, and $h$, all of which can be estimated from calibration data.

---

## Drift–Diffusion Model (DDM)

### The basics

The Drift–Diffusion Model is a mathematical framework for two-alternative decision-making. A latent evidence variable $y_t$ evolves continuously over time:

$$dy_t = v\,dt + \sigma\,dW_t$$

- $v$ is the **drift rate** — how fast evidence accumulates on average in favor of one option. A higher drift rate means a faster, more confident decision.
- $\sigma\,dW_t$ is Gaussian noise (Brownian motion) — representing moment-to-moment variability in the evidence.
- A decision is made when $y_t$ crosses one of two boundaries: $+a$ (imagined speech) or $-a$ (idle).

The three main parameters that control behavior are:
- **Drift rate $v$:** higher → faster and more accurate decisions.
- **Boundary $a$:** higher → slower but more accurate decisions (more evidence required).
- **Non-decision time $T_\text{nd}$:** irreducible delay from signal acquisition and motor output, not related to the decision itself.

### EEG as an input to DDM

This is the key insight from Nunez et al. (2017): you do not have to treat the DDM as just a behavioral model. You can map trial-level EEG features directly onto DDM parameters — using, for instance, pre-stimulus alpha power to predict drift rate, or N200 latency to predict non-decision time. This turns the DDM from a descriptive model of decision behavior into an active decoder of EEG state.

For our project, the idea would be: take our normalized EEG features (Z-scored band power ratios) and use them to modulate the drift rate $v$ in real time. When the features are close to baseline, $v$ is small, evidence drifts slowly, and no decision is reached quickly. When imagined speech begins and features shift away from baseline, $v$ increases, and the accumulator crosses the threshold faster.

### Schurger et al. (2012): the leaky accumulator

Schurger and colleagues modeled self-initiated movement onset using a leaky stochastic accumulator — a DDM variant with a leak term:

$$dy_t = -\lambda y_t\,dt + v\,dt + \sigma\,dW_t$$

The leak term $-\lambda y_t$ pulls the accumulator back toward zero whenever it is not being actively driven upward. This matters for imagined speech: during idle periods, even small amounts of neural noise can push the accumulator slightly above zero, and the leak prevents these fluctuations from building into false alarms. Only a sustained increase in drift rate — corresponding to an actual imagined speech event — can consistently overcome the leak and reach threshold.

---

## CUSUM vs DDM: similarities and where they differ

Both methods make decisions by accumulating evidence toward a threshold, and both allow explicit control over the speed-accuracy trade-off through that threshold. That is the core similarity and why both are natural fits for our pipeline.

The differences are in expressivity and interpretability. CUSUM is non-parametric — it makes almost no assumptions about the data-generating process and needs very few parameters to specify. This makes it fast, easy to implement on edge hardware, and easy to diagnose when it misbehaves. DDM and SDE-based accumulators are parametric — they model the full dynamics of the decision process, including noise structure, leak, urgency signals, and non-decision time. This makes them more expressive and interpretable in cognitive terms, but also more complex to fit and more computationally demanding at inference time.

For our project, the practical strategy is to think of them as operating at different levels of complexity: CUSUM as a fast, lightweight onset detector that we implement and validate first; DDM/SDE as a principled framework for understanding *why* certain threshold and normalization choices work, and potentially improving them in later project phases.

---

## How this fits our pipeline

The full evidence-accumulation pipeline has four stages:

**1. Feature extraction and normalization** (Pre-Read 1)
Compute Z-scored relative band power ratios per window. These are the inputs to the accumulator.

**2. Evidence mapping**
Map the feature vector to a scalar evidence signal $e_t$. In the simplest case this is a linear combination of Z-scored features; in a more sophisticated setup it could be a small learned model whose output is interpreted as an instantaneous drift rate.

**3. Accumulation**
- CUSUM: $S_t = \max\left(0,\, S_{t-1} + (e_t - \mu_0 - k)\right)$
- DDM/SDE: $dy_t = v_t\,dt + \sigma\,dW_t$ (with optional leak term)

**4. Threshold and handoff**
When $S_t$ or $y_t$ crosses threshold $h$, Stage 1 flags imagined speech onset and passes the detected segment to Stage 2 (EEGNet / FBCNet / EEG Conformer) for content decoding.

The threshold $h$ is the main tuning knob for the system-level latency-accuracy trade-off. A lower $h$ means faster decisions but more false alarms; a higher $h$ means fewer false alarms but slower responses. We should report performance at multiple values of $h$ — following the format of Sen et al. (2025) from the chosen papers — rather than picking a single operating point and reporting only accuracy.

---

## What the team should take away from this

We are not building a classifier — we are building a two-stage detection-and-decoding system, and the detection stage is the part that determines system latency. A classifier that is 95% accurate on pre-segmented trials is useless if we cannot first tell the system when to start listening.

Evidence accumulation is how we handle that "when." CUSUM gives us a concrete, implementable version of it that we can put in the pipeline immediately. DDM gives us the theoretical framework to reason about parameter choices and to connect what we are doing to the broader literature on decision-making and neural dynamics. Both are worth understanding before we start writing code.
