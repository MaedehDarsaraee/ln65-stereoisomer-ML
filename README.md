# Machine‑Learning Guided Exploration of Antimicrobial Peptide Stereoisomer

This repository contains everything needed to reproduce the machine‑learning analysis of
the membrane‑disruptive undecapeptide **ln65 (KKLLKLLKLLL)**, in which the antibacterial
activity and hemolysis of its 2048 possible L/D stereoisomers are predicted from an
11‑bit binary fingerprint of the stereochemistry.


## Overview

Each ln65 stereoisomer is encoded as an **11‑bit binary fingerprint** (1 = L, 0 = D); the
amino‑acid sequence is identical across all stereoisomers and is not encoded. Two binary
classification tasks are learned:

- **Antibacterial activity**  active if MIC ≤ 8 µg/mL against all five tested strains
  (*E. coli*, *P. aeruginosa*, *A. baumannii*, *K. pneumoniae*, MRSA), otherwise inactive.
- **Hemolysis**  hemolytic if MHC ≤ 125 µg/mL on human red blood cells, otherwise non‑hemolytic.

Multilayer‑perceptron (MLP), SVM, random‑forest, and XGBoost classifiers are compared by
10‑fold cross‑validation; the MLP is used for design‑space prediction over all 2048
stereoisomers and for SHAP‑based explainability. Candidate sequences for the iterative
synthesis rounds were proposed by random selection, a fine‑tuned language model, and a
nearest‑neighbor (point‑mutation) approach.

## Repository structure

```
.
├── data/        # experimental datasets and CD data
├── models/      # trained MLP models (joblib)
├── output/      # model predictions over the design space and candidate sets
├── notebooks/   # analysis notebooks (run in order)
├── requirements.txt
└── README.md
```

### `data/`
| File | Description |
|------|-------------|
| `synthesized_full-deduplicated.csv` | 321 experimentally tested stereoisomers (deduplicated), with `active` and `not hemolytic` labels |
| `augmented_deduplicated.csv` | 620‑stereoisomer augmented set (measured + missing mirror‑image enantiomers) |
| `synthesized_Hippo.csv` | 31 previously reported ln65 stereoisomers (CD analysis) |
| `cd_data_with_act_hem_labels.xlsx` | circular‑dichroism measurements with activity/hemolysis labels |

### `models/`
| File | Trained on | Used for |
|------|-----------|----------|
| `initial_model_mlp_act.joblib` / `_hem.joblib` | 265 (initial rounds) | forward‑validation predictions |
| `full_model_mlp_act.joblib` / `_hem.joblib` | 321 (all experimental) | full‑set analysis |
| `augmented_model_mlp_act.joblib` / `_hem.joblib` | 620 (mirror‑augmented) | 2048 design‑space prediction (Figure 4) |

(Activity models: class 1 = active. Hemolysis models: class 1 = non‑hemolytic.)

### `output/`
| File | Description |
|------|-------------|
| `ranked_all_2048.csv` | predictions for all 2048 possible stereoisomers |
| `active_not_hemolytic.csv`, `active_hemolytic.csv`, `inactive_hemolytic.csv`, `inactive_not_hemolytic.csv` | predicted category sets used for forward validation |
| `top20_untested_candidates.csv` | top‑ranked untested candidates |

### `notebooks/`
| Notebook | Produces |
|----------|----------|
| `deduplicated_data_analysis.ipynb` | dataset composition and category counts (Figure 2) |
| `mlp/svm/rf/xgboost_10fold_cv_final.ipynb` | classifier comparison by 10‑fold CV (Table 1) |
| `gpt_generation.ipynb` | model training and prediction over the 2048 design space |
| `full_model_analysis.ipynb` | full and augmented model training and evaluation |
| `02_model_analysis.ipynb` | prediction scatter and SHAP analysis (Figure 4) |
| `cd_scatter.ipynb` | circular‑dichroism analysis (Figure 5) |
| `LN65-tmap.ipynb` | design‑space map (TMAP) |

## Installation

```bash
git clone https://github.com/<user>/<repo>.git
cd <repo>
python -m venv venv && source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

Built with Python 3.9, scikit‑learn 1.4.1, XGBoost 2.1.4, SHAP.

## Reproducing the results

Run the notebooks in `notebooks/`. A fixed random seed (`random_state = 42`) is used
throughout. Cross‑validation of the augmented (620) model keeps mirror‑image pairs in the
same fold to avoid information leakage.


## Contact

maedeh.darsaraee@unibe.ch
Reymond group, University of Bern.
