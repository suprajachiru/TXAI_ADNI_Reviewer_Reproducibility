# TXAI Reproducibility Package — Manuscript-Aligned ADNI Runner

This reviewer package is configured for the **ADNI-centered experiment described in the manuscript**. It is intentionally separate from the earlier OASIS-1 reference run.

## Manuscript-aligned settings
- Dataset: ADNI (not redistributed)
- Task: CN vs AD
- Patient-level stratified 5-fold cross-validation
- Random seed: 42
- Optimizer: Adam
- Learning rate: 0.0001
- Batch size: 16
- Maximum epochs: 100
- Early-stopping patience: 15 epochs
- Weight decay: 1e-4
- Dropout: 0.50
- Attention heads: 8
- Training-split Z-score normalization
- Modalities: MRI + PET + clinical + biomarker + demographic

These settings match the manuscript's reported experimental configuration.

## Official ADNI data source
The ADNI dataset used by this package must be obtained from the official ADNI/LONI Image and Data Archive (IDA):

**ADNI:** https://adni.loni.usc.edu

ADNI data access is controlled by the ADNI Data Use Agreement and approved researcher access. The dataset is therefore not downloaded automatically by this repository and is not redistributed here.

The official ADNI site provides MRI, PET, clinical, and biomarker data through the secure LONI IDA.

## IMPORTANT — ADNI data and exact original run
ADNI data are not included in this repository. The reviewer package therefore requires the **same patient-level ADNI manifest and source data used for the reported experiment**. The manifest must contain the paths and labels used in that experiment.

This package does **not** fabricate the manuscript's 98.21% result. After the original ADNI data/manifest are supplied and the experiment is run, the generated artifacts should be placed in `outputs/` and checked against the manuscript before claiming exact reproduction.

## ADNI manifest
Use `data/adni_manifest_template.csv` as the column template. Required columns:

`patient_id, session_id, diagnosis, stage, mri_path, pet_path, clinical_json, biomarker_json, demographic_json`

Optional progression columns are also supported. `diagnosis` must use 0/1 for the CN-vs-AD task.

## Run
After obtaining authorized ADNI access, download/prepare the same ADNI data used for the manuscript experiment and create the patient-level manifest. Then set the environment variables in Colab/Linux:

```bash
export TXAI_DATASET=ADNI
export TXAI_MANIFEST=/content/adni_data/manifest.csv
export TXAI_IMAGE_SIZE=96,96,96
python src/TXAI_Reproducibility_Implementation.py
```

**Replace `TXAI_IMAGE_SIZE` with the exact 3D input size used in the original ADNI experiment.** The supplied manuscript states 224 x 224 for image preprocessing but does not specify the third dimension of the 3D tensor.

## Generated artifacts
The run writes:
- `experiment_config.json`
- `fold_metrics.csv`
- `pooled_metrics.json`
- `oof_predictions.csv`
- `pooled_roc.png`
- `txai_fold1.pt` ... `txai_fold5.pt`
- `trustworthy_fold*.json`
- `xai_fold*.json`

## Reviewer note
The earlier public repository contained an OASIS-1 lightweight reference run. This package changes the default to ADNI and the manuscript-aligned five-fold/100-epoch/batch-16 configuration. The code does not claim to reproduce the manuscript results until the original ADNI data/manifest and the corresponding execution are supplied and verified.

Do not upload ADNI participant data or restricted data to GitHub.


## Manuscript-Reported Results

The following values are the results reported in the manuscript for the
proposed TXAI framework. They are included here for reference to the
published manuscript claims and are **not represented as results reproduced
by the current public package unless corresponding ADNI execution artifacts
are present**.

| Metric | Proposed TXAI Framework |
|---|---:|
| Accuracy | **98.21%** |
| Precision | **98.74%** |
| Recall | **98.58%** |
| F1-score | **98.66%** |
| AUC | **99.63%** |

These values should be interpreted as the manuscript-reported results.
The repository provides the implementation and manuscript-aligned
configuration needed to perform the ADNI experiment. Exact reproduction
requires the authorized ADNI data and the corresponding patient-level
manifest. ADNI participant-level data are not redistributed in this
repository.

### Computational-Efficiency Values Reported in the Manuscript

| Model | Training Time | Inference Time / Sample | Memory Usage | Trainable Parameters |
|---|---:|---:|---:|---:|
| Multi-Scale Multimodal Deep Learning Framework | 121.3 min | 11.5 ms | 5.48 GB | 42.70 M |
| Proposed TXAI Framework | 128.6 min | 13.1 ms | 5.92 GB | 46.85 M |

### TXAI Parameter Count Reported in the Manuscript

The manuscript reports **46.85 million trainable parameters** for the
complete TXAI framework. The repository source also includes a
component-wise parameter-count function so that the trainable parameter
count can be inspected from the implementation.

> **Reproducibility clarification:** The numerical values above are
> manuscript-reported values. They should not be interpreted as newly
> reproduced ADNI results until the authorized ADNI experiment has been
> executed and its output artifacts have been generated and checked.
