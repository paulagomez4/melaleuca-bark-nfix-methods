# Bark BNF Methods — Data and Code

Data and code accompanying:

Gomez-Alvarez, P., Jeffrey, L., Cook, P., Dittmann, J., Erler, D., Carvalho, M., Wen, W., Johnston, S.G., Leung, P.M., Deschaseaux, E., Greening, C., Maher, D. "Nitrogen fixation in tree bark: methodological advances reveal an overlooked forest nitrogen source." Submitted to *New Phytologist*.

Raw 16S rRNA amplicon sequencing reads (Illumina, V4 region, 515F/806R primers) are deposited in the NCBI Sequence Read Archive under BioProject accession [PRJNA1533092].

## Contents

### `qiime2_outputs/`

QIIME 2 (v2025.7) artifacts generated from 16S rRNA amplicon sequencing of *Melaleuca quinquenervia* bark (3 replicate samples: Bark-1, Bark-2, Bark-3), processed with the DADA2 plugin (forward reads truncated at 250 bp, reverse at 210 bp) and classified against the SILVA 138 database using a Naive Bayes classifier (`classify-sklearn`, plant-surface weighted variant).

- `feature-table.qza` — ASV feature table (`FeatureTable[Frequency]`)
- `rep-seqs.qza` — representative ASV sequences (`FeatureData[Sequence]`)
- `taxonomy.qza` — SILVA 138 taxonomic assignments (`FeatureData[Taxonomy]`)
- `rooted-tree.qza` — rooted phylogenetic tree, MAFFT alignment + FastTree (`Phylogeny[Rooted]`)

`qiime2_outputs/phyloseq-export/` — the same four artifacts exported to flat files, in the exact folder structure the R script below reads directly:

```
phyloseq-export/
├── feature-table/asv-table.tsv
├── taxonomy/taxonomy.tsv
├── metadata/sample-metadata.tsv
├── sequences/dna-sequences.fasta
└── tree/tree.nwk
```

### `r_scripts/`

- `16S_ampliconseq_manuscript_silva2.Rmd` — imports the exported files above into a `phyloseq` object; generates the taxonomic composition figures (phylum- and class-level relative abundance, incl. the manuscript's community composition figure), alpha-diversity metrics (Observed ASVs, Shannon index, Faith's PD), beta-diversity ordinations (Bray-Curtis and weighted UniFrac PCoA), and methanotroph relative-abundance summaries reported in the manuscript.

## Requirements

R (v4.5.0) with the following packages: `phyloseq`, `qiime2R`, `tidyverse`, `vegan`, `ggtree`, `treeio`, `RColorBrewer`, `genefilter`, `pheatmap`, `patchwork`, `ape`, `Biostrings`.

## Citation

If you use these data or code, please cite the manuscript above.
