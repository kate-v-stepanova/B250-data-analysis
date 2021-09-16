# PCA plot

1. Get sequences: 

```
bsub -R "rusage[mem=10G]" $BASE_DIR/software/ext_diricore/1_get_seq_from_bam.sh 21221_RNA all_unique
```

2. Load modules

```
module load R/3.4.2
module load gcc/7.2.0
```

3. Get plots

All samples: 

```
Rscript $BASE_DIR/software/simple_scripts/pca_plot.r 21221_RNA hg19 all_unique
```

Pool 24: 

```
Rscript $BASE_DIR/software/scripts/pca_plot.r 21012 mm10 all_unique pool_24
```

Pool 26: 

```
Rscript $BASE_DIR/software/scripts/pca_plot.r 21012 mm10 all_unique pool_26
```
