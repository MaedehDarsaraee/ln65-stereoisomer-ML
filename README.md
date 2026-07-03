# Machine‑Learning Guided Exploration of Antimicrobial Peptide Stereoisomers

This repository contains everything needed to reproduce the machine‑learning analysis of
the membrane‑disruptive undecapeptide ln65 (KKLLKLLKLLL), in which the antibacterial
activity and hemolysis of its 2048 possible L/D stereoisomers are predicted from an
11‑bit binary fingerprint of the stereochemistry.


## Overview

Each ln65 stereoisomer is encoded as an 11‑bit binary fingerprint (1 = L, 0 = D); the
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
├── data/        # experimental datasets
├── models/      # trained MLP models 
├── output/      # model predictions 
├── notebooks/   
├── requirements.txt
└── README.md
```

### `data/`
| File | Description |
|------|-------------|
| `synthesized.csv` | initial training set used to train the first round model and generate the prospective candidates |
| `synthesized_full.csv` | experimentally tested stereoisomers |
| `augmented.csv` |  augmented set: measured + missing mirror image enantiomers |

### `models/`
| File | Trained on | Used for |
|------|-----------|----------|
| `initial_model_mlp_act.joblib` / `_hem.joblib` | initial rounds | prospective predictions |
| `full_model_mlp_act.joblib` / `_hem.joblib` | all experimentally synthesized and tested stereoisomers | full set analysis |
| `augmented_model_mlp_act.joblib` / `_hem.joblib` | mirror‑augmented | 2048 design space prediction |


## Installation

```bash
git clone https://github.com/MaedehDarsaraee/ln65-stereoisomer-ML.git
cd ln65-stereoisomer-ML
python -m venv venv && source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

Built with Python 3.9, scikit‑learn 1.4.1, XGBoost 2.1.4, SHAP.


## Citation


## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

## Contact

Maedeh Darsaraee [maedeh.darsaraee@unibe.ch]
Reymond group, University of Bern.
