# DeepMEL_tutorial
DeepMEL is a deep learning tool for DNA sequence classification, combining CNN and BiLSTM to predict biological topics from genomic sequences. It uses a dual-input architecture (forward and reverse-complement sequences) for multi-label classification across 24 topics. This project is intended for teaching how to operate DeepMEL.


Here is the English version of the README document based on your DeepMEL project.

---


## Project Overview

DeepMEL is a multi-label classification model based on deep learning (Keras + TensorFlow 1.14) designed for classifying DNA sequences into topics. The project includes three main modules: model training, performance evaluation, and interpretability analysis (DeepExplainer). The data used in this documentation is randomly generated.

---

## Environment Setup

It is recommended to use Conda to create isolated environments for CPU and GPU as follows:

### CPU Environment
```bash
conda create --name DeepMEL_conda_env_cpu python=3.6 tensorflow=1.14.0 keras=2.2.4
conda activate DeepMEL_conda_env_cpu
conda install numpy=1.16.2 matplotlib=3.1.1 ipykernel=5.1.2
```

### GPU Environment
```bash
conda create --name DeepMEL_conda_env_gpu python=3.6 tensorflow-gpu=1.14.0 keras-gpu=2.2.4
conda activate DeepMEL_conda_env_gpu
conda install numpy=1.16.2 matplotlib=3.1.1 ipykernel=5.1.2
```

---

## File Descriptions

| File Name | Description |
|-----------|-------------|
| `training.ipynb` | Model training, data generation, and training curve plotting |
| `performance_analysis.ipynb` | Model performance evaluation (AUROC, AUPR) and prediction analysis |
| `scoring_and_deepexplainer.ipynb` | Model prediction, DeepExplainer visualization, and single-sequence interpretation |

---

## Data Format

The model accepts input in FASTA format. The sequence ID format is as follows:

```
>chr1:71000-71500_1
ATCG...
```

Where:
- `chr1:71000-71500`: Genomic location information
- `_1`: Indicates the sequence belongs to topic 1 (total of 24 topics)

Sequence length is **500 bp**, encoded using one-hot encoding (A, C, G, T).

---

## Model Architecture Overview

- Input: Bidirectional sequences (forward + reverse)
- Network layers:
  - Conv1D (128 filters, kernel size=20)
  - MaxPooling1D
  - Dropout(0.2)
  - TimeDistributed(Dense)
  - Bidirectional(LSTM)
  - Flatten
  - Dense(256) + Dropout(0.4)
  - Dense(24) + Sigmoid
- Output: Probability distribution over 24 topics

The model architecture diagram is saved as `results/model.png`.

---

## Training Workflow (training.ipynb)

1. **Generate random data** (example)
   - Generate training set (excluding chr2 and chr11)
   - Generate validation set (chr11)
   - Generate test set (chr2)

2. **Read data and convert to one-hot encoding**

3. **Build the model** (`build_model()`)

4. **Train the model**
   - Epochs: 5 (example)
   - Batch size: 128
   - Uses EarlyStopping and ModelCheckpoint

5. **Plot training curves** (accuracy + loss)

---

## Performance Evaluation (performance_analysis.ipynb)

- Compute AUROC and AUPR
- Plot performance curves for each topic
- Check model output distribution and prediction stability

---

## Interpretability Analysis (scoring_and_deepexplainer.ipynb)

- Load the trained model
- Use DeepExplainer to interpret a single sequence
- Visualize SHAP contributions for forward/reverse sequences
- Support in silico mutagenesis analysis (per-base mutation impact)

---

## Results Saving

All training results, model weights, and images are saved in the `results/` directory:

| File Name | Description |
|-----------|-------------|
| `model.json` | Model architecture |
| `model_best_loss.hdf5` | Model weights with lowest validation loss |
| `model_best_acc.hdf5` | Model weights with highest validation accuracy |
| `model_end.hdf5` | Model weights from the final epoch |
| `accuracy.png` / `loss.png` | Training curves |
| `training_curves.png` | Training curves (Chinese version) |

---
