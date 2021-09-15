# Cluster Profiler (GO term)

> **_IMPORTANT:_** `$BASE_DIR` has to be specified in your `.bash_profile`. Details [here](docs/0_before_you_start.md)



1. Get sequences
```
bsub -q medium -R "rusage[mem=30G]" $BASE_DIR/software/ext_diricore/1_get_seq_from_bam.sh 22276 all_unique
```

2. Get gene counts:

All counts: 

```
bsub -q medium -R "rusage[mem=30G]" python $BASE_DIR/software/ext_diricore/0_get_gene_counts.py 22276 all_unique hg19
```

CDS counts: 

```
bsub -q medium -R "rusage[mem=30G]" python $BASE_DIR/software/ext_diricore/0_get_counts_from_cds.py 22276 all_unique hg19
```

2. Diff expr:

```
module load gcc/7.2.0
module load R/3.4.2
```

pool 24 (all counts + cds counts): 

```
Rscript $BASE_DIR/software/diff_expr/1_diff_expr.r 22276 all_unique 20 hg19 pool_24
Rscript $BASE_DIR/software/diff_expr/1_diff_expr.r 22276 all_unique_cds 20 hg19 pool_24
```

pool 25 (all counts + cds counts): 

```
Rscript $BASE_DIR/software/diff_expr/1_diff_expr.r 22276 all_unique 100 hg19 pool_25

Rscript $BASE_DIR/software/diff_expr/1_diff_expr.r 22276 all_unique_cds 100 hg19 pool_25
```

pool 26 (all counts + cds counts): 

```
Rscript $BASE_DIR/software/diff_expr/1_diff_expr.r 22276 all_unique 50 hg19 pool_26
Rscript $BASE_DIR/software/diff_expr/1_diff_expr.r 22276 all_unique_cds 50 hg19 pool_26
```

3. Agg contrasts into 1 file:

pool_24: `python $BASE_DIR/software/diff_expr/2_diff_expr.py 22276 all_unique_pool_24 20`

pool_25: `python $BASE_DIR/software/diff_expr/2_diff_expr.py 22276 all_unique_pool_25 100`

pool_26: `python $BASE_DIR/software/diff_expr/2_diff_expr.py 22276 all_unique_pool_26 50`

4. GO term

pool_24: 

```
for t in 2 -2 1.5 -1.5; do echo "bsub -q medium -R \"rusage[mem=30G]\" Rscript $BASE_DIR/software/diff_expr/cluster_profiler_2.r 22276 all_unique_pool_24 20 $t hg19"; done
``` 

pool_25: 

```
for t in 2 -2 1.5 -1.5; do echo "bsub -q medium -R \"rusage[mem=30G]\" Rscript $BASE_DIR/software/diff_expr/cluster_profiler_2.r 22276 all_unique_pool_25 100 $t hg19"; done
```

pool_26: 

```
for t in 2 -2 1.5 -1.5; do echo "bsub -q medium -R \"rusage[mem=30G]\" Rscript $BASE_DIR/software/diff_expr/cluster_profiler_2.r 22276 all_unique_pool_26 50 $t hg19"; done
```

5. copy results: `scp -r e984a@odcf-cn34u03s10:$BASE_DIR/22276/analysis/output/figures/go_term/ /Users/b250-admin/analysis/22276/figures && scp -r e984a@odcf-cn34u03s10:$BASE_DIR/22276/analysis/output/go_term/ /Users/b250-admin/analysis/22276/data_files`

