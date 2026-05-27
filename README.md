[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.on002778-blue)](https://doi.org/10.82901/nemar.on002778)

Welcome to the resting state EEG dataset collected at the University of San Diego and curated by Alex Rockhill at the University of Oregon.

Please email arockhil@uoregon.edu before submitting a manuscript to be published in a peer-reviewed journal using this data, we wish to ensure that the data to be analyzed and interpreted with scientific integrity so as not to mislead the public about findings that may have clinical relevance. The purpose of this is to be responsible stewards of the data without an "available upon reasonable request" clause that we feel doesn't fully represent the open-source, reproducible ethos. The data is freely available to download so we cannot stop your publication if we don't support your methods and interpretation of findings, however, in being good data stewards, we would like to offer suggestions in the pre-publication stage so as to reduce conflict in published scientific literature. As far as credit, there is precedent for receiving a mention in the acknowledgements section for reading and providing feedback on the paper or, for more involved consulting, being included as an author may be warranted. The purpose of asking for this is not to inflate our number of authorships; we take ethical considerations of the best way to handle intellectual property in the form of manuscripts very seriously, and, again, sharing is at the discretion of the author although we strongly recommend it. Please be ethical and considerate in your use of this data and all open-source data and be sure to credit authors by citing them.

An example of an analysis that we could consider problematic and would strongly advice to be corrected before submission to a publication would be using machine learning to classify Parkinson's patients from healthy controls using this dataset. This is because there are far too few patients for proper statistics. Parkinson's disease presents heterogeneously across patients, and, with a proper test-training split, there would be fewer than 8 patients in the testing set. Statistics on 8 or fewer patients for such a complicated diease would be inaccurate due to having too small of a sample size. Furthermore, if multiple machine learning algorithms were desired to be tested, a third split would be required to choose the best method, further lowering the number of patients in the testing set. We strongly advise against using any such approach because it would mislead patients and people who are interested in knowing if they have Parkinson's disease.

Note that UPDRS rating scales were collected by laboratory personnel who had completed online training and not a board-certified neurologist. Results should be interpreted accordingly, especially that analyses based largely on these ratings should be taken with the appropriate amount of uncertainty.

In addition to contacting the aforementioned email, please cite the following papers:

Nicko Jackson, Scott R. Cole, Bradley Voytek, Nicole C. Swann. Characteristics of Waveform Shape in Parkinson's Disease Detected with Scalp Electroencephalography. eNeuro 20 May 2019, 6 (3) ENEURO.0151-19.2019; DOI: 10.1523/ENEURO.0151-19.2019.

Swann NC, de Hemptinne C, Aron AR, Ostrem JL, Knight RT, Starr PA. Elevated synchrony in Parkinson disease detected with electroencephalography. Ann Neurol. 2015 Nov;78(5):742-50. doi: 10.1002/ana.24507. Epub 2015 Sep 2. PMID: 26290353; PMCID: PMC4623949.

George JS, Strunk J, Mak-McCully R, Houser M, Poizner H, Aron AR. Dopaminergic therapy in Parkinson's disease decreases cortical beta band coherence in the resting state and increases cortical beta band power during executive control. Neuroimage Clin. 2013 Aug 8;3:261-70. doi: 10.1016/j.nicl.2013.07.013. PMID: 24273711; PMCID: PMC3814961.

Appelhoff, S., Sanderson, M., Brooks, T., Vliet, M., Quentin, R., Holdgraf, C., Chaumon, M., Mikulan, E., Tavabi, K., Höchenberger, R., Welke, D., Brunner, C., Rockhill, A., Larson, E., Gramfort, A. and Jas, M. (2019). MNE-BIDS: Organizing electrophysiological data into the BIDS format and facilitating their analysis. Journal of Open Source Software 4: (1896).

Pernet, C. R., Appelhoff, S., Gorgolewski, K. J., Flandin, G.,
Phillips, C., Delorme, A., Oostenveld, R. (2019). EEG-BIDS, an extension to the brain imaging data structure for electroencephalography. Scientific Data, 6, 103. https://doi.org/10.1038/s41597-019-0104-8.

Note: see this discussion on the structure of the json files that is sufficient but not optimal and will hopefully be changed in future versions of BIDS: https://neurostars.org/t/behavior-metadata-without-tsv-event-data-related-to-a-neuroimaging-data/6768/25.

## NEMAR curation changes (2026-05-21, revised 2026-05-27)

The BIDS validator went from 4 errors + 1290 warnings to 0 errors + 968 warnings. None of the raw `.bdf` recordings were touched — every change is to a text sidecar.

**Participant table (`participants.tsv`)**
- The `gender` column held lowercase `f`/`m`, but the paired `participants.json` defines this column with uppercase categories (`F` = female, `M` = male). The 31 cells were capitalized to match so they read as valid values.
- The `hand` column held lowercase `r` for the same reason — `participants.json` defines handedness as uppercase `R`/`L`/`A` — so it was capitalized to `R` across all 31 rows.
- No other column values were changed.

**Participant descriptions (`participants.json`)**
- The `MMSE` entry listed score bins (`">24"`, `"19 - 23"`, `"10 - 18"`, `"<9"`) as if they were the allowed cell values, which made the validator reject the actual integer scores (e.g. `30`). It was rewritten as a plain numeric column measured in points, and the bin information was moved into the column's description text where it belongs.

**Dataset description (`dataset_description.json`)**
- Added `DatasetType: "raw"` so the dataset is validated as raw data rather than a derivative.
- Updated `BIDSVersion` from `1.2.2` to `1.11.1`, the version the current validator checks against.
- Left `GeneratedBy` absent, as the source published it. The original OpenNeuro dataset declares no generation tooling, and this rehost only fixes validator issues, so nothing was invented here.

**Task sidecar added at the dataset root (`task-rest_eeg.json`)**
- A new shared sidecar carrying a one-sentence task description paraphrased from this README (resting-state EEG; healthy controls have a single session, Parkinson's patients two — one on and one off dopaminergic medication). Placed at the root so it applies to every recording at once instead of being repeated in each file.

**Behavioral sidecar (`task-rest_beh.json`)**
- Added a task name (`rest`) and a task description noting this is resting state with no behavioral responses; the `_beh.tsv` files are placeholders kept only for BIDS compatibility, as the existing `trial` column note already explains. That original column documentation is preserved unchanged.

**Events sidecar added at the dataset root (`task-rest_events.json`)**
- A new shared sidecar describing the four columns in every events table: `onset` and `duration` (both in seconds), `sample`, and `value`. The `value` column carries the BioSemi `Status`-channel trigger code. These columns appeared in the data but were previously undocumented; one root sidecar documents them for all recordings.

**Recording sidecars (`_eeg.json`, all 46 recordings)**
- The misc-channel-count field was spelled `MiscChannelCount`; BIDS uses all-uppercase `MISCChannelCount`. It was renamed so the validator recognizes it. The value, `0`, was already correct and is unchanged.

**Acquisition times (`scans.tsv`) — left exactly as published**
- EEGDash's loader appends a `.000000` microsecond suffix to the acquisition times when it reads the files, but the published timestamps (e.g. `2011-01-19T11:22:56`) are already valid BIDS — fractional seconds are optional — so they were left unchanged rather than having the loader's suffix baked in.
