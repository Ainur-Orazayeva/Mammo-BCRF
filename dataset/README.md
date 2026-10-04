# Dataset access and local inputs

The committed files in this directory describe the cohort and the frozen computational protocol. They contain no raw DICOM collection, ROI-mask collection, or replacement for provider access.

Obtain **VinDr-Mammo v1.0.0** from its [PhysioNet record](https://physionet.org/content/vindr-mammo/1.0.0/) after accepting its Data Use Agreement. Obtain **CBIS-DDSM Calc-Test** from the [TCIA collection](https://www.cancerimagingarchive.net/collection/cbis-ddsm/). Use full mammograms as model inputs; the external ROI series only supplies reference boxes.

The exact original source inputs were absent from the supplied macOS project copy. The release includes cohort counts, trained-model identity records, frozen predictions, and analysis results. It does **not** include the complete sample/acquisition/split manifests, image caches, or model weights. Do not reconstruct these files by guessing image IDs, reassigning studies, or substituting another model. A fresh cohort is a different experiment.

## Expected input layout for notebooks 01–04

```text
dataset/                         # Default WORK_DATA_ROOT and DATASET_ROOT
├── images_dicom/                 # Authorized local VinDr DICOM files
├── annotations/
│   ├── finding_annotations.csv   # Exact local frozen snapshot
│   ├── breast-level_annotations.csv
│   └── metadata.csv
├── SOURCE_SHA256SUMS.txt          # Original source checksum manifest
├── Raw/
│   ├── immutable_sample_manifest.csv
│   ├── immutable_acquisition_manifest.csv
│   └── CBIS-DDSM_Calc-Test/
│       ├── calc_case_description_test_set.csv
│       ├── immutable_external_series_manifest.csv
│       └── dicom/                # Full mammograms and reference ROI series
├── Splits/
│   └── split_assignments.csv
├── Processed/
│   ├── standard_dicom_1024_metadata.csv
│   ├── standard_dicom_1024_uint16.npy
│   ├── cbis_ddsm_calc_test_1024_metadata.csv
│   └── cbis_ddsm_calc_test_1024_uint16.npy
├── checkpoints/                  # Original BCRF and comparator artifacts
└── protocol/                     # Included operational settings and identities
```

The checkpoint/deployment metadata in `protocol/` identifies expected BCRF files for seeds 17, 29, and 43 and all five development variants. Notebook 04 also needs `cam_classifier_seed{seed}.pt` and `unet_pseudomask_seed{seed}.pt`. Their historical training source is not included. Preserve the exact original weights and caches when replaying frozen inference.

Existing WSL data may remain outside the Git checkout:

```bash
export MAMMO_DATA_ROOT=/home/azyqan/projects/Mammology/dataset
export MAMMO_WORK_DATA_ROOT=/home/azyqan/projects/Mammology/02_next_scientific_study/Data
export MAMMO_MODEL_ROOT=/home/azyqan/projects/Mammology/02_next_scientific_study/Models
export MAMMO_PYTHON=/home/azyqan/venvs/ml-torch/bin/python
```

Start Jupyter from the repository or its `notebooks/` directory. Path adapters preserve the original logical identities used by frozen input checks. The saved protocol has study-level seed 342; existing frozen files must remain unchanged. The source annotation snapshot differs from the source checksum manifest, as recorded in `protocol/source_annotation_hashes.csv`; obtain the exact snapshot when reproducing the historical results.

The large local input directories and model binaries are excluded by `.gitignore`. Notebook 05 requires only the committed result/feature CSVs and figures and is the appropriate starting point for readers without restricted data access. Full reproduction remains dependent on the missing original inputs.
