# Mechano_Vae

A personal learning project exploring variational autoencoders (VAEs) for biological data, with the long-term goal of building a latent space representation of cell mechanical phenotypes from AFM data.

## Structure

```
mechano_vae/
│
├── 01_scRNA_VAE/
│   └── scVAE.ipynb                  # VAE on PBMC3k scRNA-seq data (learning scaffold)
│
└── 02_AFM_VAE/
    ├── mechanoVAE.ipynb             # VAE on per-measurement AFM data (snapshot approach)
    └── TrajectorVAE.ipynb           # Conditional VAE on per-cell trajectories (trajectory approach)
```

## 01 — VAE on scRNA-seq data (PBMC3k)

As a first step I trained a simple VAE from scratch on the PBMC3k dataset — 2700 human blood cells with 32738 genes. This is a well-known benchmark dataset in single-cell biology, which made it a good starting point for learning.

The notebook covers:
- Data loading and preprocessing with scanpy
- VAE architecture in PyTorch (encoder, reparameterization trick, decoder)
- Training loop with reconstruction loss + KL divergence
- Latent space extraction and UMAP visualization
- Comparison between VAE latent space and raw data UMAP

The model was trained for 500 epochs on an Apple Silicon Mac (MPS backend). The resulting latent space separates major immune cell types — T cells, B cells, monocytes — without ever seeing the cell type labels during training.

**Result:**

| Epochs | Structure |
|--------|-----------|
| 10     | Major lineages separated, subtypes mixed |
| 300    | CD4/CD8 T cells partially resolved |
| 500    | Clear separation of all major cell types |

## 02 — VAE on AFM mechanical data

The next step is to apply the same approach to AFM compression-recovery data from HeLa cells under different pharmacological treatments (DMSO, Y27632, CytoD, IPA3, LatA, Blebbi). Each cell is represented as a trajectory of mechanical features over time — stiffness, indentation depth, compression depth — measured before, during, and after compression.

The goal is to learn a latent representation of mechanical phenotypes and ask whether the latent space organizes by treatment condition without supervision.

## 02 — VAE on AFM mechanical data — trajectory approach
Each row is one cell, represented as its full compression-recovery trajectory. This is the more biologically meaningful representation.
Feature vector per cell:
[E_5s/E_0, E_30s/E_0, E_60s/E_0,
 ΔH_0/E_0, ΔH_5s/E_0, ΔH_30s/E_0, ΔH_60s/E_0]
Normalization by E_0 forces the model to learn relative dynamics rather than absolute stiffness. E_0 itself is dropped as it becomes 1.0 for every cell.

Condition vector:

[batch one-hot (20 unique file+replicate combinations),
 Force of compression (standardized),
 Contact time (standardized)]


## Requirements

```
python >= 3.9
torch
scanpy
umap-learn
pandas
numpy
matplotlib
```

## Notes

This is a learning project, not a polished tool. The code prioritizes clarity over efficiency. Comments are intentionally verbose because I wrote this while learning.
