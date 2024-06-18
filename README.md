# Analysis of Interspersed Repeats in the Human Genome

This repository contains the analysis of interspersed repeats in the human genome (GRCh38/hg38) using a combination of bioinformatics tools and databases.

## Data Sources
- Consensus DNA sequences of human repeat elements were downloaded from [Dfam](https://dfam.org/), a database of transposable element families.
- The human genome (GRCh38/hg38) was used as the reference genome.

## Methods
1. **Annotation of Repeats**: Repeats in the human genome were annotated by comparing the genome to consensus sequences using [LAST](https://last.cbrc.jp/).
2. **Training**: The average rates of insertion, deletion, and substitutions between the genome and repeat consensus sequences were determined using [last-train](https://last.cbrc.jp/).
3. **Alignments**: Alignments were found using [lastal](https://last.cbrc.jp/), and the output was processed with [last-postmask](https://last.cbrc.jp/) to remove simple-sequence alignments of dubious homology.
4. **Identification of "Hybrid TEs"**: The tool [TE-reX](https://github.com/dfam-consortium/TE-reX) was used to identify and extract "hybrid TEs" from the genome.

## Classification of Subfamilies
- Self-comparisons of targeted subfamily instances were performed using [rmblast.pl](https://www.repeatmasker.org/) in [RepeatModeler version 2.0.3](https://www.repeatmasker.org/).
- [AlignAndCallConsensus.pl](https://www.repeatmasker.org/) was used to refine the classification based on multiple sequence alignments.

## Construction of Evolutionary Tree
- Instances of MLT2 larger than 300 base pairs were extracted from the genome and aligned using [MAFFT](https://mafft.cbrc.jp/).
- The aligned sequences were inputted into [FastTree](https://www.microbesonline.org/fasttree/) to generate a maximum-likelihood tree.

## Evolutionary Relationship Analysis
- [Lift-over](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC1000000/) was employed to investigate genomic similarities across species, helping to understand evolutionary relationships and conservation of genetic elements.



For detailed information on each step, refer to the corresponding sections in the repository.








