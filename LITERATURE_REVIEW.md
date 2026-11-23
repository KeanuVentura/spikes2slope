# Spikes2Slope — Literature review: optotagging criteria, cell-type features, and spike-to-LFP modeling

**Research date:** November 22, 2026  
**Status:** Working literature notes for discussion with Liz Kaplan and Austin; **not** an approved labeling or modeling protocol.  
**Context:** Allen Visual Coding Neuropixels; `checkpoint2.ipynb`. This document intentionally lives outside the notebook so plots and code remain presentation-focused.

## Executive takeaways

1. **A single spike across a session's light presentations is not convincing standalone evidence of optotagging.** The AllenSDK rate-ratio example is a useful *candidate screen*, not a universally validated identity criterion. More stringent published protocols combine light-locked response elevation with latency, jitter, reliability, or sustained responses.
2. **Do not discard weak candidates before inspecting raw event times.** The scientifically appropriate next step is to verify pulse timing, artifacts, baseline windows, and trial-level spike responses, then assess a pre-specified validation rule.
3. **PV, SST, and VIP are not reliably identified by waveform width alone.** Published multifeature classification shows useful but imperfect discrimination; results from Neuropixels Ultra cannot be assumed to generalize to Allen's older Neuropixels probes.
4. **Austin's paper directly bears on our larger Spikes2Slope question.** In an Allen-derived *simulated* mouse V1 model, dynamic spike-to-LFP kernels reconstruct simulated LFPs well, while upstream synaptic inputs can dominate the LFP despite covariance with local PV activity. Thus a spiking–LFP association need not demonstrate that local spikes generate the recorded field.

## 1. What counts as optotagged?

### A. AllenSDK: optotagging tutorial (Visual Coding)

**Source:** [AllenSDK Optotagging Analysis](https://allensdk.readthedocs.io/en/latest/_static/examples/nb/ecephys_optotagging.html), section “Identifying Cre+ units.”  
**Exact quotation:** > “We can select a threshold, such as 2x increase in firing rate”

**Method described:** Compute baseline firing in **−10 to −2 ms** and evoked firing in **+1 to +9 ms**, relative to the onset of the 10-ms light pulse. The example selects `evoked_rate / (baseline_rate + 1) > 2` to avoid division by zero. Allen also warns that spikes at pulse onset/offset can be light artifacts, and that late responses could be visually driven rather than direct optical activation.

**Our interpretation:** Our `matched_candidate` flag replicates an educational screening approach. The threshold does **not** by itself demonstrate statistical significance, trial reliability, or genetic identity. A very low baseline (including zero) makes ratio-based selection particularly vulnerable to sparse-event effects.

**Related protocol note:** Allen documents the LED-to-laser hardware transition via `session_id >= 789848216`, with reported **maximum fiber-tip outputs** of 4 mW (LED) and 35 mW (laser). These are not delivered-power measurements for individual epochs. [Same tutorial, “Types of optotagging stimuli”](https://allensdk.readthedocs.io/en/latest/_static/examples/nb/ecephys_optotagging.html).

### B. Wei et al. (2023): mouse V1 optotagging with an explicit rate floor

**Source:** [“Associations between in vitro, in vivo and in silico cell classes in mouse primary visual cortex,” *Nature Communications* (2023)](https://doi.org/10.1038/s41467-023-37844-8).  
**Exact quotation (short excerpt):** > “higher than 25 Hz”

**Method described:** They counted spikes in **2–8 ms** of a 10-ms light stimulus to avoid artifacts and identified units with mean evoked rate >25 Hz **and** >2.5 times their corresponding pre-stimulus baseline rate. These are study-specific criteria, not universal thresholds.

**Our interpretation:** This is direct precedent for requiring more evidence than an arbitrary evoked/baseline ratio. We cannot simply apply 25 Hz to Allen without checking stimulation intensity, trial counts, and response distributions.

### C. Allen Visual Behavior Neuropixels: a four-part confirmation protocol

**Source:** [“Map of spiking activity underlying change detection in the mouse visual system,” optotagging methods](https://pmc.ncbi.nlm.nih.gov/articles/PMC12632887/).  
**Exact quotation:** > “We defined four criteria, requiring that optotagged neurons exhibit fast, reliable spiking”

**Method described (SST/VIP recordings):**
- Exclude the first **1.5 ms after onset** and last **1.5 ms before offset** as artifact-prone.
- Demand a pulse response above baseline mean + **2 SD**, using baseline windows sampled from intertrial intervals.
- Require median first-spike latency **<8 ms** and first-spike jitter (median absolute deviation) **<2 ms**.
- Require **at least 30%** of cosine-ramp time bins with significantly elevated firing (`p < 0.01` under the paper's procedure).

**Our interpretation:** Particularly relevant because the data and Cre lines are close to our context; however, this uses a **different experimental dataset and stimulus design**. We should check whether every component (especially a cosine ramp) is available for the Visual Coding sessions before adapting the criteria.

### D. Neuropixels Opto (2026): explicit pulse reliability

**Source:** [“Neuropixels Opto: combining high-resolution electrophysiology and optogenetics,” *Nature Methods* (2026)](https://doi.org/10.1038/s41592-026-03076-z).  
**Exact quotation:** > “the driving emitter evoked at least one spike in 30% of the trials”

**Method described:** The authors combined an increase in response to several pulses, >=30% trial reliability, and short evoked-spike latency (<8 ms) as part of their optotagging definition. This is a specific multi-emitter, short-interval stimulus protocol, not directly equivalent to Allen Visual Coding.

**Our interpretation:** The distinction between **one spike in a single trial** and **at least one spike in many trials** is crucial. Having one spike in 45 independent presentations would correspond to ~2.2% response incidence, not 30%, assuming the count truly refers to one event across all presentations.

### Synthesis for Liz: evidence for/against counting a single spike

| Question | Literature-supported assessment |
|---|---|
| Does a spike inside the light window justify initial screening? | Potentially, as part of a **rate-based candidate screen**, provided artifacts and timing are handled. |
| Can one spike across an entire protocol establish a reliably tagged unit? | **Not supported as a standalone identification rule** by the protocols reviewed here. |
| Is one spike *per trial* sufficient? | It may contribute to strong evidence **when repeated across many trials** with short latency and robust baseline comparison. |
| Should we exclude all currently weak candidates immediately? | **No.** Inspect original stimuli/spikes; determine if weak counts were defined per pulse, per condition, or across pooled conditions, and quantify reliability. |
| What should we call the 1,127 current units? | **Screen-positive / putative optotagging candidates**, pending validation. |

**Important:** The selected publications exemplify multiple conventions; this is **not a systematic review** establishing a single accepted community-wide criterion. No publication here proves that our current single-spike units are false positives.

## 2. Can SST and VIP be distinguished from extracellular spike waveforms?

### Key study: Neuropixels Ultra

**Source:** [“Ultra-high density electrodes improve detection, yield, and cell type identification in neuronal recordings,” *bioRxiv* / PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC10473688/).  
**Exact quotation:** > “approximately 75% accuracy”

**Evidence:** In their optotagged PV/SST/VIP evaluation, a classifier achieved approximately **0.75** accuracy using single-channel summary features, **0.77** using full single-channel waveform, and **0.74** using summary features plus spatial footprint, on *Neuropixels Ultra* recordings. Classifier performance depends on probe density, feature representation, sampling and the study's validation design. The same paper reports that footprint and ISI can provide information beyond duration alone, and that conventional Neuropixels-like recordings may be less informative for some feature sets.

**Interpretation:** Above-chance discrimination of PV/SST/VIP is possible **in that study**, but **SST vs VIP alone** is a separate binary task and is not established to be reliably solved by simple waveform duration, half-width or peak-to-trough ratios. Classifier accuracy is not a definitive cell-identification guarantee.

**What to test later:** On independently validated optotagged units, compare (a) waveform summary features, (b) aligned full waveforms, (c) ISI/bursting/baseline rate and possibly multichannel footprint. Use **held-out animals or sessions**, not randomly split units from the same session, and report a confusion matrix with per-class sensitivity, especially SST↔VIP.

**Our own exploratory observation (not independent validation):** In Checkpoint 2, Sst and Vip candidate waveforms and K-means cluster memberships substantially overlapped; Pvalb was more represented in the shorter-duration group. Those patterns are descriptive and may be confounded by session, anatomical location, and candidate-selection rules. **K-means was our own analysis** on Kilosort-sorted units, not Kilosort cell-type labeling.

## 3. Austin's paper: Meneghetti et al. (2025)

**Citation:** Meneghetti, N., Rimehaug, A. E., Einevoll, G. T., Mazzoni, A., & Ness, T. V. (2025). **“Kernel-based LFP estimation in detailed large-scale spiking network model of mouse visual cortex.”** *Neural Networks*, 192, 107866. https://doi.org/10.1016/j.neunet.2025.107866  
**Source:** PDF supplied by Austin, **22 pages**; key results pp. 1–2, 8–12, 16–19. This is **a modeling study**, not direct empirical validation of our Allen optotagging labels.

### Question and approach

Can a computationally tractable method reconstruct the LFP produced by a detailed mouse V1 network **from population firing rates**? The paper evaluates a convolution-based model using **spatiotemporal kernels** derived from neuronal morphology, synapse placement and strengths, connectivity, and postsynaptic dynamics. It compares kernel-estimated LFPs against ground-truth LFPs generated by expensive **multicompartmental simulations plus volume-conductor modeling**.

The model is based on an Allen-derived mouse V1 network with approximately **52,000** biophysically detailed neurons (plus a larger point-neuron surround). Presynaptic sources include local populations and external LGN, lateromedial cortex (LM), and background inputs. The model uses PV, Sst, and **Htr3a** inhibitory families; **Htr3a is broader than VIP** and should not be treated as a one-to-one VIP population label.

### Main results

- **Static vs dynamic kernels:** In the L2/3 analysis, fixed kernels gave median R² approximately **0.82** for flashes but only **0.28** for drifting gratings. Accounting for changing postsynaptic membrane potential and effective conductance increased median R² to approximately **0.98** and **0.96**, respectively (paper pp. 8–10, Fig. 2).
- **External input dominance in the model:** LM feedback dominated upper-layer LFP contributions, whereas LGN was especially influential in L4. Contributions attributed to local V1 spiking were smaller in the simulated field than those driven by external synaptic sources (pp. 10–14, Figs. 3–5). These are **model predictions**, not empirical causal results.
- **PV association vs source attribution:** PV dominated the *local inhibitory* contribution in the model and covaried with the total LFP, but external inputs could drive both PV spikes and the measured field. Thus spike-triggered LFP correlation can misattribute causal generators (pp. 10–12, Fig. 3).
- **Limits:** The approach performed poorly for several L5 subfamilies with asymmetric firing distributions, which violate key model assumptions (pp. 15–16, Fig. 6). The authors explicitly call for experimental validation of the prediction that upstream inputs dominate.

**Exact quotation (abstract, p. 1):** > “local synaptic activity from V1 neuronal families contributed only marginally to the LFP.”

**Exact quotation (discussion, p. 18):** > “requires empirical validation.”

### Why this matters to Spikes2Slope

**Central conceptual warning:** A relation between PV spikes and the LFP (or its spectral exponent) might reflect **shared external inputs**, synchrony or postsynaptic geometry, rather than a direct scalar readout of local inhibitory spiking or E/I balance. This challenges naive mechanistic interpretations of spike–aperiodic-slope correlations.

**Potential longer-term modeling route (proposal, not approved):** Once we have appropriate unit/region/time coverage, consider population-rate convolution with biologically motivated LFP kernels and ask whether the **observed 1/f exponent or broadband power** is captured. Important obstacles include incomplete recorded population coverage, unknown synaptic weights/locations, unobserved upstream inputs, and transferability of model-derived kernels to Allen experimental recordings.

**Important negative finding for our immediate task:** This paper does **not** test single-spike optotagging criteria, validate SST/VIP waveform discrimination, or demonstrate that the aperiodic exponent uniquely indexes E/I balance.

**Paper's code:** https://github.com/nicolomeneghetti/LFPkernel_AllenInst_V1model (listed in paper p. 20). Kernel framework: https://github.com/LFPy/LFPykernels.

## 4. Research decisions and the next checkpoint

**Do not make these decisions without discussion with Liz:**
- Do not equate `matched_candidate` with validated genetic identity or use it as indisputable SVM ground truth.
- Do not impose a borrowed threshold (e.g., 25 Hz or 30% trials) without checking the actual Allen pulse protocol and sensitivity to low firing rates.
- Do not classify the remainder of gray/unmatched spike-sorted units as biologically excitatory.
- Do not elevate K-means cluster IDs into neuron-type labels.

**Proposed next deliverable: `checkpoint3.ipynb` — “Optotagging response validation.”**

1. Inspect `session.optogenetic_stimulation_epochs`, pulse lengths and levels, timing relative to prior pulses, and whether trials were counted separately or pooled.
2. Audit the baseline and evoked windows (Allen example: −10 to −2 ms and +1 to +9 ms); check whether boundary conventions and artifact exclusions behave as intended.
3. Create a small light-aligned raster/PSTH comparison including **single-spike / low-, moderate-, and high-response** candidates across Cre lines, with explicit trial counts.
4. Quantify spike probability by trial, baseline response, latency and jitter where estimable. Consider a statistical test against baseline and multiplicity control **after** confirming the experimental design.
5. Compare candidate retention under several **predeclared exploratory criteria**, retaining original labels and documenting disagreement; seek Liz's approval before any supervised-label definition.

### Questions to take to Liz

- Should our primary scientific objective prioritize **high-confidence specificity** of optotagging over candidate yield for SVM training?
- Which pulse type(s), light intensities, and trial repetitions are comparable across LED and laser sessions?
- How should we handle low baseline counts and units with only one spike across all pulses?
- Does the literature on SST/VIP separation justify prioritizing multifeature electrophysiology over waveform-shape-only models?
- Should Austin's kernel-modeling direction be pursued separately from the immediate cell-identification/validation work?

---

## References

1. Allen Institute. *Optotagging Analysis* (AllenSDK tutorial). https://allensdk.readthedocs.io/en/latest/_static/examples/nb/ecephys_optotagging.html
2. Wei et al. (2023). *Associations between in vitro, in vivo and in silico cell classes in mouse primary visual cortex.* https://doi.org/10.1038/s41467-023-37844-8
3. *Map of spiking activity underlying change detection in the mouse visual system* (Allen Visual Behavior Neuropixels, methods). https://pmc.ncbi.nlm.nih.gov/articles/PMC12632887/
4. *Neuropixels Opto: combining high-resolution electrophysiology and optogenetics* (2026). https://doi.org/10.1038/s41592-026-03076-z
5. *Ultra-high density electrodes improve detection, yield, and cell type identification in neuronal recordings.* https://pmc.ncbi.nlm.nih.gov/articles/PMC10473688/
6. Meneghetti et al. (2025). *Kernel-based LFP estimation in detailed large-scale spiking network model of mouse visual cortex.* https://doi.org/10.1016/j.neunet.2025.107866

**Quotation policy:** Quoted passages are deliberately brief; the surrounding analyses are paraphrased. Always use the linked methods or the PDF page location when citing a quote in meeting notes, presentations, or manuscripts.
