# TXAI ADNI Reviewer Reproducibility Package

## Trustworthy Explainable Artificial Intelligence (TXAI) for Comprehensive Alzheimer's Disease Diagnosis, Staging, and Progression Prediction Using Multimodal Data

This repository provides the reviewer-facing reproducibility resources for the TXAI framework described in the manuscript.

The repository clearly distinguishes between:

1. the **manuscript-reported ADNI results**;
2. the **manuscript-aligned ADNI implementation and configuration**; and
3. the **OASIS reference execution**, which is provided only as an openly accessible execution example and is **not claimed to reproduce the manuscript's ADNI results**.

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

These values are the diagnostic results reported in the manuscript under the established patient-independent cross-validation protocol.

**Important:** The values above are explicitly identified as **manuscript-reported results**. They are not presented as newly reproduced results unless corresponding ADNI execution artifacts have been generated using the authorized ADNI data.

ADNI is the primary development, training, and internal evaluation dataset described in the manuscript.

---

## 2. Manuscript Experimental Configuration

The manuscript specifies the following principal configuration:

| Parameter | Manuscript Configuration |
|---|---|
| Primary Dataset | ADNI |
| Validation Strategy | Patient-independent Stratified 5-Fold Cross-Validation |
| Training | 70% |
| Validation | 10% |
| Testing | 20% |
| Optimizer | Adam |
| Initial Learning Rate | 0.0001 |
| Learning-Rate Scheduler | ReduceLROnPlateau |
| Batch Size | 16 |
| Maximum Epochs | 100 |
| Early Stopping Patience | 15 epochs |
| Weight Decay | 1 × 10⁻⁴ |
| Dropout | 0.50 |
| Attention Heads | 8 |
| Feature Embedding Dimension | 512 |
| Weight Initialization | Xavier Uniform |
| Data Normalization | Z-score Normalization |
| Random Seed | 42 |
| GPU | NVIDIA RTX 4090 |
| GPU Memory | 24 GB VRAM |
| CPU | Intel Core i9 |
| RAM | 64 GB |
| Python | 3.11 |
| PyTorch | 2.x |

The manuscript specifies an image resolution of **224 × 224 pixels**. The exact third-dimensional volumetric depth/resampling configuration should follow the original preprocessing implementation and is not inferred here as 224 × 224 × 224.

---

## 3. Multimodal Input Configuration

The manuscript describes the TXAI framework as a multimodal system using:

- **MRI** — structural neuroimaging
- **PET** — functional/metabolic neuroimaging
- **Clinical/Cognitive data** — including MMSE, CDR, and ADAS-Cog
- **Biomarker data** — including CSF/blood biomarkers
- **Demographic data** — including age, sex, education, and related patient information

The complete primary ADNI experiment uses the five-modality configuration described in the manuscript.

Patient-level longitudinal records are kept within the same evaluation fold to prevent patient overlap and longitudinal information leakage.

---

## 4. TXAI Architecture

The principal architecture described in the manuscript consists of:

- **3D ResNet-50** MRI encoder
- **3D DenseNet-121** PET encoder
- Clinical MLP encoder: **256 → 128**
- Biomarker MLP encoder: **128 → 64**
- Demographic MLP encoder: **64 → 32**
- **Multi-head Cross-Modal Self-Attention (MCSA)** with 8 heads
- Shared prediction layers
- Diagnosis prediction head
- Disease-staging prediction head
- Disease-progression prediction head
- Personalized-risk prediction head

The manuscript reports approximately **46.85 million trainable parameters** for the complete TXAI framework.

The source implementation includes component-wise parameter-count inspection so that the trainable-parameter total can be checked against the executable implementation.

---

## 5. Repository Structure

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

### Source Implementation

`src/TXAI_Reproducibility_Implementation.py`

The source contains the ADNI execution path, configuration handling, patient-level manifest validation, five-fold cross-validation logic, model training, evaluation, checkpoint generation, trustworthiness analysis, and explainability-output generation.

### Configuration

`configs/manuscript_adni_config.json`

This file records the manuscript-oriented ADNI experimental configuration.

### Manifest Template

`data/adni_manifest_template.csv`

This file specifies the patient-level organization expected by the implementation.

### ADNI Outputs

`adni_outputs/`

This directory is reserved for artifacts generated from the actual authorized ADNI execution.

### OASIS Reference Outputs

`reference_oasis_outputs/`

This directory contains the earlier openly accessible OASIS reference execution, if included.

---

## 6. ADNI Data Access

Official ADNI source:

**https://adni.loni.usc.edu**

ADNI participant-level data are access-controlled and are **not redistributed through this repository**.

An authorized ADNI user must obtain the required data through the appropriate ADNI/LONI Image and Data Archive access procedure and prepare the patient-level manifest required by the implementation.

The repository does not attempt to automatically download participant-level ADNI data from the public ADNI homepage.

No ADNI participant-level imaging, clinical, biomarker, or demographic data are included in this repository.

---

## 7. ADNI Manifest

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

Additional longitudinal and progression-related fields may be supplied where available.

The implementation validates the manifest before starting the ADNI experiment and checks patient identifiers to support patient-level separation.

All records belonging to the same patient must remain within the same cross-validation fold.

---

## 8. Running the Manuscript-Aligned ADNI Experiment

After obtaining authorized ADNI data, prepare the patient-level manifest according to:

```text
data/adni_manifest_template.csv
```

Set the dataset and manifest location.

Example:

```python
import os

os.environ["TXAI_DATASET"] = "ADNI"
os.environ["TXAI_MANIFEST"] = "/content/adni_data/manifest.csv"
```

Then execute:

```bash
python src/TXAI_Reproducibility_Implementation.py
```

The manuscript-aligned ADNI configuration is:

```text
Dataset               : ADNI
Cross-validation      : 5 folds
Training              : 70%
Validation            : 10%
Testing               : 20%
Batch size            : 16
Maximum epochs        : 100
Early stopping        : 15 epochs
Learning rate         : 0.0001
Weight decay          : 0.0001
Attention heads       : 8
Embedding dimension   : 512
Normalization         : Z-score
Random seed            : 42
```

---

## 9. Expected ADNI Execution Artifacts

A completed authorized ADNI execution should generate reviewer-relevant artifacts such as:

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

These files should be placed in `adni_outputs/` **only after they have been generated from the authorized ADNI experiment**.

The repository does not label placeholder files as reproduced ADNI results.

---

## 10. Reproducibility Status

The repository intentionally distinguishes between **manuscript-reported numerical results** and **verified execution artifacts**.

### Currently provided

- Public GitHub repository
- TXAI source implementation
- Explicit ADNI execution path
- Manuscript-aligned ADNI configuration
- Patient-independent five-fold cross-validation
- 70% training / 10% validation / 20% testing protocol
- Batch size 16
- Maximum 100 epochs
- Early stopping patience 15
- Learning rate 0.0001
- Weight decay 1 × 10⁻⁴
- Eight attention heads
- Feature embedding dimension 512
- Random seed 42
- ADNI manifest template
- Experiment configuration
- Manuscript-reported diagnostic results
- Component-wise parameter-count inspection
- Reference OASIS execution materials

### Required for exact reproduction of the reported ADNI results

The following artifacts should be generated from the authorized ADNI dataset and corresponding patient-level manifest:

- five actual ADNI fold results;
- fold-level metrics;
- pooled out-of-fold predictions;
- pooled ROC;
- five trained TXAI checkpoints;
- trustworthiness artifacts;
- explainability artifacts; and
- the saved configuration from the actual execution.

Until these artifacts are generated and verified from the authorized ADNI data, the repository does **not** claim that it has independently reproduced the manuscript's 98.21% ADNI result.

This distinction is intentional and prevents the OASIS reference execution from being incorrectly presented as evidence for the manuscript's ADNI results.

---

## 11. OASIS Reference Execution

The repository may contain:

```text
reference_oasis_outputs/
```

These files correspond to the earlier openly accessible OASIS reference execution.

They are provided only to demonstrate execution of the implementation using an openly accessible reference dataset.

The OASIS execution is **not** the manuscript's primary ADNI experiment.

Therefore, the OASIS results must **not** be interpreted as reproducing the manuscript-reported:

```text
98.21% Accuracy
98.74% Precision
98.58% Recall
98.66% F1-Score
99.63% AUC
```

The OASIS reference configuration differs from the manuscript's ADNI configuration in dataset, cohort, available modalities, fold configuration, batch size, and training duration.

The OASIS reference outputs are therefore kept separately under:

```text
reference_oasis_outputs/
```

and are not placed under:

```text
adni_outputs/
```

---

## 12. Relationship Between ADNI, OASIS, and AIBL

The manuscript evaluates TXAI using three Alzheimer's disease datasets:

| Dataset | Role |
|---|---|
| **ADNI** | Primary development, training, and internal evaluation |
| **OASIS** | Independent external assessment/reference execution |
| **AIBL** | Independent external assessment |

The datasets do not contain identical modality combinations.

Accordingly, unavailable modalities are not artificially generated. External evaluation uses the modalities and diagnostic labels available in each dataset.

The complete five-modality configuration described in the manuscript corresponds to the primary ADNI multimodal experiment.

---

## 13. Explainability and Trustworthiness Outputs

The manuscript evaluates the TXAI framework using multiple explainability and trustworthiness components.

### Explainability

The framework includes:

- SHAP-based global feature explanation
- Grad-CAM visual interpretation for imaging
- Integrated Gradients for feature attribution
- Attention-based modality interpretation

### Trustworthiness

The framework includes:

- Predictive uncertainty estimation
- Prediction confidence
- Model calibration
- Reliability assessment
- Robustness evaluation

The implementation provides output locations for the corresponding explainability and trustworthiness artifacts generated during an authorized execution.

---

## 14. Parameter Verification

The manuscript reports approximately **46.850 million trainable parameters** for the complete TXAI framework.

The principal component-level values reported in the manuscript are:

| Component | Architecture | Parameters (Million) |
|---|---|---:|
| MRI Encoder | 3D ResNet-50 | 21.55 |
| PET Encoder | 3D DenseNet-121 | 22.10 |
| Clinical Encoder | MLP 256 → 128 | 0.05 |
| Biomarker Encoder | MLP 128 → 64 | 0.02 |
| Demographic Encoder | MLP 64 → 32 | 0.01 |
| Fusion Module | MCSA, 8 heads | 2.98 |
| Shared Prediction Layers | FC 512 → 256 | 0.131 |
| Diagnosis Head | FC + Softmax | 0.002 |
| Disease Staging Head | FC + Softmax | 0.002 |
| Progression Prediction Head | FC | 0.001 |
| Personalized Risk Head | FC | 0.004 |
| **Total TXAI Framework** | **Complete architecture** | **46.850** |

The source implementation provides component-wise parameter inspection to facilitate verification against the executable architecture.

---

## 15. Reproducibility and Interpretation

The manuscript-reported numerical results and the executable repository should be interpreted together.

The repository provides the manuscript-aligned implementation, configuration, manifest template, and execution procedure required to perform the ADNI experiment.

Because ADNI participant-level data are access-controlled, the underlying patient data cannot be redistributed through this repository.

Therefore, exact reproduction requires:

1. authorized access to the relevant ADNI data;
2. preparation of the corresponding patient-level manifest;
3. application of the manuscript-specified preprocessing procedures;
4. patient-independent stratified five-fold partitioning;
5. execution using the manuscript-aligned configuration; and
6. generation and inspection of the resulting fold-level and pooled artifacts.

No ADNI participant-level data are included in this repository.

---

## 16. Reviewer-Facing Reproducibility Summary

This repository provides the manuscript-aligned reproducibility resources requested for verification of the TXAI framework:

1. **TXAI source implementation** corresponding to the proposed architecture;
2. **explicit ADNI execution path**;
3. **patient-independent stratified five-fold cross-validation**;
4. **70% training / 10% validation / 20% testing protocol**;
5. **batch size 16 and maximum 100 epochs**;
6. **learning rate 0.0001 and early stopping patience 15**;
7. **ADNI patient-level manifest template**;
8. **manuscript-aligned experiment configuration**;
9. **manuscript-reported diagnostic performance of 98.21% accuracy, 98.74% precision, 98.58% recall, 98.66% F1-score, and 99.63% AUC**;
10. **component-wise parameter-count inspection**;
11. **defined locations for fold metrics, predictions, ROC results, checkpoints, trustworthiness results, and explainability outputs**; and
12. **clear separation between the manuscript's ADNI experiment and the earlier OASIS reference execution**.

The reported numerical values are explicitly identified as **manuscript-reported values**. They are not represented as independently reproduced ADNI results unless the corresponding execution artifacts have been generated from the authorized ADNI dataset.

For exact reproduction, reviewers with authorized ADNI access can prepare the required manifest and execute the supplied implementation using the manuscript-aligned configuration.

---

## 17. Important Note

This repository does **not** substitute an OASIS demonstration for the manuscript's ADNI experiment.

The OASIS execution is retained only as a reference execution example.

The manuscript's reported **98.21% ADNI accuracy** and associated metrics remain identified separately as the results reported in the manuscript until the corresponding ADNI execution artifacts are generated and verified.

This separation is maintained to ensure that results obtained from one dataset are not incorrectly represented as results obtained from another dataset.
