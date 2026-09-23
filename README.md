# Bark BNF Methods — Data and Code

Data and code accompanying:

Gomez-Alvarez, P., Jeffrey, L., Cook, P., Dittmann, J., Erler, D., Carvalho, M., Wen, W., Johnston, S.G., Leung, P.M., Deschaseaux, E., Greening, C., Maher, D. "Nitrogen fixation in tree bark: methodological advances reveal an overlooked forest nitrogen source." Submitted to *New Phytologist*.

Raw 16S rRNA amplicon sequencing reads (Illumina, V4 region, 515F/806R primers) are deposited in the NCBI Sequence Read Archive under BioProject accession [PRJNA1533092].

## Contents
### `bnf_data/`

Raw nitrogen-fixation and experimental data (.csv) underlying the manuscript's ARA, 15N2 assimilation, and CH4-addition experiments.

- `ARA_data_publication.csv` — Ethylene concentration and BNF rates estimated by the Acetylene Reduction Assay (Air and stem gas treatments, 1/2/4-day incubations, plus sterilised controls). Underlies Figure 1 and Tables S2–S4.
- `15N_data.csv` — δ15N and BNF rates measured with the 15N2 assimilation method across all treatments (Air, stem gas, C2H2 + Air, C2H2 + stem gas, DFM + stem gas, 0.3% CH4 + stem gas, Control), 1/2/4-day incubations. Underlies Figure 2 and Tables S5–S7.
- `ARA_vs_15N_data.csv` — Paired BNF rates from both methods (ARA and 15N2 assimilation) on the same samples, for the C2H2 + stem gas, C2H2 + Air, and stem gas treatments after 4 days. Underlies Figure 5 and Table S14.
- `data_CH4.csv` — δ15N and BNF rates from the CH4-addition experiment (5% CH4, March 2025): 15N2 + CH4 + O2, 15N2 + O2, and autoclaved Control treatments, at 1/2/3/5/7-day incubations (Control at days 3 and 7 only). Underlies Figure 3 and Tables S8–S10.
- `data_O2_CH4.csv` — Headspace O2 (%) measured in the same CH4-addition experiment vials. Underlies Figure 3 and Tables S11–S12, and the O2 vs δ15N correlation (Figure 4).

### `phyloseq-export/`

File exports of the QIIME 2 (v2025.7) processing outputs for the 16S rRNA amplicon sequencing of *Melaleuca quinquenervia* bark (3 replicate samples: Bark-1, Bark-2, Bark-3), processed with the DADA2 plugin (forward reads truncated at 250 bp, reverse at 210 bp) and classified against the SILVA 138 database using a Naive Bayes classifier (`classify-sklearn`, plant-surface weighted variant).

Raw QIIME 2 artifacts (`.qza`) are not included in this repository. The underlying raw sequencing reads are deposited separately in NCBI SRA (BioProject PRJNA1533092); together with the processing parameters stated above and in the Methods, the QIIME 2 pipeline can be rerun from the raw reads if the full artifact provenance is needed.

`phyloseq-export/` — the files the R script below reads directly:

```
phyloseq-export/
├── feature-table/asv-table.tsv
├── taxonomy/taxonomy.tsv
├── metadata/sample-metadata.tsv
├── sequences/dna-sequences.fasta
└── tree/tree.nwk
```

### `r_scripts/`

- `ARA_vs_15N2.rmd` — reads `ARA_data_publication.csv`, `15N_data.csv`, and `ARA_vs_15N_data.csv`. Fits: a two-way ANOVA for the ARA data (treatment × incubation time); a GLM (gamma distribution, log link) for the main 15N2 data (treatment × incubation time, all treatments); and a second GLM comparing ARA vs. 15N2 assimilation method rates (method × treatment, stem gas/C2H2+stem gas/C2H2+Air, after 4 days). All GLM main effects and interactions are tested with Type III F-tests (`car::Anova`); pairwise post hoc comparisons use Tukey's HSD (Tables S3, S4, S6) or Benjamini-Hochberg adjustment (Table S7), as noted in each table's caption. Produces Figures 1, 2, and 5, and Tables S2–S7 and S14.

- `CH4_paperbark_markdown.Rmd` — reads `data_CH4.csv` and `data_O2_CH4.csv`. Because the autoclaved Control samples in this experiment were only measured at 2 of 5 incubation times (days 3 and 7), the full three-treatment model is rank-deficient and cannot support a valid omnibus test. This script therefore fits **three** GLM objects (gamma distribution, log link) rather than one:
  - `glm_model` — full model (all three treatments, all time points). Kept for reference only; not used for inference due to the rank-deficiency above.
  - `glm_model_no_control` — methane vs. no-methane only, across all five incubation times (balanced, complete). Source of the omnibus treatment/time/interaction test reported in Results, and of the methane-vs-no-methane rows in Table S9 and the within-treatment time comparisons in Table S10.
  - `glm_model_day3_7` — all three treatments, restricted to days 3 and 7 only (balanced, complete). Source of the omnibus Control-vs-treatment test, the Control comparison rows in Table S9, and the Control day-3-vs-day-7 row in Table S10.

  All pairwise post hoc comparisons across these models use Tukey's HSD. The script also fits a two-way ANOVA on `data_O2_CH4.csv` for the headspace oxygen dynamics (Tables S11–S12) and runs the Spearman correlation between O2 % and δ15N (Figure 4). Produces Figure 3, Figure 4, and Tables S8–S12.

- `16S_ampliconseq_manuscript_silva2.Rmd` — imports the exported files above into a `phyloseq` object; generates the taxonomic composition figures (phylum- and class-level relative abundance, incl. the manuscript's community composition figure), alpha-diversity metrics (Observed ASVs, Shannon index, Faith's PD), beta-diversity ordinations (Bray-Curtis and weighted UniFrac PCoA), and methanotroph relative-abundance summaries reported in the manuscript.

## Requirements

R (v4.5.0) with the following packages:

- Amplicon analysis: `phyloseq`, `qiime2R`, `tidyverse`, `vegan`, `ggtree`, `treeio`, `RColorBrewer`, `genefilter`, `pheatmap`, `patchwork`, `ape`, `Biostrings`
- BNF/statistical analysis: `car`, `emmeans`, `DHARMa`, `ggplot2`

## Citation

If you use these data or code, please cite the manuscript above.
