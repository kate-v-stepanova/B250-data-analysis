# Cluster Profiler (GO term)

> **_IMPORTANT:_** `$BASE_DIR` has to be specified in your `.bash_profile`. Details [here](docs/0_before_you_start.md)



## 1. Get sequences

```
module load samtools
bsub -q medium -R "rusage[mem=30G]" $BASE_DIR/software/ext_diricore/1_get_seq_from_bam.sh 22276 all_unique
```

## 2. Get gene counts:

All counts: 

```
bsub -q medium -R "rusage[mem=30G]" python $BASE_DIR/software/ext_diricore/0_get_gene_counts.py 22276 all_unique hg19
```

CDS counts: 

```
bsub -q medium -R "rusage[mem=30G]" python $BASE_DIR/software/ext_diricore/0_get_counts_from_cds.py 22276 all_unique hg19
```

## 3. Diff expr:

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

## 4. Agg contrasts into 1 file

All:

```
python $BASE_DIR/software/diff_expr/2_diff_expr.py 22276 all_unique 20 pool_24
python $BASE_DIR/software/diff_expr/2_diff_expr.py 22276 all_unique 100 pool_25
python $BASE_DIR/software/diff_expr/2_diff_expr.py 22276 all_unique 50 pool_26
```

CDS:

```
python $BASE_DIR/software/diff_expr/2_diff_expr.py 22276 all_unique_cds 20 pool_24
python $BASE_DIR/software/diff_expr/2_diff_expr.py 22276 all_unique_cds 100 pool_25
python $BASE_DIR/software/diff_expr/2_diff_expr.py 22276 all_unique_cds 50 pool_26
```


## 5. GO term

Load R module:

```
module load gcc/7.2.0
module load R/3.6.2
```

All:

```
for t in 2 -2 1.5 -1.5; do echo "bsub -q medium -R \"rusage[mem=30G]\" Rscript $BASE_DIR/software/diff_expr/cluster_profiler_2.r 22276 all_unique 20 $t hg19 pool_24"; done
for t in 2 -2 1.5 -1.5; do echo "bsub -q medium -R \"rusage[mem=30G]\" Rscript $BASE_DIR/software/diff_expr/cluster_profiler_2.r 22276 all_unique 100 $t hg19 pool_25"; done
for t in 2 -2 1.5 -1.5; do echo "bsub -q medium -R \"rusage[mem=30G]\" Rscript $BASE_DIR/software/diff_expr/cluster_profiler_2.r 22276 all_unique 50 $t hg19 pool_26"; done
```

CDS:

```
for t in 2 -2 1.5 -1.5; do echo "bsub -q medium -R \"rusage[mem=30G]\" Rscript $BASE_DIR/software/diff_expr/cluster_profiler_2.r 22276 all_unique_cds 20 $t hg19 pool_24"; done
for t in 2 -2 1.5 -1.5; do echo "bsub -q medium -R \"rusage[mem=30G]\" Rscript $BASE_DIR/software/diff_expr/cluster_profiler_2.r 22276 all_unique_cds 100 $t hg19 pool_25"; done
for t in 2 -2 1.5 -1.5; do echo "bsub -q medium -R \"rusage[mem=30G]\" Rscript $BASE_DIR/software/diff_expr/cluster_profiler_2.r 22276 all_unique_cds 50 $t hg19 pool_26"; done
```

## 5. copy results 

```
scp -r USERNAME@odcf-cn34u03s10:$BASE_DIR/22276/analysis/output/figures/go_term/ ~/analysis/22276/figures
```

Note that the dir `~/analysis/22276/figures` has to be created on the local computer



# Cluster Profiler with TE as input

## 1. Get TE results

Get TE results as described [here](docs/10_ribodiff.md)

## 2. Aggregate TE results:

```
python /omics/groups/OE0532/internal/from_snapshot/software/diff_expr/aggregate_TE_results.py 21221 all_unique
```

## 3. Cluster Profiler:

```
module load R/3.6.2
module load gcc/7.2.0

for t in 2 -2 1.5 -1.5; do echo "bsub -q medium -R \"rusage[mem=30G]\" Rscript $BASE_DIR/software/diff_expr/cluster_profiler_TE.r 21221 all_unique 20 $t hg19
```

# ClusterProfiler for a dataset with replicates

Example: 21221

## 1. Create replicates file

```
vim $BASE_DIR/21221/analysis/input/metadata/replicates.tsv
```

The file should look as following:

```
sample  condition
sh12_1  sh12
sh12_2  sh12
sh12_3  sh12
sh40_1  sh40
sh40_2  sh40
sh40_3  sh40
GFP_sh_1        GFP_sh
GFP_sh_2        GFP_sh
GFP_sh_3        GFP_sh
CTN_sh_1        CTN_sh
CTN_sh_2        CTN_sh
CTN_sh_3        CTN_sh
```

## 2. Create contrasts file

```
vim $BASE_DIR/21221/analysis/input/metadata/rpf_density_contrasts_replicates.tsv
```

The file should look like this:

```
CTN_sh  GFP_sh  #
sh12    GFP_sh  #
sh40    GFP_sh  #
```

## 3. Get sequences

```
bsub -q medium -R "rusage[mem=30G]" $BASE_DIR/software/ext_diricore/1_get_seq_from_bam.sh 21221 all_unique
bsub -q medium -R "rusage[mem=30G]" $BASE_DIR/software/ext_diricore/1_get_seq_from_bam.sh 21221_RNA all_unique
bsub -q medium -R "rusage[mem=30G]" python $BASE_DIR/software/ext_diricore/0_get_gene_counts.py 21221 all_unique hg19
bsub -q medium -R "rusage[mem=80G]" python $BASE_DIR/software/ext_diricore/0_get_gene_counts.py 21221_RNA all_unique hg19

```

## 4. Diff Expression


```
Rscript $BASE_DIR/software/diff_expr/1_diff_expr_with_replicates.r 21221 all_unique 20 hg19 replicates
Rscript $BASE_DIR/software/diff_expr/1_diff_expr_with_replicates.r 21221_RNA all_unique 20 hg19 replicates
```

## 5. Aggregate counts

```
python $BASE_DIR/software/diff_expr/2_diff_expr.py 21221 all_unique 20 replicates
python $BASE_DIR/software/diff_expr/2_diff_expr.py 21221_RNA all_unique 20 replicates
```

## 6. Cluster Profiler

```
for t in 2 -2 1.5 -1.5; do echo "bsub -q medium -R \"rusage[mem=30G]\" Rscript $BASE_DIR/software/diff_expr/cluster_profiler_2.r 21221 all_unique 20 $t hg19 replicates"; done
for t in 2 -2 1.5 -1.5; do echo "bsub -q medium -R \"rusage[mem=30G]\" Rscript $BASE_DIR/software/diff_expr/cluster_profiler_2.r 21221_RNA all_unique 20 $t hg19 replicates"; done
```

