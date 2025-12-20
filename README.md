# HPML Project:  U-Net Performance Optimization for Quantum Phase Reconstruction
## Team Information
- **Team Name**: U-Net Performance Optimization for Quantum Phase Reconstruction
- **Members**:
- Cameron Rodriguez (cjr2210)
- Jackson Lee (fbg2107)
- Fred Angelo Garcia (UNI)

- ...
---
## 1. Problem Statement
How to optimize the performance of a U-Net model for reconstructing quantum phase information from intensity measurements, focusing on optimizing model size, inference latency, and accuracy for deployment on resource-constrained environments such as a lab.

We explore varios optimization techqniques
- Optimizer Comparison (Jackson)
- Pruning (Cameron)
- Quantization  Post Training (Fred)

---
## 2. Model Description
We employ a U-Net architecture tailored for image-to-image translation tasks, specifically designed to reconstruct phase information from intensity measurements in quantum systems. The U-Net consists of an encoder-decoder structure with skip connections that facilitate the preservation of spatial information during the reconstruction process.


---
## 3. Final Results Summary
Example Table:
| Metric | Value |
|----------------------|-------------|
| Final Top-1 Accuracy | XX.XX% |
| Inference Latency | XX.XX ms |
| Model Size | XX MB |
| Peak Memory Use | XX MB |
| Training Time/Epoch | XX s |
| Device | NVIDIA T4 GPU |
---

## 4. Reproducibility Instructions

### A. Requirements
Install dependencies:
```bash
pip install -r requirements.txt
```
---
B. Wandb Dashboard
View training and evaluation metrics here: Wandb Dashboard Link
(Replace with actual link)
---
### C. Specify for Training or For Inference or if Both
To train the model from scratch:
```bash
python train.py --config configs/default.yaml
```
---
### D. Evaluation
To evaluate the trained model:
```bash
python eval.py --weights checkpoints/best_model.pth
```
---
### E. Quickstart: Minimum Reproducible Result
To reproduce our minimum reported result, run:

here is a link to the data generated (need to be logged into lionmail):
https://drive.google.com/drive/folders/1gGsmoTqXq7Eu9O8knNRrxfiQtUfylpZl?usp=sharing    
run the following notebooks with the .npz files in the home directory

```bash
quantized_density_to_phase_grad.ipynb # for quantized model results
optimizer_comparison_figs.ipynb  # for optimizer
```
---
## 5. Notes (up to you)
- All scripts are located in `scripts/`, `train.py`, `eval.py`, and `configs/`.
- Trained Model are saved in `models/`.
- Cont
