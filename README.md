# ODDITY LAB 2.0 — GENESIS

**A standalone, reproducible browser sandbox for exploring unusual synthetic neuron architectures and testing whether they actually help.**

ODDITY LAB compares a conventional **fixed recurrent reservoir** with a **grafted experimental reservoir** that can use a coupled second soma, axonal receiving branches, an extra signalling channel, rewiring, transmission delays, persistent state traces, glial-inspired regulation, and error-driven neuromodulation.

**Research status:** This is an educational and exploratory computational model, **not** a validated model of biological neurons, a proven neuromorphic architecture, or evidence of consciousness. A candidate may perform better, equally, or worse. Improvements require repeated, appropriately controlled testing.

## Start in seconds

1. Open **[index.html](index.html)** in any modern browser, such as Chrome, Edge, or Firefox.
2. Choose a prediction task and a random seed.
3. Press **Play**, or advance with **Single step**.
4. Compare the green fixed baseline with the purple grafted candidate.
5. Use **Ablate all** to verify that both networks give identical outputs with experimental features disabled.
6. Re-enable grafts individually, then use **Paired multi-seed replication** to assess robustness.

The complete main application is one HTML file. It uses inline HTML, CSS, and JavaScript, runs locally/offline, and requires **no account, server, build step, Python installation, package manager, or external dependency**.

To run with a development server instead, an optional command is:

~~~bash
python -m http.server 8000
~~~

Then navigate to http://localhost:8000/ .

GitHub Pages can also host this repository's root **index.html** if Pages is enabled in repository settings.

## Main features

### Experimental synthetic grafts

| Module | Computational hypothesis |
| --- | --- |
| **Dual soma** | A second coupled nonlinear state adds another temporal integration compartment |
| **Axonal dendrites** | Feedback from outgoing neighbours changes the neuron's effective input |
| **Vector signal** | A slow, phase-coded secondary channel provides richer-than-scalar state |
| **Rewiring** | Hebbian adaptation and periodic edge replacement alter connectivity |
| **Delay lines** | Individual synapses carry 1–3-step delayed signals |
| **Trace memory** | Persistent activity traces support longer temporal dependencies |
| **Glial governor** | Activity-dependent gain regulation may stabilize state dynamics |
| **Neuromodulator** | Lagged prediction-error magnitude changes the candidate's learning gain |

Each graft can be enabled or disabled independently. Changes to the architecture or task restart matched runs so comparisons remain interpretable.

### Six binary prediction tasks

- **2-step delayed echo:** Recall a bit from two time steps ago.
- **6-step delayed echo:** Test longer temporal retention.
- **Temporal XOR:** Predict XOR of current and previous stimuli.
- **3-bit parity:** Compute parity of a short temporal window.
- **Previous-five majority:** Estimate the majority of five previous bits.
- **Hybrid Boolean rule:** Combine delayed inputs and the present bit.

Choose 16, 24, or 36 neurons and vary the training rate, shared noise, and simulation speed.

### Real-time visual diagnostics

- Side-by-side animated connectomes and activation states
- Synaptic connectivity matrix view
- Rolling prediction accuracy and relative log-loss
- Prediction probability meters
- Selected-neuron inspector and time-series traces
- Average activation, computation proxy, rewiring count, and modulation state
- Event and intervention log
- Live binary stimulus stream

### Interventions and continual adaptation

- Pause, resume, single-step, and restart by seed
- Inject candidate-only spikes
- Lesion or restore an individual candidate neuron
- Invert and restore the target rule to observe drift and re-learning
- Freeze learning for evaluation, then resume it
- Clear temporary interventions without deleting historical observations

**Important:** Candidate-only shocks and lesions break direct matched-condition comparisons for the affected period. Reset before a fresh controlled comparison.

## Experimental protocol

A sensible first experiment:

1. Select **6-step echo**, seed **4242**, and the default 24-neuron network.
2. Run at least 1,000 online training steps.
3. Freeze learning and inspect performance on subsequent stimuli.
4. Reset with all grafts disabled. Confirm the candidate equals the baseline.
5. Enable **trace memory** alone, followed by **delay lines** alone, using the same task and seed.
6. Run the integrated multi-seed benchmark with multiple seeds.
7. Compare both **accuracy differences** (percentage points) and **log-loss differences**; lower log-loss is better.
8. Repeat on another task to check generality.

The **Paired multi-seed replication** control automatically runs **1,000 training steps plus 300 frozen test steps per seed**, reports per-seed results, and estimates a mean paired accuracy difference with an approximate 95% between-seed confidence interval. Seed choices, finite samples, and multiple-hypothesis testing still matter.

A proposed graft is worth further investigation only if gains persist across independently selected seeds, tasks, stronger controls, and reasonable compute budgets.

## Measurements and reproducibility

**Prequential evaluation:** Every online prediction is scored *before* its target updates the learning rule. The baseline and candidate start from matched network topology and readout initialization, receive the same input stream, and receive matched common noise. When **all graft modules are off**, the candidate is intended to be an **identity control** for the fixed baseline.

The application records:

| Metric | Meaning |
| --- | --- |
| Rolling accuracy | Fraction of correct recent binary classifications |
| Accuracy gap | Candidate minus baseline, in percentage points |
| Log-loss difference | Candidate log-loss minus baseline; negative favours candidate |
| Rewires | Number of accepted structural edge replacements |
| Mean activity | Mean absolute reservoir activation |
| Compute proxy | Approximate relative model work, not measured energy or runtime |
| Frozen test accuracy | Accuracy after disabling model updates |

### Data exports

- **CSV:** Step-by-step stimuli, labels, probabilities, correctness, losses, rewiring, evaluation flags, and computation proxies.
- **JSON receipt:** Configuration, seed, experiment notes, intervention history, raw run data, batch results, and a configuration fingerprint.

The configuration fingerprint uses **32-bit FNV**, which is a convenient non-cryptographic checksum. It is **not a secure hash**, an immutable archive, or proof against tampering. Store exported results and source revision IDs separately for stronger provenance.

## Files

| File | Purpose |
| --- | --- |
| [index.html](index.html) | Full ODDITY LAB 2.0 GENESIS application — recommended entry point |
| [legacy/oddity_lab_v1.html](legacy/oddity_lab_v1.html) | Original ODDITY LAB simulator preserved for regression and comparison |
| [pilot.html](pilot.html) | Compact interactive ODDITY LAB 2.0 pilot |

The two archived editions are supplementary; **index.html** is the full-featured primary experiment.

## Architecture (high level)

1. Generate a deterministic binary stimulus stream from the selected seed.
2. Feed the identical stream and matched noise to both reservoirs.
3. Update neuronal states using recurrent connectivity and, for the candidate, enabled graft mechanisms.
4. Compute binary probabilities with a trainable logistic readout.
5. Score the predictions against the current task's target **before training**.
6. Update readouts online unless frozen; apply candidate plasticity only if enabled.
7. Record measurements, visualization state, and any interventions.
8. Re-run with independent seeds and frozen testing for paired comparisons.

The candidate and baseline have the same *number* of neurons, but they are **not perfectly matched for state capacity or computational cost** when grafts are enabled. Additional studies should include parameter-matched, state-matched, compute-matched, and random-feature baselines.

## Known limitations

- Synthetic mathematical abstractions do not reproduce real neuron morphology, electrochemistry, or verified neuron-to-neuron signalling.
- Some extra mechanisms may increase state capacity or computation without being inherently more efficient.
- The classification tasks are intentionally small and are not benchmarks for general intelligence.
- Repeated hyperparameter exploration can inflate apparent significance.
- Reported confidence intervals are approximate and depend on between-seed assumptions.
- Browser simulation speed and the compute proxy do not establish hardware-level energy efficiency.
- Random seeds provide reproducibility within the implementation; browser/math-library differences and future code changes may matter.
- JSON receipts have no cryptographic signature or external attestation.
- The simulator does **not** establish consciousness, sentience, emotion, or biological plausibility.

## Potential next research upgrades

- CPU/GPU profiling with recorded hardware and environment details
- Explicit parameter-matched and FLOP-matched baselines
- External, fixed train/test bitstreams and withheld task suites
- Additional independent synthetic temporal tasks and drift schedules
- Neuron state recording and replay
- Automated one-factor ablations and bootstrap uncertainty estimates
- Formal deterministic regression tests and continuous integration
- Versioned manifests, benchmark sealing, and independent validation receipts

## Project information

**Project:** ODDITY LAB — GENESIS  
**Repository:** https://github.com/kai9987kai/ODDITY-LAB  
**Context:** Independent experimental synthetic-connectome research inspired by Sonapse-LAB and dual-soma/axonal-graft ideas.  
**Current version:** 2.0

Use this software to explore falsifiable hypotheses, not to assume that more complex neural wiring will improve performance.
