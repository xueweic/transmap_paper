# TransMap manuscript resources

Code, notebooks, and shareable inputs for reproducing the TransMap manuscript's
figures and analyses. This repository is being assembled; the figure pages are
placeholders until their corresponding analyses and inputs have been reviewed.
The method implementation belongs in the companion R package,
[`transmap`](https://github.com/xueweic/transmap).

## Repository layout

| Path | Purpose |
| --- | --- |
| `Main_Figures/Figure_1/` through `Figure_7/` | Notebooks and files for each main figure. |
| `Supplementary_Figures/` | Supplementary figure notebooks, grouped by figure. |
| `Simulation_Studies/` | Simulation design, execution, and result summaries. |
| `Data_Applications/` | Real-data analyses and downstream validation. |
| `Supplementary_Tables/`, `Supplementary_Data/` | Table and data-generation analyses. |
| `data/manifest.tsv` | Provenance and access information for distributable inputs. |
| `templates/` | Starting point for a figure notebook. |
| `manuscript-website/` | Jupyter Book configuration and table of contents. |

Add a notebook to its figure or analysis directory, use paths relative to the
repository root, and record the input files, software versions, random seeds,
and output panels in the notebook. Add each notebook to
`manuscript-website/_toc.yml` when it is ready to appear in the book. The book
build does **not** execute notebooks; stored outputs remain visible when present.

Only include `.rds` files that can be redistributed. Record each in
`data/manifest.tsv`, including its source, access terms, and checksum. Store
restricted donor-level data, credentials, and unreviewed inputs outside this
repository. Large files can instead be deposited in an appropriate repository
and referenced from the manifest; do not assume GitHub is suitable for every
dataset.

## Build the book locally

Jupyter Book 1 is used to retain the `_config.yml` and `_toc.yml` structure of
the ColocBoost paper repository.

```sh
python3 -m venv .venv
.venv/bin/python -m pip install -r manuscript-website/requirements.txt
.venv/bin/jupyter-book build . --config manuscript-website/_config.yml --toc manuscript-website/_toc.yml
```

The HTML output is written to `_build/html/`. GitHub Actions checks that the
book builds, but publishing is a separate, manually confirmed workflow. The
Pages source must also be configured in repository settings before that
workflow can deploy. **A GitHub Pages site may be public even if this repository
is private**, so review notebook outputs and linked files before publishing.
