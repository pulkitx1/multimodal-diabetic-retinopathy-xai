
# Multi-Modal Diabetic Retinopathy Diagnosis using XAI

A multimodal deep learning research project that predicts diabetic retinopathy (DR) severity by combining retinal fundus images with clinical and demographic data. The project investigates whether feature-level fusion improves five-class severity prediction compared with image-only and clinical-only models.

## Overview

Diabetic retinopathy is a diabetes-related eye complication that can damage the retina and cause vision loss. Automated severity assessment using deep learning may support research into more efficient retinal screening.

This project uses the **mBRSET (Mobile Brazilian Retinal Dataset)** to classify diabetic retinopathy severity into five ordered ICDR classes, from DR0 to DR4.

Unlike image-only approaches, this project combines visual retinal features with structured patient information to investigate the benefits of multimodal learning.

> **Note:** This is an academic research prototype, not a clinically validated diagnostic system.

## Key Features

- **Multimodal learning:** Combines retinal fundus image embeddings and clinical features.
- **Deep learning:** Uses pretrained ConvNeXt-V2 Large for retinal image modelling and feature extraction.
- **Clinical modelling:** Uses a tuned, class-weighted Random Forest baseline.
- **Feature fusion:** Compares concatenation fusion and gated fusion.
- **Five-class classification:** Predicts ICDR severity classes DR0, DR1, DR2, DR3, and DR4.
- **Class imbalance handling:** Uses class-weighted learning and focal loss where appropriate.
- **Patient-wise evaluation:** Uses a 70:15:15 train-validation-test split to reduce patient-level data leakage.
- **Model comparison:** Evaluates clinical-only, image-only, and multimodal approaches.

## Dataset

The project uses the **mBRSET dataset**, containing retinal fundus images and associated clinical and demographic information.

The dataset contains five target classes:

| Class | Description |
|---|---|
| DR0 | No diabetic retinopathy |
| DR1 | Mild diabetic retinopathy |
| DR2 | Moderate diabetic retinopathy |
| DR3 | Severe diabetic retinopathy |
| DR4 | Proliferative diabetic retinopathy |

The clinical branch processes 17 original attributes into a 39-dimensional feature vector using imputation, missing-value indicators, and one-hot encoding.

Dataset access and usage must follow the terms established by the dataset provider. Do not upload restricted dataset images or patient-level records to this repository.

## Methodology

### 1. Clinical Data Branch

- Preprocess numerical and categorical variables.
- Handle missing values and encode categorical attributes.
- Generate 39-dimensional clinical feature vectors.
- Train a class-weighted Random Forest classifier.
- Tune hyperparameters using Optuna.
- Examine clinical feature importance.

### 2. Retinal Image Branch

- Preprocess fundus images and resize them to 384 × 384 pixels.
- Compare ResNet and ConvNeXt-V2 approaches.
- Use ConvNeXt-V2 Large for the final image modelling pipeline.
- Extract 1,536-dimensional image embeddings for multimodal experiments.
- Apply class-weighted focal loss during image model training.

### 3. Multimodal Feature Fusion

The project evaluates two fusion strategies:

- **Concatenation fusion:** Combines image and clinical representations into a single feature vector.
- **Gated fusion:** Learns feature-wise gates to adjust the contribution of the combined representations.

Twelve experimental configurations are evaluated using different image embedding dimensions, clinical representation dimensions, and fusion methods.

### 4. Evaluation

Models are evaluated using:

- Accuracy
- Macro-F1 score
- Balanced accuracy
- Quadratic Weighted Kappa (QWK)
- Confusion matrices and per-class classification reports

QWK is particularly relevant because the five DR severity labels are ordered.

## Results

The following results were obtained on the patient-wise held-out test set.

| Model | Accuracy | Macro-F1 | Balanced Accuracy | QWK |
|---|---:|---:|---:|---:|
| Clinical-only (Random Forest) | 67.81% | 0.3485 | 0.3563 | 0.3828 |
| Image-only (ConvNeXt-V2) | 80.00% | 0.5406 | 0.5979 | 0.7749 |
| Multimodal (Experiment 7) | **82.33%** | **0.5833** | **0.6369** | **0.7819** |

### Best Multimodal Configuration

Experiment 7 used:

- Image embedding dimension: 256
- Clinical feature dimension: 39
- Fusion method: Concatenation

Compared with the image-only baseline, this configuration improved accuracy, Macro-F1, balanced accuracy, and QWK on the reported test split.

**Important limitation:** The dataset is imbalanced, and the results are based on one dataset and one patient-wise split. Further validation is needed to assess generalisation to other populations and imaging devices.

## Explainable AI (XAI)

Interpretability is an important direction for this project.

Planned extensions include:

- **SHAP:** Explain the contribution of clinical variables to model predictions.
- **Grad-CAM:** Highlight retinal image regions that influence predictions.
- **Multimodal explanation:** Investigate how clinical and image features contribute jointly to predictions.

These explainability methods should be labelled as implemented only after their outputs have been generated and validated.

## Technology Stack

- Python
- PyTorch
- torchvision
- Hugging Face Transformers
- ConvNeXt-V2 Large
- scikit-learn
- Optuna
- pandas
- NumPy
- Matplotlib

## Project Workflow

1. Prepare the mBRSET dataset and match images with clinical records.
2. Create patient-wise training, validation, and test splits.
3. Preprocess clinical variables and train the Random Forest baseline.
4. Train the image model and extract retinal image embeddings.
5. Train and compare multimodal fusion configurations.
6. Evaluate all approaches using classification metrics.
7. Investigate explainability methods for clinical and image predictions.

## Repository Structure

The following is a suggested structure; adapt it to match the files actually present in your repository.

```text
multimodal-diabetic-retinopathy-xai/
├── data/
│   └── README.md
├── notebooks/
├── src/
│   ├── data_preprocessing.py
│   ├── clinical_model.py
│   ├── image_model.py
│   ├── feature_extraction.py
│   ├── fusion_model.py
│   └── evaluation.py
├── results/
│   └── README.md
├── requirements.txt
├── .gitignore
└── README.md
```

## Reproducibility

The experiments use a patient-wise 70:15:15 split with random seed 42. The clinical, image, and fusion approaches share the same saved patient assignments for fair comparison within the reported experimental pipeline.

Exact reproduction may require the original dataset, trained checkpoints, preprocessing artefacts, and experiment configurations.

## Limitations and Future Work

- Class imbalance, especially in rare DR severity grades.
- Evaluation on a single dataset and split.
- No external validation across independent datasets or imaging devices.
- Further development and validation of SHAP and Grad-CAM explanations.
- Investigation of more robust multimodal fusion strategies.

## Contributors

- **Pulkit Meena** — B.Tech, Information Technology, IIIT Bhopal
- **Anurag Soni** — B.Tech, Information Technology, IIIT Bhopal

## Project Supervisor

**Dr. Vishakha Chourasia**  
Assistant Professor, Department of Information Technology  
Indian Institute of Information Technology, Bhopal

## Disclaimer

This repository describes an academic research project. Predictions are experimental and must not be used as a substitute for professional ophthalmic assessment or clinical diagnosis.

## License

Add a license only after confirming that your code and any included materials may be distributed under its terms. Dataset licensing and access conditions remain separate.
