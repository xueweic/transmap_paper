# Supplementary Data

Numerical results underlying the figures are provided in the `data` folder of each figure.

- **AD_TransMap_colocalization.tsv**: TransMap colocalization results for AD GWAS (Bellenguez et al.) with ROSMAP *cis*-eQTL, *trans*-eGene-QTL, *trans*-eProgram-QTL and *trans*-biomarker QTL (protein, glycoprotein and metabolite). Each row is one AD-focal 95% colocalization confidence set (CoS) retained by ColocBoost `get_robust_colocalization(cos_npc_cutoff = 0.5, npc_outcome_cutoff = 0.2, pvalue_cutoff = 1e-4)`. These results underlie Figure 6. See [Figure 6d](../Main_Figures/Figure_6/Figure_6d.ipynb) for a walk-through of the table.

## Column Descriptions for AD_TransMap_colocalization.tsv

| Column Name | Type | Description |
|---|---|---|
| `focal_outcome` | character | Focal trait of the CoS (always `AD_Bellenguez_2026`). |
| `colocalized_outcomes` | character | Traits that colocalize within the CoS, separated by `; `. Names follow `{context}_{cis\|trans}_{gene\|local_program\|biomarker}_group{k}_{context}_{feature}`, where `feature` is an Ensembl gene ID, a *trans*-eProgram (`hotspot_module_*`) or a biomarker ID. |
| `cos_id` | character | ColocBoost CoS identifier within the analysis region (`cos{k}:y{i}_y{j}...`, listing the trait indices in that region). |
| `purity` | double | Minimum absolute LD correlation among variants in the CoS. |
| `top_variable` | character | Variant with the largest variant colocalization probability (VCP), `chr:pos:ref:alt` (GRCh38). |
| `top_variable_vcp` | double | VCP of `top_variable`. |
| `cos_npc` | double | Normalized probability that the CoS is a true colocalization. |
| `min_npc_outcome` | double | Minimum normalized probability across the colocalized traits. |
| `n_variables` | integer | Number of variants in the CoS. |
| `colocalized_index` | character | Indices of the CoS variants in the analysis region, separated by `; `. |
| `colocalized_variables` | character | CoS variants (`chr:pos:ref:alt`), separated by `; `. |
| `colocalized_variables_vcp` | double | VCP of each CoS variant, in the same order as `colocalized_variables`. |
| `region` | character | Analysis region (`chr:start-end`). Neighbouring regions overlap, so the same signal can appear in more than one row. |
| `minpv` | double | Minimum marginal p-value over the reported CoS variants for each trait, in the same order as `colocalized_outcomes`. A small number of values exceed 1e-4 (counted in the Figure 6d notebook). |
