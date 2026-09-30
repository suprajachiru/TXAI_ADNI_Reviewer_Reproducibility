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
