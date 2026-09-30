# TXAI ADNI Reviewer Reproducibility Package

## Trustworthy Explainable Artificial Intelligence (TXAI) for Comprehensive Alzheimer's Disease Diagnosis, Staging, and Progression Prediction Using Multimodal Data

This repository provides the reviewer-facing reproducibility materials for the TXAI framework described in the manuscript.

The repository is organized to distinguish clearly between:

1. the **manuscript-reported ADNI results**;
2. the **manuscript-aligned ADNI implementation and configuration**; and
3. the earlier **OASIS reference execution**, which is included only as an openly accessible execution example and is **not** claimed to reproduce the manuscript's ADNI results.

---

## 1. Manuscript-Reported ADNI Results

The manuscript reports the following diagnostic classification results for the proposed TXAI framework:

| Metric | Proposed TXAI Framework |
|---|---:|
| Accuracy | **98.21%** |
| Precision | **98.74%** |
| Recall | **98.58%** |
| F1-Score | **98.66%** |
| AUC | **99.63%** |

These values are the **results reported in the manuscript**. They are not represented in this repository as newly reproduced results unless corresponding ADNI execution artifacts have been generated from the authorized ADNI data.

The manuscript evaluates the TXAI framework using patient-independent five-fold cross-validation.

---

## 2. Manuscript Experimental Configuration

The manuscript specifies the following principal training configuration:

| Parameter | Manuscript Configuration |
|---|---|
| Dataset | ADNI |
| Cross-validation | Patient-independent 5-fold |
| Optimizer | Adam |
| Initial learning rate | 0.0001 |
| Batch size | 16 |
| Maximum epochs | 100 |
| Early stopping patience | 15 epochs |
| Weight decay | 1e-4 |
| Dropout | 0.50 |
| Attention heads | 8 |
| Feature embedding dimension | 512 |
| Initialization | Xavier Uniform |
| Random seed | 42 |
| Learning-rate scheduler | ReduceLROnPlateau |

The manuscript describes the multimodal input configuration as:

- MRI
- PET
- Clinical data
- Biomarker data
- Demographic data

The reported full multimodal configuration therefore uses all five input categories.

---

## 3. TXAI Architecture Described in the Manuscript

The manuscript describes the proposed architecture using the following principal components:

- 3D ResNet-50 MRI encoder
- 3D DenseNet-121 PET encoder
- Clinical MLP encoder: 256 → 128
- Biomarker MLP encoder: 128 → 64
- Demographic MLP encoder: 64 → 32
- Multi-head Cross-Modal Self-Attention with 8 heads
- Shared prediction layers
- Diagnosis prediction head
- Disease-staging prediction head
- Progression prediction head
- Personalized-risk prediction head

The manuscript reports a total of approximately **46.85 million trainable parameters** for the complete TXAI framework.

The source implementation includes component-wise parameter-count inspection so that the parameter total can be checked against the executable implementation.

---

## 4. Repository Contents

```text
TXAI_ADNI_Reviewer_Reproducibility/
│
├── README.md
├── requirements.txt
│
├── configs/
│   └── manuscript_adni_config.json
│
├── data/
│   └── adni_manifest_template.csv
│
├── src/
│   └── TXAI_Reproducibility_Implementation.py
│
├── adni_outputs/
│   ├── README.md
│   └── STATUS.json
│
└── reference_oasis_outputs/
    └── ...
```

### Source implementation

`src/TXAI_Reproducibility_Implementation.py`

The source contains the ADNI execution path, configuration, patient-level manifest validation, five-fold cross-validation logic, model training, evaluation, checkpoint generation, trustworthiness analysis, and explainability-output generation.

### Configuration

`configs/manuscript_adni_config.json`

This file records the manuscript-oriented ADNI configuration used by the reviewer package.

### Manifest template

`data/adni_manifest_template.csv`

The manifest specifies the patient-level organization expected by the implementation.

The repository does **not** contain ADNI participant-level imaging or clinical data.

---

## 5. ADNI Data Access

Official ADNI source:

https://adni.loni.usc.edu

ADNI participant-level data are access-controlled and are not redistributed through this repository.

An authorized ADNI user must obtain the required data through the appropriate ADNI/LONI Image and Data Archive access procedure and prepare the patient-level manifest required by the implementation.

The repository therefore does not attempt to download ADNI participant data automatically from the public ADNI homepage.

---

## 6. ADNI Manifest

The ADNI execution requires a patient-level manifest.

The implementation expects the following principal fields:

```text
patient_id
session_id
diagnosis
stage
mri_path
pet_path
clinical_json
biomarker_json
demographic_json
```

Additional longitudinal/progression fields may be supplied where available.

The implementation validates the manifest before starting the ADNI experiment and checks for duplicate patient identifiers to support patient-level separation.

---

## 7. Running the Manuscript-Aligned ADNI Experiment

After obtaining authorized ADNI data and preparing the manifest, set the manifest location.

Example:

```python
import os

os.environ["TXAI_DATASET"] = "ADNI"
os.environ["TXAI_MANIFEST"] = "/content/adni_data/manifest.csv"
```

Then run:

```bash
python src/TXAI_Reproducibility_Implementation.py
```

The ADNI path is configured for:

```text
Dataset       : ADNI
Cross-validation: 5 folds
Batch size    : 16
Maximum epochs: 100
Patience      : 15
Learning rate : 0.0001
Weight decay  : 0.0001
Attention heads: 8
Random seed   : 42
```

---

## 8. Expected ADNI Output Artifacts

A completed authorized ADNI execution should generate the following reviewer-relevant artifacts:

```text
adni_outputs/
│
├── experiment_config.json
├── fold_metrics.csv
├── pooled_metrics.json
├── oof_predictions.csv
├── pooled_roc.png
│
├── txai_fold1.pt
├── txai_fold2.pt
├── txai_fold3.pt
├── txai_fold4.pt
├── txai_fold5.pt
│
├── trustworthy_fold1.json
├── trustworthy_fold2.json
├── trustworthy_fold3.json
├── trustworthy_fold4.json
├── trustworthy_fold5.json
│
├── xai_fold1.json
├── xai_fold2.json
├── xai_fold3.json
├── xai_fold4.json
└── xai_fold5.json
```

These files should be added to `adni_outputs/` **only after they have been generated from the authorized ADNI experiment**.

---

## 9. Important Reproducibility Status

The repository intentionally distinguishes between **reported manuscript values** and **verified execution artifacts**.

### Currently provided

- Public GitHub repository
- ADNI-oriented source implementation
- Manuscript-aligned configuration
- Five-fold cross-validation configuration
- Batch size 16
- 100 maximum epochs
- Early stopping patience 15
- Random seed 42
- ADNI manifest template
- ADNI execution instructions
- Manuscript-reported diagnostic results
- Parameter-count inspection
- Reference OASIS execution materials

### Required for exact reproduction of the reported ADNI results

The following must be generated using the authorized ADNI dataset and corresponding patient-level manifest:

- five actual ADNI fold results;
- fold-level metrics;
- pooled out-of-fold predictions;
- pooled ROC;
- five trained TXAI checkpoints;
- trustworthiness artifacts;
- explainability artifacts; and
- the saved experiment configuration from the actual run.

Until those files are generated and verified, the repository should **not** claim that it has independently reproduced the manuscript's 98.21% ADNI result.

This distinction is intentional and prevents OASIS reference results from being incorrectly presented as ADNI evidence.

---

## 10. OASIS Reference Execution

The repository may contain:

```text
reference_oasis_outputs/
```

These files correspond to the earlier openly accessible OASIS reference execution.

They are provided only to demonstrate that the implementation can be executed with an openly accessible reference dataset.

The OASIS execution is **not** the manuscript's ADNI experiment and must not be interpreted as reproducing:

```text
98.21% accuracy
98.74% precision
98.58% recall
98.66% F1-score
99.63% AUC
```

The OASIS reference configuration differs from the manuscript configuration, including dataset, cohort, fold count, batch size, and training duration.

---

## 11. Reproducibility and Interpretation

The manuscript-reported numerical results and the executable repository should be interpreted together.

The repository provides the code and configuration needed to perform the ADNI experiment, while the restricted nature of the ADNI participant-level dataset prevents redistribution of the underlying patient data.

For an exact reproduction, the reviewer should use the same authorized ADNI data organization, patient-level manifest, preprocessing, modality availability, and execution configuration corresponding to the manuscript experiment.

No ADNI participant-level data are included in this repository.

---

## 12. Reviewer-Facing Summary

This repository addresses the reproducibility request by providing:

1. the TXAI source implementation;
2. an explicit ADNI execution path;
3. manuscript-aligned five-fold cross-validation;
4. manuscript-aligned batch size and training duration;
5. the ADNI manifest template;
6. the complete experiment configuration;
7. the manuscript-reported 98.21% diagnostic result and associated metrics;
8. defined locations for fold metrics, predictions, checkpoints, trustworthiness results, and explainability artifacts; and
9. a clear separation between ADNI manuscript results and the earlier OASIS reference execution.

The repository does not fabricate ADNI execution artifacts. Once authorized ADNI execution is performed, the generated artifacts can be placed in `adni_outputs/` to provide direct computational evidence for the reported results.
