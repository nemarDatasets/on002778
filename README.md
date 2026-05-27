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

BIDS validator: 4 errors + 1290 warnings → 0 errors + 968 warnings. Raw `.bdf` binary payloads unchanged.

### `participants.tsv`
- `gender` column: `f`/`m` → `F`/`M` for all 31 rows. Why: the paired `participants.json` declares `gender` with `Levels: {"F": "female", "M": "male"}` (uppercase enum), so the lowercase TSV cells did not match and were flagged as the wrong type.
- `hand` column: `r` → `R` for all 31 rows. Why: same reason — `participants.json` declares `hand.Levels` as uppercase `{"R","L","A"}`.
- No other column values changed.

### `participants.json`
- `MMSE` entry: removed the invalid `Levels` block. Why: the previous Levels keys were bin descriptors (`">24"`, `"19 - 23"`, `"10 - 18"`, `"<9"`), not literal cell values, so the validator interpreted them as an enum and rejected the actual integer cells (e.g. `30`) as not matching. Rewrote the entry as a numeric `Units: "points"` column and moved the bin information into the `Description` text. Closes the `MMSE` `TSV_VALUE_INCORRECT_TYPE` error.

### `dataset_description.json`
- Added `DatasetType: "raw"`. Why: without it, the validator treats the dataset under derivative-rules and emits spurious warnings.
- `GeneratedBy` is left as the source published it (absent). The original OpenNeuro dataset declares no generation tooling, and the NEMAR rehost only fixes validator issues — not generation — so nothing is added here.
- Bumped `BIDSVersion` `1.2.2` → `1.11.1`. Why: set to the BIDS version the validator checks against.

### `task-rest_eeg.json` (new, inheriting root sidecar)
- Created at the dataset root with one key: `TaskDescription`, a one-sentence paraphrase of this README (resting-state EEG; healthy controls a single session, Parkinson's patients two sessions, one on and one off dopaminergic medication). Why: closes the 46 `SIDECAR_KEY_RECOMMENDED:TaskDescription` warnings on the `_eeg.bdf` recordings via BIDS inheritance, in one place.

### `task-rest_beh.json`
- Added `TaskName: "rest"` and `TaskDescription` (resting state, no behavioral responses; the `_beh.tsv` files are placeholders kept only for BIDS-spec compatibility — this was already noted in the existing `trial` column description). Why: closes 92 `TaskName` and 46 `TaskDescription` warnings on the behavioral side via inheritance. The original `trial` column documentation is preserved unchanged.

### `task-rest_events.json` (new, inheriting root sidecar)
- Created at the dataset root, documenting four columns of every `_events.tsv`: `onset` (with `Units: "s"`), `duration` (with `Units: "s"`), `sample`, and `value`. Why: the per-recording `_events.tsv` files have `sample` and `value` columns that were not declared in any sidecar, which the validator flags as `TSV_ADDITIONAL_COLUMNS_UNDEFINED`. The `value` column carries the BioSemi `Status`-channel trigger code. One root sidecar closes 92 warnings via inheritance.

### `sub-*/ses-*/eeg/sub-*_ses-*_task-rest_eeg.json` (46 per-recording sidecars)
- Renamed the key `MiscChannelCount` → `MISCChannelCount`. Why: the previous spelling is not BIDS-canonical (BIDS requires all-uppercase `MISC`), so the validator did not recognise the field and continued to warn that `MISCChannelCount` was missing. No value change — the field stays at `0`. Closes the 46 `SIDECAR_KEY_RECOMMENDED:MISCChannelCount` warnings.

### `sub-*/ses-*/sub-*_ses-*_scans.tsv`
- Left unchanged from the source. EEGDash's loader appends a `.000000` microsecond suffix to `acq_time` on read, but the source timestamps (e.g. `2011-01-19T11:22:56`, no fractional seconds) are already valid BIDS — fractional seconds are optional in the `date-time` type — so the suffix is an unnecessary loader-side normalization and is not baked into the published data.
