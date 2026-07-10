# GatedGCN-ADMET

A GatedGCN + LSPE multi-task model for the **OpenADMET – ExpansionRx Blind Challenge**,
predicting 9 ADMET endpoints from molecular structure (SMILES).

This repository contains the *base* GatedGCN model (`minzina-base-check` submission) and
the data / environment needed to reproduce it.

## Model

- **Backbone**: Gated Graph ConvNet with Learnable Structural Positional Encoding
  (`GatedGCNLSPELayer`), implemented in [DGL](https://www.dgl.ai/).
- **Head**: multi-task regression over 9 endpoints (`GatedGCN_MultiReg`).
- **Training**: K-Fold CV, MSE loss (log-space targets), early stopping,
  Optuna hyperparameter search.
- **Best base config**: `hidden_dim=256, dropout=0.1, num_layer=9, batch_size=128`.

### Endpoints

LogD · KSOL · HLM CLint · MLM CLint · Caco-2 Permeability (Papp A>B) ·
Caco-2 Permeability (Efflux) · MPPB · MBPB · MGMB

## Leaderboard results (recorded)

| Variant                       | num_layer | MA-RAE | Rank |
|-------------------------------|-----------|--------|------|
| GatedGCN base (this repo)     | 9         | 0.76   | 67   |
| GatedGCN + only-ECFP variant  | 10        | 0.69   | 31   |

Scored by Macro-Averaged Relative Absolute Error (MA-RAE) on the blinded test set.

## Data

- `data/9admet_train.csv` — SMILES + measured values for the 9 endpoints.
- `data/9admet_test.csv` — blinded test SMILES (predictions submitted to the challenge).

Data originates from the public
[OpenADMET-ExpansionRx challenge dataset](https://huggingface.co/datasets/openadmet/openadmet-expansionrx-challenge-train-data).

## Setup & run

```bash
conda env create -f environment.yaml
conda activate gatedgcn-admet
# Install the torch / dgl build matching your CUDA (see https://www.dgl.ai/pages/start.html)
jupyter lab   # then run baseline_gatedgcn.ipynb from the repo root
```

Run the notebook from the repository root so the `data/` relative paths resolve.
Checkpoints are written to `save/` (git-ignored).

## Challenge

- [OpenADMET – ExpansionRx Blind Challenge](https://openadmet.ghost.io/expansionrx-openadmet-blind-challenge/)
- Tutorial: https://github.com/OpenADMET/ExpansionRx-Challenge-Tutorial
