### Verified descriptive results (conditional on hardware-period mapping)
- **LED: 261.0/694.0 (37.6%)** candidates have exactly one total evoked spike and zero baseline spikes; 15 sessions, 4295 QC-eligible units, 694 candidates (16.2% yield).
- **Laser: 0.0/433.0 (0.0%)** candidates have exactly one total evoked spike and zero baseline spikes; 13 sessions, 4120 QC-eligible units, 433 candidates (10.5% yield).
- Pooled candidate mean rates: LED 11.18 Hz; Laser 52.12 Hz. **Laser is higher** in these processed data.
### Statistical comparisons
- Preferred session means: LED − Laser = -32.83 Hz, 95% CI [-47.00, -18.67], t(12.71)=-5.018, p=0.0002519; n=15/13.
- Exploratory pooled units; potentially pseudoreplicated: LED − Laser = -40.94 Hz, 95% CI [-47.07, -34.81], t(460.61)=-13.124, p=1.186e-33; n=694/433.
### Possible confounds
- **Selection/exposure constraint:** one spike with zero baseline can pass only with fewer than 62.5 pulses. LED sessions have 45 pulses; laser sessions have 75 or 151. The primary 0% laser result is therefore enforced by the detector and exposure, not independent evidence of greater reliability.
- Hardware period shares session, animal, acquisition era, genotype composition, stimulation levels, and presentation-count differences. Counts and yields answer different questions; similar counts do not establish equivalence.
- Total-spike criteria depend on exposure; the inherited ratio rule and candidate conditioning also affect the observed distributions. Zero baseline and weak evoked activity do not prove invalid tagging.
- Session-level inference is preferred, but observational confounding persists. Assess magnitude and confidence intervals alongside p-values; biological importance requires a domain-specific benchmark.
### Unresolved questions
- Independently verify the session-ID device mapping; it is a historical proxy, not direct device metadata established here.
- Establish calibrated optical intensity, pulse-resolved response reliability, latency, artifact checks, and an agreed low-response definition. Saved pooled rates cannot provide these. Per-intensity inference would require a separate cached-epoch/spike analysis.
- Confirm the intended anatomical scope (visual cortex versus LGd) and label/QC policy with Liz and Austin. Exploratory tests have no multiplicity correction.
### Figures for review
- **04_AUSTIN_low_spiking_100pct** directly answers Austin’s percentage question; pair with **05_low_response_companion** and **07_threshold_sensitivity**.
- **02_rate_distributions** and **08_session_level_comparison** address firing strength and independent-session variation; **01_counts_and_yield** separates raw counts from yield.

Outputs (PNG, PDF, tables, provenance): `/Users/keanuventura/Documents/GitHub/GOESWaterVaporVisualization/valentines/spikes2slope/opto_results/checkpoint2_figures/run_20261115_175548_818416`.