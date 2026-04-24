# mechano_vae

A personal learning project exploring variational autoencoders (VAEs) for biological data, with the long-term goal of building a latent space representation of cell mechanical phenotypes from AFM data.

## Structure

```
mechano_vae/
│
├── 01_scRNA_VAE/
│   └── 01_pbmc3k_vae.ipynb      # VAE on PBMC3k scRNA-seq data
│
└── 02_AFM_VAE/                   # coming next
    └── 02_afm_vae.ipynb
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

## 02 — VAE on AFM mechanical data (coming soon)

The next step is to apply the same approach to AFM compression-recovery data from HeLa cells under different pharmacological treatments (DMSO, Y27632, CytoD, IPA3, LatA, Blebbi). Each cell is represented as a trajectory of mechanical features over time — stiffness, indentation depth, compression depth — measured before, during, and after compression.

The goal is to learn a latent representation of mechanical phenotypes and ask whether the latent space organizes by treatment condition without supervision.

## Motivation

The single-cell genomics field has shown that self-supervised models trained on large biological datasets can learn meaningful latent representations of cell state. Mechanobiology lacks an equivalent — partly because of data fragmentation, non-standardized protocols, and the absence of a shared data infrastructure.

This project is a small step toward understanding what such a model might look like, starting from first principles.

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
