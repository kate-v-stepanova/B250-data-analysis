# Cumulative plots

## 1. Get sequences from bam

```
$BASE_DIR/software/ext_diricore/1_get_seq_from_bam.sh 22276 all_unique
```

## 2. Get cumulative plot

```
python $BASE_DIR/software/ext_diricore/cumulative/22_cumulative_reads.py 22276 all_unique $BASE_DIR/22276/analysis/input/metadata/MT-genes.txt
```

## 3. Get cumulative plot for a subset of genes
