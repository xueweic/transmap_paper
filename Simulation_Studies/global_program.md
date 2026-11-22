# Global-program simulation

## Question and network

This study evaluated recovery of distal variant-gene pairs when genetic effects are distributed through a directed, multilayer regulatory network. Each simulated dataset contains $n=1,000$ donors, 60 variants, eight proximal expression mediators, and nine distal genes. The first 40 variants have paths to distal genes; the remaining 20 are null. The nine distal genes form three successive layers of sizes four, three, and two. The fixed network defines 160 true pairs among $60\times9=540$ candidate variant-gene pairs. [Figure 1e](../Main_Figures/Figure_1/Figure_1e.ipynb) displays this network and truth matrix.

Let $A_{GC}$ be the $60\times8$ variant-to-proximal-mediator adjacency matrix, $A_{CY}$ the $8\times9$ proximal-to-distal adjacency matrix, and $A_{YY}$ the $9\times9$ directed distal-to-distal adjacency matrix. Five active variants enter each proximal mediator, and only the first four distal genes receive direct proximal input. The path-count matrix used to define the simulated genetic effects is

$$
P=A_{GC}A_{CY}(I_9+A_{YY}+A_{YY}^{2}).
$$

Thus $P_{jg}>0$ identifies a true variant $j$-distal gene $g$ pair, and multiple directed routes can give $P_{jg}>1$. The equation includes direct, one-step, and two-step downstream propagation of the **genetic mean**; the implementation does not recursively propagate realized expression residuals through the distal layers.

## Generative model and calibration

For variant $j$, a minor-allele frequency $f_j$ is sampled with replacement from an author-supplied pool after filtering to $0.01<f_j<0.5$. Genotypes are independently drawn as $G_{ij}\sim\operatorname{Binomial}(2,f_j)$ and column-centered to form $X_{ij}=G_{ij}-\bar G_j$. Write $v_j$ for the sample variance of $X_{\cdot j}$, $q=v_1$, and

$$
b_j=\begin{cases}
\sqrt{q/v_j}, & j\leq40,\\
0, & j>40.
\end{cases}
$$

This scaling gives every active variant the same sample variance $b_j^2v_j=q$ for its weighted genotype. The simulated proximal and distal expression matrices are

$$
C=X\operatorname{diag}(b)A_{GC}+E_C,
\qquad
Y=X\operatorname{diag}(b)P+E_Y.
$$

The entries of $E_C$ and $E_Y$ are independent, zero-mean Gaussian residuals with gene-specific variances. In particular, $Y$ is generated from $X$ and $P$ directly, not by inserting the realized $C$ and its noise into a recursive expression equation. The network is therefore a model of shared *genetic* paths rather than a stochastic mediation model.

Residual variances are calibrated separately. For proximal mediator $c$, let $\mu_{C,c}$ denote column $c$ of $X\operatorname{diag}(b)A_{GC}$. With target per-active-variant *cis* heritability $h_c=0.04$, its residual variance is

$$
\sigma^2_{C,c}=q/h_c-\widehat{\operatorname{Var}}(\mu_{C,c}).
$$

For distal gene $g$, write $\mu_{Y,g}$ for column $g$ of $X\operatorname{diag}(b)P$ and $J_g=\{j:P_{jg}>0\}$. Define the average genetic variance contribution among its true variant-gene pairs as

$$
A_g=\frac{1}{|J_g|}\sum_{j\in J_g}b_j^2v_jP_{jg}^2,
\qquad
\sigma^2_{Y,g}=A_g/\phi-\widehat{\operatorname{Var}}(\mu_{Y,g}),
$$

where $\phi\in\{0.0025,0.004,0.01\}$ is the target **mean** per-causal-pair distal heritability within a gene. Under the calibrated variance, the target for a particular true pair is

$$
h^2_{jg}=\frac{b_j^2v_jP_{jg}^2}{A_g/\phi}
=\phi\,\frac{b_j^2v_jP_{jg}^2}{A_g}.
$$

Individual pair effects can therefore differ when path counts differ; $\phi$ is not an identical heritability assigned to every path or to a specific layer. The path matrices and coefficients remain fixed across the three signal settings; the distal residual variances change to achieve the target mean.

## Comparison and evaluation

The same simulated data generate marginal variant-gene association $Z$-scores for each method. The *TransMap-global* fit receives the $60\times9$ matrix $Z$ and $n$. The plotted ARCHIE comparator receives a genotype-variance-scaled cross-association matrix,

$$
(\widehat\Sigma_{GE})_{jg}=
\frac{Z_{jg}}{\sqrt{n\,2\widehat f_j(1-\widehat f_j)}},
\qquad \widehat f_j=\frac{1}{2n}\sum_iG_{ij}.
$$

Both plotted fits use the same underlying penalized matrix-decomposition implementation with its default tuning and a common-sparsity setting. Nine components are fitted, and the first four define the pair score

$$
S_{jg}=\left|\sum_{k=1}^{4}u_{jk}v_{gk}\right|.
$$

An additional historical ARCHIE-code implementation was run as a source comparator but is not displayed in Figure 1f. Fixed-sparsity and allele-frequency-band checks were run separately; they are not part of the Figure 1f RDS. The panel does not use an independently generated held-out cohort.

For each of 1,000 independently simulated datasets at each $\phi$, the 540 pair scores are ranked against the 160 true pairs. Tied scores enter together. Precision-recall curves start at $(\mathrm{recall},\mathrm{precision})=(0,1)$ and end with all pairs selected. The area under the precision-recall curve (AUPRC) is computed by trapezoidal integration **within each replicate**, then averaged across replicates. The [Figure 1f notebook](../Main_Figures/Figure_1/Figure_1f.ipynb) plots mean precision-recall curves and reports mean replicate-level AUPRC; the area under a displayed mean curve should not be substituted for that mean AUPRC. Its RDS contains only plot-ready summaries, not the 1,000 fitted datasets.
