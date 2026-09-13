# IAppels — Field Experiment Data & Statistical Analysis

This repository contains the evaluation dataset and the complete statistical analysis notebook used to produce the results reported in the paper:

> **AI-Assisted Emergency Call Processing: Integrating LLMs and Decision Trees in Real-World Field Experiments**
> IAppels project — DTNUM (Ministère de l'Intérieur), SDIS de l'Ain, Université Marie et Louis Pasteur.

It is provided for **reproducibility**: every figure, table, and statistical test reported in the manuscript's field-experiment analysis (Results Analysis section) can be regenerated from the data and code included here.

## Repository contents

```
.
├── evaluations.csv                          # Post-call evaluation dataset (157 calls)
├── OperatorD_sensitivity_analysis.ipynb      # Full statistical analysis notebook
├── requirements.txt                          # Python dependencies
├── .gitignore
└── README.md
```

## Dataset — `evaluations.csv`

Each row corresponds to one emergency call processed with IAppels assistance, evaluated by an operator through a post-call questionnaire. Personally identifiable and free-text content (call transcription, operator observations) has been removed from this public version; only structured evaluation fields are included.

| Column | Description |
|---|---|
| `Heure de fin` | Timestamp marking the end of the call |
| `Fidélité` | Transcription quality score (1–5 Likert scale) |
| `Note résumé` | Summary quality score (1–5 Likert scale) |
| `Note entité` | Named Entity Recognition (NER) quality score (1–5 Likert scale) |
| `Urgence correcte` | Whether the urgency prediction was correct (`oui` / `non`) |
| `Catégorie correcte` | Whether the emergency category prediction was correct (`oui` / `non`) |
| `Entités supplémentaires` | Additional named entities identified by the operator |
| `Resume` | Model-generated call summary |
| `Categorie` | Predicted emergency category |
| `Categorie corrigée` | Operator-corrected emergency category, when the prediction was incorrect |
| `Operateur` | Operator identifier (only reliably recorded for a 15-call subset — see paper, Section 4.3, and notebook) |

**Missing data:** several evaluation fields were optional in the version of IAppels used during this experiment, so not every call has a complete set of responses. Each statistical test in the notebook explicitly reports the exact number of valid (non-missing) observations it is based on, rather than assuming the full dataset size (N=157).

## Analysis notebook — `OperatorD_sensitivity_analysis.ipynb`

The notebook reproduces, in order:
1. Descriptive statistics — urgency and emergency-category prediction accuracy, and mean transcription/summary/NER scores, together with the number of valid responses used for each.
2. Correlation analysis — Pearson correlation between transcription quality and downstream (summary, NER) quality, with the correlation heatmap.
3. Mann–Whitney U tests — comparing transcription/summary/NER scores between calls with correct vs. incorrect urgency predictions, with exact sample sizes reported per test.
4. Operator-wise evaluation table — scores and prediction accuracy broken down by operator (reproducing Table 2 of the manuscript).
5. Kruskal–Wallis tests — assessing inter-operator variability in transcription/summary/NER scores.
6. Sensitivity analysis — all of the above metrics recomputed **with** and **without** Operator D's data, since a follow-up interview identified this operator as an outlier (see paper, Section 4.3).

## Getting started

```bash
git clone <this-repo-url>
cd <this-repo>
pip install -r requirements.txt
jupyter notebook OperatorD_sensitivity_analysis.ipynb
```

Running the notebook end-to-end regenerates all figures locally into a `figures/` folder (ignored by version control — see `.gitignore`) and prints every statistic reported in the manuscript.

