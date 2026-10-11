# Spikes2Slope

Exploratory computational-neuroscience research in the Voytek Lab at UC San Diego on neuronal spiking, LFP spectral properties, and the limits of interpreting aperiodic slope as an E/I-related measure.

## Repository contents

- `sessionscheck.ipynb`: Allen Visual Coding Neuropixels session/region **metadata** coverage exploration.
- `spikeseda.ipynb`: initial exploration of Allen session, probes, channels, LFP, and optogenetic event metadata.
- `visual_coding_coverage.csv`: exported **unfiltered anatomical channel counts**, not confirmed usable LFP-channel coverage.
- `AGENTS.md`: instructions and scientific safeguards for Codex and other repository-aware coding agents.

## Working locally

Open the repository in VS Code with the existing `allen_env` Jupyter kernel, if installed. Inspect `AGENTS.md` before requesting substantial agent changes. Reuse the AllenSDK cache at `~/allen_neuropixels_data` where applicable; do not download large electrophysiology files just to inspect metadata.

The notebooks are **exploratory**, not a finalized analysis pipeline. Scientific requirements and priorities should be checked against the latest team discussions and meeting notes, which are maintained outside this repository.

Related team repository: [ekaplan7/spikes_to_slope](https://github.com/ekaplan7/spikes_to_slope). Do not assume the two repositories are automatically synchronized.
