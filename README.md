[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.on003876-blue)](https://doi.org/10.82901/nemar.on003876)

Epilepsy Interictal Dataset
=====================

This dataset was updated and prepared for release as part of a manuscript by Bernabei & Li et al. (in preparation). A subset of the data has been featured in [1] and [2].

Summary
-------------

This dataset comprises of de-identified subjects with interictal iEEG recordings possibly with sleep or awake state annotated. The subjects come from the following centers:

- National Institute of Health (NIH)
- Johns Hopkins Hospital (JHH)
- University of Miami Florida Jackson Memorial Hospital (UMF)

In the actual study, there is also data from Kansas University Medical Center (KUMC), University of Pittsburgh Medical Center and Cleveland Clinic, whose data is not shared due to restrictions imposed by the centers there.

Some subjects, namely with the ``rns`` prefix in their subject ID were treated with RNS rather then surgical resection/ablation.

Derivatives
----------------

The processed data corresponding to the ``source-sink`` analysis and ``hfo`` comparisons are shown in the ``derivatives/`` folder. The HFO analysis consists of two folders, one is an RMS detector and the other is a Hilbert detector. See the paper for details.

Ties to Other Datasets
--------------------------------

NIH ``pt1, pt2, pt3``, JHH ``jh103, jh105`` subjects are also datasets in ``https://openneuro.org/datasets/ds003029``, where the ictal snapshots are stored. These correspond to the following:

- pt1: pt01
- pt2: pt2
- pt3: pt3
- jh103: jh103
- jh105: jh105

Moreover, the cclinic subjects are used in that study, but not open-access due to data sharing limitations at Cleveland Clinic. Those ictal datasets were analyzed in https://www.nature.com/articles/s41593-021-00901-w. 

References
----------------

[1] Li, A., Huynh, C., Fitzgerald, Z. et al. Neural fragility as an EEG marker of the seizure onset zone. Nat Neurosci 24, 1465â1474 (2021). https://doi.org/10.1038/s41593-021-00901-w

[2] Kristin M. Gunnarsdottir, Adam Li, Rachel J. Smith, Joon-Yi Kang, Nathan E. Crone, Anna Korzeniewska, Adam Rouse, Nathaniel Cameron, Iahn Cajigas, Sara Inati, Kareem A. Zaghloul, Varina L. Boerwinkle, Sarah Wyckoff, Nirav Barot, Jorge Gonzalez-Martinez, Sridevi V. Sarma. Source-sink connectivity: a novel resting-state EEG marker of the epileptogenic zone. bioRxiv 2021.10.15.464594; doi: https://doi.org/10.1101/2021.10.15.464594

[3] Appelhoff, S., Sanderson, M., Brooks, T., Vliet, M., Quentin, R., Holdgraf, C., Chaumon, M., Mikulan, E., Tavabi, K., HÃ¶chenberger, R., Welke, D., Brunner, C., Rockhill, A., Larson, E., Gramfort, A. and Jas, M. (2019). MNE-BIDS: Organizing electrophysiological data into the BIDS format and facilitating their analysis. Journal of Open Source Software 4: (1896). https://doi.org/10.21105/joss.01896

[4] Holdgraf, C., Appelhoff, S., Bickel, S., Bouchard, K., D'Ambrosio, S., David, O., â¦ Hermes, D. (2019). iEEG-BIDS, extending the Brain Imaging Data Structure specification to human intracranial electrophysiology. Scientific Data, 6, 102. https://doi.org/10.1038/s41597-019-0105-7

## Channel names follow the data headers (18 Oct 2026)

In 46 recordings the channel list of channels.tsv did not equal the data-file header (names, order or count), so `mne_bids.read_raw_bids` stopped with 'Channel mismatch' (or 'Duplicate channel names'); EEGDash renamed the data channels by position to the channels.tsv names, which gives wrong labels wherever the two lists are not aligned. Since 18 Oct 2026 every channels.tsv lists exactly the header channels, in header order, with the header names (MNE names: duplicated header labels numbered -0, -1).

Rows were matched to header channels BY NAME, never by position (exact; then case/whitespace; then the recorder prefix 'EEG '/'POL ', '_' and zero padding; then a trailing '-<reference>' token). Matched rows keep all their columns (type, units, status, ...); 4294 rows took the header name (the header labels carry 'POL ' prefix, 'EEG ' prefix + '-G2' suffix, 'EEG POL ' prefix, 'EEG POL ' prefix + '-Re' suffix).

16 header channels had no channels.tsv row (11 files) and got a new row: type from the name (ECG/EMG/EOG/trigger) or from the other contacts of the same shaft, else MISC; units from the header; status n/a. Examples: `sub-NIH10_ses-extraoperative_task-interictal_run-01_ieeg.edf`: EEG POL; `sub-NIH2_ses-extraoperative_task-interictal_run-01_ieeg.edf`: EEG POL-Ref; `sub-PY18N015_ses-extraoperative_task-interictal_run-01_ieeg.edf`: POL-0, POL-1.

16 channels.tsv rows had no channel in the data file (11 files) and were removed; they are kept verbatim under `sourcedata/channels-removed-2026-10-18/` (same relative path). Examples: `sub-NIH10_ses-extraoperative_task-interictal_run-01_ieeg.edf`: N/A; `sub-NIH2_ses-extraoperative_task-interictal_run-01_ieeg.edf`: N/A; `sub-PY18N015_ses-extraoperative_task-interictal_run-01_ieeg.edf`: -0, -1.

EEGDash's positional renaming had given wrong labels to 16 channels in 11 files (rows missing, extra or out of order before that position).

electrodes.tsv is unchanged: it names the contacts (e.g. `A1`), while channel names follow the recorder (e.g. `A1-AV`); to attach positions in MNE, map channel names to contact names first, e.g. `raw.rename_channels(lambda n: re.sub(r'-[A-Za-z0-9]+$', '', n))`.
