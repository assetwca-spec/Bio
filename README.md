Project 02 - Predicting Antibiotic Resistance from Genome Sequence

Real-data analysis pipeline for Mycobacterium tuberculosis antibiotic resistance prediction using the public CRyPTIC compendium.

Overview

This notebook builds and evaluates per-drug machine-learning models that predict resistance to isoniazid (INH) and rifampicin (RIF) from genomic information.

It uses exclusively public data from the CRyPTIC Consortium and follows the requirements of Project 02 (labelled dataset assembly, join-loss reporting, 
knowledge-driven baseline, per-drug classifiers, balanced accuracy + Very Major Error analysis, and feature interpretation).

Data sources

| Resource | Description | Licence |
| [CRyPTIC Consortium Dataset (Zenodo 15679886)](https://zenodo.org/records/15679886) | GENOMES.parquet, UKMYC_PHENOTYPES.parquet and supporting tables | CC-BY 4.0 |
| H37Rv reference coordinates | Classic resistance loci (katG, rpoB, etc.) | Public knowledge |

Pipeline summary

1. Download CRyPTIC tables (or load from local `cryptic_data/`).
2. Join genomic records to phenotype records on `UNIQUEID` and report the percentage of lost records.
3. Restrict to INH and RIF and keep only clear S/R labels.
4. Convert the real `WGS_PREDICTION_STRING` (catalogue-style genotypic calls) into binary genomic features.
5. Build a knowledge-driven baseline by selecting the string positions that best match the phenotype.
6. Train Random Forest classifiers (one per drug) with class weighting that penalises missing resistance.
7. Evaluate with 5-fold stratified cross-validation:
   - Balanced accuracy
   - Very Major Error rate (Resistant -> Susceptible)
   - Major Error rate (Susceptible -> Resistant)
8. Rank feature importances and compare ML performance with the catalogue baseline.

Results

| Drug | Balanced accuracy | Very Major Error | Catalogue baseline |
| INH  | ~94.5 %           | ~6.5 %           | ~94.1 %            |
| RIF  | ~93.8 %           | ~5.1 %           | ~93.6 %            |

The models recover the high-confidence genotypic rules already encoded in the CRyPTIC prediction string.

How to run

```bash
# 1. Install dependencies
pip install pandas numpy scikit-learn pyarrow matplotlib seaborn joblib

# 2. Open the notebook
jupyter notebook Untitled46.ipynb
# or
jupyter lab
