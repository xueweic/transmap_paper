# Simulation Studies

Two simulation studies benchmarked the distinct tasks addressed by TransMap. The local study evaluated discovery of a *trans*-QTL for a specified gene program. The global study evaluated recovery of the variant-gene pairs underlying a directed, multilayer regulatory network. The notebooks contain plot-ready results rather than the full simulation runs.

## Local program study

Programs varied in size, the fraction of affected genes, residual expression correlation, genetic effect magnitude and direction, and sample size. TransMap-local was compared with TransPCO, a first-PC eigengene and individual-gene *trans*-eQTL testing. [Figure 1d](../Main_Figures/Figure_1/Figure_1d.ipynb) contains the matched 72-setting comparison with TransPCO. A separately labeled [Figure 1c MOCK layout preview](../Main_Figures/Figure_1/Figure_1c_MOCK.ipynb) uses synthetic bars and is **not an analysis result**; a genuine Figure 1c is not yet published here.

## Global program study

A directed network linked 40 *trans*-active variants to nine distal genes through proximal mediators and successive gene layers; 20 additional variants had no *trans* effect. Recovery was evaluated against 160 true variant-gene pairs. [Figure 1e](../Main_Figures/Figure_1/Figure_1e.ipynb) shows the network and truth matrix, and [Figure 1f](../Main_Figures/Figure_1/Figure_1f.ipynb) shows precision-recall curves for TransMap-global and ARCHIE across three effect levels. The corresponding RDS files contain only data needed to draw these panels.

## References

[1] [transmap](https://github.com/xueweic/transmap): R package implements TransMap for multi-scale *trans*-QTL mapping. See details in our [tutorial website](https://xueweic.github.io/transmap/).
