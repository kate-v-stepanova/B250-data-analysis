# RPF coverage metaplot

## For smaller datasets

Let's consider not that large datasets, which is around `12M` aligned reads per sample (or less). If it is a larger dataset, still can try this option - because it's easier. If it did not work, then go for the method described in next section.

### 0. Generate reference files

Skip this step if references for this genome have already been created.

```
python $BASE_DIR/software/ext_diricore/triplets/00_get_codon_posistions_any_combination.py 3 hg19
```

### 1. Get sequences from bam

```
bsub -q long -R "rusage[mem=50G]" $BASE_DIR/software/ext_diricore/1_get_seq_from_bam.sh 17920 all_unique
```

### 2. Get data

```
bsub -q long -R "rusage[mem=200G]" python $BASE_DIR/software/ext_diricore/rpf_coverage/4_rpf_coverage_aa_combination_metaplot.py 18927 all_unique 3 60 hg19
```

If this step failed because of the memory limit, check instructions for larger datasets.

### 3. Plot the results

For position -12, window size = 60, min reads per window = 50, min number of windows per combination = 400

```
bsub -q long -R "rusage[mem=100G]" python  $BASE_DIR/software/ext_diricore/rpf_coverage/5_rpf_coverage_metaplot.py 18927 all_unique all_genes -12 60 50 400
```
