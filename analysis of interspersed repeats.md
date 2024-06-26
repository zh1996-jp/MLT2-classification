# Analysis of interspersed repeats in the human genome

First we can download consensus DNA sequences of human repeat elements from [Dfam][] (details can be found from [TE-reX][]):
    dfam-fasta -c9606 -x'root;Interspersed_Repeat' > reps.fa

Then annotate repeats in the human genome (GRCh38/hg38), by comparing the genome to these consensus sequences using [LAST][]. 

Prepare a database (say "db") for the repeat sequences file 'repeats.fa':

    lastdb db repeats.fa

Next, [determine the rates of insertion, deletion and
substitutions][train] between genome and consensus sequences:

    last-train -P8 --revsym -X1 -m100 db human_genome.fa > rep.train

* `-P8` makes it faster by using 8 parallel threads: adjust as
  suitable for your computer.  This has no effect on the results.
* `-X1` tells it to treat `N`s in the consensus sequences as unknown
  bases.
* `--revsym` option enforces reverse-complement symmetry of the substitution rates (e.g. c→t implies g→ a on the opposite strand). 
* `m100` parameter makes it more slow and sensitive, by permitting a maximum of 100 initial matches per query (genome) position. 

Then, use lastal to find alignments:

    lastal -P8 -p rep.train -m100 -D1e7 --split repdb genome.fa | last-postmask > genome-to-reps.maf

* `-P8` (8 threads) makes it faster by using 8 parallel threads.
* `-m100` set a high sensitivity for alignment detection.
* `-D1e7` selects alignments that are rarely expected to occur by chance in random sequences: at most once per 107query base-pairs. 

Finally, we used [TE-reX][], a tool that builds upon the results produced by LAST, to identify and extract ”hybrid TEs” from the genome:

    te-rex genome-to-reps.maf reps-to-reps.tab my-out

* `reps-to-reps.tab` file has pairwise alignments between repeat consensus sequences, indicating which
coordinates in one consensus are homologous to which coordinates in another.

Now we can get a list of hybrid elements, `my-out.txt` shows the count of each type of hybrid:

    2058  MLT2A2#LTR/ERVL       MLT2B3#LTR/ERVL       MLT2A2#LTR/ERVL
    212   L1MD2_5end#LINE/L1    L1MEf_5end#LINE/L1
    124   L1P1_5end#LINE/L1     L1P2_5end#LINE/L1

As we can see from the result, a type of hybrid TE occurs 2058 times in the human genome:   

    MLT2A2#LTR/ERVL       MLT2B3#LTR/ERVL       MLT2A2#LTR/ERVL


[LAST]: https://gitlab.com/mcfrith/last
[train]: https://gitlab.com/mcfrith/last/-/blob/main/doc/last-train.rst
[mismap probability]: https://gitlab.com/mcfrith/last/-/blob/main/doc/last-split.rst
[BED]: https://genome.ucsc.edu/FAQ/FAQformat.html#format1
[Dfam]: https://dfam.org/home
[SVA]: https://dfam.org/browse?name_accession=SVA
[TE-reX]: https://github.com/mcfrith/te-rex

