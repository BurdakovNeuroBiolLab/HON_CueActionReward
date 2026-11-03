# HON encoding of cue, action, and reward in behaving mice

Data and analysis code accompanying:

**Disentangling cue, action, and reward co-variables within rapid neural dynamics of hypocretin/orexin neurons in behaving mice**

Eva Bracey, Aditi Aravind, Nikola Grujic, Daria Peleg-Raibstein, Denis Burdakov <br>
Laboratory of Neurobehavioural Dynamics, ETH Zürich

Using two-photon and fiber-photometry imaging of hypocretin/orexin neurons (HONs) in mice performing a Pavlovian food-cue task, we applied a multivariate encoding model to ask what fraction of HON activity is explained by body movement, food probability/uncertainty, and food delivery, separately for the cue and outcome periods.

## Contents

- **`Sample_2P.ipynb`** — Sample implementation of the multivariate encoding model on two-photon HON recordings from one example session (see [Encoding pipeline](#encoding-pipeline) below).
- **`Figure_Data_and_Stats/`** — Source data tables (`.csv`/`.xlsx`) and summary statistics (`Stats_*.xlsx`) underlying the main and supplementary figures (Figs. 2–4, S1–S3), organized by figure/panel label (e.g. `Table_4D.csv`, `Stats_4D.xlsx`).

## Encoding pipeline

`Sample_2P.ipynb` reproduces, on one example session, the multivariate encoding analysis used throughout the paper to ask how much of each HON's activity is explained by behavior vs. trial/reward variables. For a chosen window (cue or outcome, modeled separately), it:

1. **Pre-processes behavioral/trial features** from the binarized behavior matrix (`ProcesBehaviour`), extracting running, licking, movement, food probability, and food uncertainty, aligned to the window of interest and downsampled to match the 2-photon volume rate.
2. **Pre-processes the 2-photon traces** (`Process2P`), pulling out the somatic (`F`) and neuropil (`Fneu`) fluorescence segments for the same trial windows from the Suite2p output.
3. **Applies feature transforms**: a cubic spline basis to uncertainty/probability/running/movement/licking, polynomial (degree-2) terms to running/movement/licking, and leaves the remaining variables (acceleration, velocity, reward) untransformed, via `create_Transformer`.
4. **Fits the encoding model** (`RidgeStim`): a ridge regression per neuron, with neuropil activity and the first PCA component across the session's other neurons included as additional inputs, evaluated with 5-fold cross-validation. Each feature's relative contribution is computed by the drop-one-feature method — the fractional drop in cross-validated r² when that feature is removed from the full model.
5. **Plots relative contribution scores** per feature across neurons (`plot_scores`), the model output shown in Figs. 2–4.

The same five steps are run twice in the notebook — once for the cue period, once for the outcome period — matching the paper's use of separate models for the two windows.

## Requirements

Python 3 with `numpy`, `pandas`, `scipy`, `scikit-learn`, `matplotlib`, `seaborn`, `h5py`.

## Note on raw data

`Sample_2P.ipynb` expects a session's binarized behavior matrix (`binarisedMat.mat`) and Suite2p outputs (`F.npy`, `Fneu.npy`) as described in the Methods; these raw recordings are not included here due to size and are not required to inspect the analysis pipeline itself.

Raw two-photon imaging and fiber photometry data are available upon reasonable request. Contact **XX** to request the raw data.
