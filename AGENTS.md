# Spikes2Slope — Instructions for Codex

## Project and scientific purpose
Spikes2Slope is an exploratory computational-neuroscience project in the Voytek Lab at UC San Diego. Keanu Ventura works with Liz Kaplan, Austin, and PI Bradley Voytek. The research asks when, if ever, aperiodic LFP spectral properties relate meaningfully to recorded spiking and excitation/inhibition (E/I)-related circuit properties, and where simple E/I interpretations fail.

Do not represent exploratory plans as finalized protocols or preliminary observations as validated findings. Scientific accuracy and Keanu's understanding take precedence over producing code quickly.

## Sources of truth and freshness
- Read this file, the repository README, and relevant notebook cells before editing.
- If `PROJECT_CONTEXT.md` is present locally, use it as **historical/foundational** project context. It is not a continuously updated research log.
- Current decisions and assignments may live in the team's Spikes2Slope Google Drive meeting notes and Slack conversation. Codex must not claim to have read them unless explicitly provided or connected and queried. Ask for the relevant source or state the limitation when current decisions affect implementation.
- Keep source provenance: identify whether a requirement comes from a confirmed team decision, a draft document, an assumption, or an exploratory suggestion.
- Reuse existing code, metadata fields, caches, and conventions before inventing replacements. Avoid adding fabricated results or citations.

## Present scope and repository contents
- `sessionscheck.ipynb`: exploratory Allen Visual Coding Neuropixels metadata/session-by-region coverage analysis.
- `spikeseda.ipynb`: exploratory single-session LFP, probes, channels, and optogenetic metadata investigation.
- `visual_coding_coverage.csv`: existing exported coverage table, **not** a table of confirmed usable LFP channels.
- The planned comparison with Visual Behavior Neuropixels and other methods remains contingent on the latest team decisions. Do not silently assume that the notebook's current methods have been approved.
- The separately maintained `ekaplan7/spikes_to_slope` repository is associated with Liz; do not push there without explicit authorization.

## Neuroscience guardrails
1. Kilosort identifies spike clusters/putative units, not molecular cell types. Narrow/broad extracellular waveform classes are imperfect proxies; never equate them definitively with inhibitory/excitatory neurons or PV/SST/VIP identity.
2. Define the precise E/I-related measurement: unit count, class-specific firing rate, optotagged identity/activity, or synaptic conductance/current are not interchangeable.
3. Do not equate an anatomical channel metadata count with the count of stored, QC-passing LFP traces. Check `has_lfp_data`, probe mapping, spatial subsampling, and actual LFP coordinates when appropriate.
4. Distinguish Allen Visual Coding from Visual Behavior Neuropixels caches, session IDs, schema, and optotagging protocols. Confirm API availability and imports in the installed AllenSDK before relying on them.
5. Treat the aperiodic exponent as a spectral descriptor, **not** a proven universal E/I biomarker. Record fit frequency range, PSD parameters, SpecParam version/mode and fit-quality checks. Compare alternative spectral features when the analysis question warrants them.
6. Align spike/LFP windows carefully; check sample rates, timestamps, gaps, state/stimulus epochs, and inclusive interval endpoints. Account for nested sessions, probes, channels, regions and correlated sliding windows in analyses.
7. Research ideas involving LFPykernels, optogenetic perturbations, and cross-region comparisons are proposals until verified with current team materials.

## Coding and notebook workflow
- Prefer a brief Markdown explanation followed by one clean Python code cell per conceptual step. Use descriptive variable names, small reusable helpers, and minimal dependencies.
- Inspect the existing notebook order, outputs, and environment before changing code. Do not blindly regenerate notebooks, clear outputs, or reorder cells with meaningful dependencies.
- Preserve original results and raw files. Make modest, logically grouped changes, show a diff/summary, and make it easy for Keanu to understand and defend each decision.
- Separate exploratory findings from validated results in prose and filenames.
- Default to metadata-only operations for coverage work. Allen electrophysiology downloads can be huge: reuse `~/allen_neuropixels_data` when available; never redownload or stream large data without need and permission. Do not commit cache files, credentials, raw NWB/LFP binaries, or other large generated data.
- Environment historically used macOS/VS Code/Jupyter, Python 3.10, conda `allen_env`, AllenSDK 2.16.2, and pinned legacy scientific dependencies. Verify installed versions; do not opportunistically upgrade incompatible packages.
- Prefer reproducible paths, deterministic seeds where relevant, explicit QC thresholds, clear plots with units, and lightweight validation/sanity checks.
- Before claiming code is correct, run suitable static checks or small tests. If Allen data or the environment is inaccessible, state precisely what was not executed. Do not invent outcomes.
- Never commit private Slack messages, nonpublic lab information, personal paths/secrets, or unpublished findings to a public repository without explicit review.

## Collaboration and Git safety
- This repository may be public. Treat added files as public unless checked otherwise.
- For substantive notebook refactors, work on a feature branch or separate commit, preserve a reviewable diff, and do not force-push.
- Never modify the Liz-owned repository, delete notebooks, rewrite history, or replace a validated pipeline without explicit approval.
- Explain proposed methodological choices and scientific limitations to Keanu rather than silently implementing them.
- Report: files touched, what changed, why, checks actually run, open scientific questions, and any download/runtime limitations.

## Preferred execution pattern
1. State how the coding task serves the larger research question.
2. Inventory relevant files and identify current behavior, pitfalls, and uncertainty.
3. Propose the smallest safe change and implement it.
4. Validate without unnecessary data downloads.
5. Summarize the change and give Keanu the next conceptual step.
