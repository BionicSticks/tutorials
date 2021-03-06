# Pan-Genome Analysis

In this tutorial we will learn how to determine a pan-genome from a collection of isolate genomes.

This tutorial is inspired from [Genome annotation and Pangenome Analysis](https://github.com/microgenomics/tutorials/blob/master/pangenome.md) from the CBIB in Santiago, Chile

## Getting the data

We'll use six *Listeria monocytogenes* genomes in this tutorial.

```bash
wget https://github.com/HadrienG/tutorials/blob/master/data/pangenome.tar.gz
tar xzf pangenome.tar.gz
cd pangenome
```

These genomes correspond to isolates of *L. monocytogenes* reported in

> Xiangyu Deng, Adam M Phillippy, Zengxin Li, Steven L Salzberg and Wei Zhang. (2010) Probing the pan-genome of Listeria monocytogenes: new insights into intraspecific niche expansion and genomic diversification. doi:10.1186/1471-2164-11-500

The six genomes you downloaded were selected based on their level of completeness (finished; contigs, etc) and their genotype (type I-III):

Genome Assembly	| Genome Accession | Genotype | Sequenced by | Status
--- | --- | --- | --- | ---
GCA_000026705 | FM242711 | type I | Institut_Pasteur | Finished
GCA_000008285 | AE017262 | type I | TIGR | Finished
GCA_000168815 | AATL00000000 | type I | Broad Institute | 79 contigs
GCA_000196035 | AL591824 | type II | European Consortium | Finished
GCA_000168635 | AARW00000000 | type II | Broad Institute | 25 contigs
GCA_000021185 | CP001175 | type III | MSU | Finished

## Annotation of the genomes

By annotating the genomes we mean to add information regarding genes, their location, strandedness, and features and attributes. Now that you have the genomes, we need to annotate them to determine the location and attributes of the genes contained in them. We will use Prokka for the annotation.

```
prokka --kingdom Bacteria --outdir prokka_GCA_000008285 --genus Listeria --locustag GCA_000008285 GCA_000008285.1_ASM828v1_genomic.fna
```

Annotate the 6 genomes, by replacing the `-outdir` and `-locustag` and `fasta file` accordingly.

## Pan-genome analysis

put all the .gff files in the same folder (e.g., ./gff) and run Roary

`roary -f results -e -n -v gff/*.gff`

Roary will get all the coding sequences, convert them into protein, and create pre-clusters. Then, using BLASTP and MCL, Roary will create clusters, and check for paralogs. Finally, Roary will take every isolate and order them by presence/absence of orthologs. The summary output is present in the `summary_statistics.txt` file.

Additionally, Roary produces a `gene_presence_absence.csv` file that can be opened in any spreadsheet software to manually explore the results. In this file, you will find information such as gene name and gene annotation, and, of course, whether a gene is present in a genome or not.

## Plotting the result

Roary comes with a python script that allows you to generate a few plots to graphically assess your analysis output.

First, we need to generate a tree file from the alignment generated by Roary:

```
FastTree –nt –gtr core_gene_alignment.aln > my_tree.newick
```

Then we can plot the Roary results:

```
roary_plots.py my_tree.newick gene_presence_absence.csv
```
