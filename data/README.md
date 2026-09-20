# Data specification and provenance

## Input files

| Local path | Required columns | Recorded count after cleaning |
| --- | --- | ---: |
| `data/raw/fluorescence.train.csv` | `seq`, `fluorescence` | 21,446 |
| `data/raw/fluorescence.test.csv` | `seq`, `fluorescence` | 27,217 |

`seq` contains an amino-acid sequence. `fluorescence` is a continuous numeric target. The original code uses `dropna().drop_duplicates()` on each whole table before splitting. Thus missing values in extra columns also affect filtering, and duplicate removal is based on whole rows, not sequences alone.

The cleaned training file is split with `test_size=0.1, random_state=42`: 19,301 fitting records and 2,145 validation records. The supplied test file remains separate. The saved log reports no training or validation sequences removed by the length filter at the 512- or 1024-token stages.

| Target statistic | Training file | Test file |
| --- | ---: | ---: |
| Mean | 3.1806 | 2.0825 |
| Standard deviation | 0.8340 | 0.9742 |

These are recorded statistics, not values recomputed from attached CSVs. The raw CSV files were not supplied with this revision. The notebook does not establish physical units, target transformation, assay details or a mutation-distance split; do not label the target as raw or log intensity without verifying those details.

## Model source is not the same as data provenance

The assignment explicitly directs students to use pretrained ProteinBERT and adapt its signal-peptide demo for fluorescence regression. The [official project](https://github.com/nadavbra/protein_bert) links to a [benchmark distribution](https://github.com/nadavbra/proteinbert_data_files/tree/master/protein_benchmarks). This is a relevant upstream source to consult, **not a verified byte-for-byte identification of the two local CSV files**.

The supplied assignment and notebook do not document the exact CSV download, version, conversion procedure or underlying experimental publication. The filenames alone are insufficient to claim a verified TAPE/Sarkisyan dataset lineage. Use the original authorized CSVs to reproduce the coursework, and document their upstream provenance once confirmed.

Place those two CSVs under `data/raw/`; they are excluded from Git. No fabricated sample data or unverified substitute download is included. Within-file sequence redundancy and train/test sequence overlap were not checked by the original code.
