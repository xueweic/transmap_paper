# Local-program simulation

## Question and design

This study tested discovery of a *trans*-QTL for a prespecified module when the variant affects only a subset of its genes. It varied the fraction of affected genes, the coherence and direction of their effects, the residual correlation among genes, and the number of independent variants acting on the module. A separate null setting measured false-positive rejection. The Figure 1 RDS files contain plot-ready summaries, not simulated expression or genotype matrices.

## Generative model

For one simulated dataset, let $n$ be the number of donors, $M$ the number of genes in the module, $m$ the number of affected genes, and $Q\in\{1,2,3\}$ the number of independent tested variants. For variant $a$, its minor-allele frequency $f_a$ is sampled with replacement from an author-supplied frequency pool after filtering to $0.05<f_a<0.5$. Independently across variants and donors,

$$
G_{ia}\sim\operatorname{Binomial}(2,f_a),\qquad
C_{ia}=b_aG_{ia}+\eta_{ia},\qquad
\eta_{ia}\sim N(0,1),
$$

where $C_{ia}$ is a simulated proximal expression mediator. With $h_c=0.04$ and $v_a=2f_a(1-f_a)$, the coefficient

$$
b_a=\sqrt{\frac{h_c}{(1-h_c)v_a}}
$$

sets the population fraction of variance of $C_{ia}$ attributable to $G_{ia}$ to $h_c$. The $Q$ mediators have independent residuals. For module gene $j$,

$$
Y_{ij}=\sum_{a=1}^{Q}B_{ja}C_{ia}+\varepsilon_{ij},\qquad
\boldsymbol\varepsilon_i\sim N_M(\mathbf 0,R_\rho),\qquad
(R_\rho)_{jk}=\rho^{|j-k|}.
$$

Exactly $m$ genes are sampled as affected. Each is assigned to **one** of the $Q$ variants, with affected genes allocated as evenly as possible across variants; all other $B_{ja}$ are zero. A nonzero coefficient has magnitude

$$
\gamma=\sqrt{\frac{s(1-h_c)}{h_c}},\qquad B_{ja}\in\{-\gamma,+\gamma\},
$$

where $s$ is the target variance of the genotype-mediated contribution to an affected gene relative to the unit marginal variance of $\varepsilon_{ij}$. In the homogeneous setting, all nonzero coefficients are positive. In the heterogeneous setting, positive and negative signs are balanced as closely as the number of affected genes permits. The null sets $s=0$ and hence $B=0$.

This construction propagates both the genetic and residual parts of $C_{ia}$ into $Y_{ij}$. For an affected gene assigned to variant $a$, the **total** per-variant *trans* heritability is

$$
h^2_{\mathrm{trans}}=
\frac{\operatorname{Var}(B_{ja}b_aG_{ia})}{\operatorname{Var}(Y_{ij})}
=\frac{s}{1+s/h_c},
$$

not $s$ itself. Thus $s\in\{0.0025,0.004,0.01\}$ implies target $h^2_{\mathrm{trans}}\approx\{0.00235,0.00364,0.00800\}$ under the population model. The $\rho$ parameter controls background *cross-gene* correlation through the AR(1) residual covariance; it is not a correlation between variants.

## Simulated settings

The manuscript Methods describe a broader design with module sizes $M\in\{20,50,100\}$, donor sample sizes from 400 to 1,000, and affected-gene fractions from 0% (null) to 100%. The archived Figure 1 data provide the following directly traceable subset; they should not be read as evidence that every manuscript-wide setting was included in these panels.

| Factor | Values represented in the Figure 1 source | Interpretation |
| --- | --- | --- |
| Module size $M$ | 20 in the Figure 1c/1d plot-ready results | Number of distal expression phenotypes |
| Donors $n$ | 1,000 in the Figure 1c/1d plot-ready results | Independent donors per simulated dataset |
| Affected genes $m/M$ | 50% in Figure 1c; 25%, 50%, 75%, and 100% in Figure 1d | Fraction carrying a nonzero coefficient |
| Residual correlation $\rho$ | 0.1, 0.3, 0.5 | AR(1) dependence across genes |
| Signal parameter $s$ | 0.0025, 0.004, 0.01; 0 for the null | Genotype-mediated path variance relative to unit distal residual variance |
| Effect direction | Homogeneous positive or balanced positive/negative | Coherent or heterogeneous gene effects |
| Variants $Q$ | 1, 2, or 3, drawn per dataset | Independent module-associated variants |
| Replicates | 1,000 per plotted condition | Independent simulated datasets |

The number of variants follows empirical weights proportional to 13,665, 1,680, and 565 for $Q=1,2,3$, respectively (85.9%, 10.6%, and 3.6% after rounding). These weights were used as a simulation-design prior on multiplicity; they do not imply that the simulated genes reproduce an observed joint molecular architecture. The Figure 1c preview and Figure 1d RDS objects distributed here are restricted to $M=20$ and $n=1,000$.

## Methods and endpoints

All methods test each simulated variant against the same expression matrix. Individual-gene testing uses marginal ordinary least squares (OLS) *t* tests and Bonferroni correction across the $M$ genes. The eigengene comparator tests the first ordinary principal-component score. *TransMap-local* whitens module expression using the **known simulated** $R_\rho$, retains axes with whitened eigenvalue greater than one, tests each retained score by marginal OLS, and applies a Bonferroni correction over the retained axes. The TransPCO comparator uses its ACAT-based test on the same gene-level association statistics. Its covariance input varies by source run (known $R_\rho$ or empirical $\operatorname{cor}(Y)$); this should not be conflated with the fixed oracle background used for *TransMap-local* in these simulations.

For a method, variant $a$, replicate $r$, and threshold $\alpha$, define $I_{ra}=\mathbb{1}(p_{ra}<\alpha)$. The plotted rate is the mean within-dataset rejection fraction,

$$
\widehat R=\frac{1}{R}\sum_{r=1}^{R}\left(\frac{1}{Q_r}\sum_{a=1}^{Q_r}I_{ra}\right).
$$

This is power when the simulated variants are active and a per-tested-variant false-positive rate under the null. It is **not** the probability of detecting at least one of the $Q_r$ variants, nor a family-wise error rate. The Figure 1c/1d panels use $\alpha=0.05$; the source simulation also records results at $10^{-5}$, which should not be substituted for the plotted threshold. The Figure 1d [notebook](../Main_Figures/Figure_1/Figure_1d.ipynb) compares *TransMap-local* and TransPCO across 72 plotted settings. The separate [Figure 1c notebook](../Main_Figures/Figure_1/Figure_1c.ipynb) is a layout preview with synthetic values; it cannot be used to assess any method.

For $Q>1$, intervals in the underlying simulation summaries account for the fact that tested variants from the same dataset share a replicate. The plotted RDS files contain summarized rates rather than replicate-level outcomes, so those intervals cannot be independently recomputed from this repository alone.
