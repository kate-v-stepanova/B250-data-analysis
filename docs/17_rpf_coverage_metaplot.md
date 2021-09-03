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

## For larger datasets

### 0. Split the reference files into 10 pieces



### 1. Get the data

```
for i in {10..100..10}; do for f in $(ls $BASE_DIR/17920/analysis/output/ext_diricore/all_unique/tsv/*.tsv); do s=$(basename $f); s=${s%.tsv}; echo "bsub -q long -R \"rusage[mem=80G]\" python $BASE_DIR/software/ext_diricore/rpf_coverage/4_rpf_coverage_aa_combination_metaplot_split.py 17920 all_unique 3 60 hg19 all_genes all_genes ${i}perc $s"; done; done
```

### 2. Merge the data into 1 file

```
cd $BASE_DIR/17920/analysis/output/ext_diricore/all_unique/rpf_coverage_aa_metaplot/all_genes/
mkdir split
cp 10perc/*.tsv split

for i in {20..100..10}; do for f in $(ls ${i}perc/*.tsv); do fn=$(basename $f); echo "cat $f >> split/$fn"; done; done > 1.sh

bash ./1.sh
rm 1.sh
```

Now merged files will be located here: `$BASE_DIR/17920/analysis/output/ext_diricore/all_unique/rpf_coverage_aa_metaplot/split`

### 3. Plot the results

```
bsub -q long -R "rusage[mem=100G]" python  $BASE_DIR/software/ext_diricore/rpf_coverage/5_rpf_coverage_metaplot.py 18927 all_unique split -12 60 50 400
```
