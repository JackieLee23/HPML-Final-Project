# HPML Project:  U-Net Performance Optimization for Quantum Phase Reconstruction
## Team Information
- **Team Name**: U-Net Performance Optimization for Quantum Phase Reconstruction
- **Members**:
- Cameron Rodriguez (cjr2210)
- Jackson Lee (jcl2259)
- Fred Angelo Garcia (fbg2107)

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

<img width="756" height="378" alt="image" src="https://github.com/user-attachments/assets/441bf6ee-5687-4f0e-9b62-f915ee41c707" />


---
## 3. Dataset
Each datapoint consists of a tuple of 200x200 image (density), 2x200x200 image (phase gradients), 200x200 image (mask indicating valid regions of density)
Train/Validation/Testing Sizes: 1008/336/336


---
## 4. Final Results Summary

Weights and Biases link: https://wandb.ai/HPML-final-team/HPML%20Final%20Project?nw=nwuserjcl2259

Example of Adam results (50 epochs, lr = 1e-3, no weight decay):
<img width="1000" height="1000" alt="image" src="https://github.com/user-attachments/assets/51ab28f8-8c3e-468b-939e-d2bb3694bce7" />

Table produced after training with Adam optimizer (before pruning and quantization):
| Metric | Value |
|----------------------|-------------|
| Test Set Masked Mean Squared Error | 7.79625e-4 (ground truth values range between -pi and pi)|
| Inference Latency on single example| 8.83 ms |
| Model Size | 66.2 MB |
| GPU Memory Allocation| 10.76% |
| Peak GPU Utilization| 90.0% |
| Training Time/Epoch | 8.78 s |
| Device | NVIDIA T4 GPU |
--


### Pruning results

Example weight distribution histogram during pruning and retraining steps:

<img src="https://github.com/user-attachments/assets/a4f994e4-04e9-4c08-87bd-ae118f3fcf32" 
     alt="pruning_weight_distribution" 
     width="1000"/>

Example weight distribution animation of the iterative pruning and retraining technique:

![hpmlgifText](https://github.com/user-attachments/assets/661b85be-8814-4f11-875f-547f60e42b09)

Accuracy loss as a function of pruning ratio for each method employed (pruning, pruning + retraining, iterative pruning + retraining):

<img src="https://github.com/user-attachments/assets/445b3397-b1e9-4859-95cf-c67b8217d699" 
     alt="pruning_weight_distribution" 
     width="400"/>

Using the iterative pruning + retraining method, we can compress the model by 99%, while still retaining 95% accuracy. This is unexpected ! A recommendation going forward would to be to characterize the speedup of the resulting model using hardware that can take advantage of sparsity (since we used unstructured pruning). 

### quantization main results
| Method | File Size (MB) | Compression (×) | Error | Error Increase (%) | Mean Diff | Max Diff |
|--------|----------------|-----------------|-------|-------------------|-----------|----------|
| Per-Tensor | 16.647 | 3.97× | 0.002918 | +33.5% | 0.038339 | 0.933781 |
| Per-Channel | 16.670 | 3.97× | 0.002583 | +18.1% | 0.050083 | 1.079907 |
| Mixed Precision | 16.654 | 3.97× | 0.002209 | +1.0% | 0.009804 | 0.134167 |
| Original | 66.159 | 1.00× | 0.002186 | 0.0% | 0.000000 | 0.000000 |

**Summary:**
- **Mixed Precision** offers the best balance: 4× compression with only 1% accuracy loss
- **Per-Channel** provides similar compression but with 18% higher errors
- **Per-Tensor** has the highest error increase at 33% despite equal compression
- All quantized models achieve ~16.6 MB (4× smaller than 66.2 MB Original)

**Recommendation:** Use **Mixed Precision** quantization for optimal performance-compression tradeoff.

---

## 5. Reproducibility Instructions
Quickstart: Minimum Reproducible Result
To reproduce our minimum reported result, run:

here is a link to the data generated (need to be logged into lionmail):
https://drive.google.com/drive/folders/1gGsmoTqXq7Eu9O8knNRrxfiQtUfylpZl?usp=sharing    
run the following notebooks with the .npz files in the home directory

```bash
density_to_phase_grad.ipynb # For search of best optimizer
pruned_density_to_phase_grad.ipynb # For pruning results
quantized_density_to_phase_grad.ipynb # for quantized model results
optimizer_comparison_figs.ipynb  # for optimizer comparisons
```
---
## 6. Notes
- We choose to use Jupyter notebooks for easy access to google colab. Data generated using imaginary time evolution of the 2D Gross Pitaevskii Equation with rotation. 
