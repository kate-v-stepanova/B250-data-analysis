# PCA plot

## 1. Get sequences: 

```
bsub -R "rusage[mem=10G]" $BASE_DIR/software/ext_diricore/1_get_seq_from_bam.sh 21221_RNA all_unique
```

## 2. Load modules

```
module load R/3.4.2
module load gcc/7.2.0
```

## 3. Get plots

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

# PCA plot for 2 datasets together

Let's say we have 2 datasets which we want to plot together, e.g. 23108 and 23109.

## 1. Get sequences for both datasets:

```
bsub -R "rusage[mem=10G]" $BASE_DIR/software/ext_diricore/1_get_seq_from_bam.sh 23108 all_unique
bsub -R "rusage[mem=10G]" $BASE_DIR/software/ext_diricore/1_get_seq_from_bam.sh 23109 all_unique
```

## 2. Create symlinks

Let's create a directory where all the files will be places together:

```
mkdir -p $BASE_DIR/23108/analysis/output/ext_diricore/all_unique_23108_23109/tsv
```

Create symlinks for 23108 files:

```
for f in $(ls $BASE_DIR/23108/analysis/output/ext_diricore/all_unique/tsv/*); do fn=$(basename $f); fn=23108_$fn; echo "ln -s $f $BASE_DIR/23108/analysis/output/ext_diricore/all_unique_23108_23109/tsv/$fn"; done
```

Create symlinks for 23109 files:

```
for f in $(ls $BASE_DIR/23109/analysis/output/ext_diricore/all_unique/tsv/*); do fn=$(basename $f); fn=23109_$fn; echo "ln -s $f $BASE_DIR/23108/analysis/output/ext_diricore/all_unique_23108_23109/tsv/$fn"; done
```

Now we have all files in one directory.

## 3. Get plots

```
bsub -q medium -R "rusage[mem=30G]" Rscript $BASE_DIR/software/simple_scripts/pca_plot.r 23108 mm10 all_unique_23108_23109
```
