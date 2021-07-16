# Ext Diricore (P-site signalling)

> **_IMPORTANT:_** This step requires [plastid](https://plastid.readthedocs.io/en/latest/). To install, run the command: `pip install plastid` 

## 0. Set up plastid

Install plastid: 

```
source activate p3
pip install plastid
```


Create reference for human (for mouse it's the same, just take the annotation for mouse - replace `hg19` with `mm10`): 

```
bsub -q long -R "rusage[mem=50G]" metagene generate $BASE_DIR/static/hg19/plastid --landmark cds_start --annotation_files $BASE_DIR/static/hg19/gencode.annotation.gtf --downstream 200
```

This will create 2 files `plastid_rois.bed` and `plastid_rois.txt` at the static directory: `$BASE_DIR/static/hg19/`



## 1. Get sequences from bam

Get sequences: 

```
bsub -R "rusage[mem=10G]" $BASE_DIR/software/ext_diricore/1_get_seq_from_bam.sh 22276 all_unique
```

## 2. Index bam files 

```
for f in $(ls $BASE_DIR/22276/analysis/output/alignments/toGenome/*_toGenome_dedup.bam); do echo "samtools index $f"; done
```

## 3. Calculate offsets by plastid 

```
for f in $(ls $BASE_DIR/22276/analysis/output/alignments/toGenome/*_dedup.bam); do fn=$(basename $f); fn=${fn%_toGenome_dedup.bam};  echo "bsub -q medium  -R "rusage[mem=10G]" psite $BASE_DIR/static/hg19/plastid_rois.txt  $BASE_DIR/22276/analysis/output/plastid/p_offsets/all_unique/$fn --min_length 25 --max_length 35 --require_upstream --count_files $f --title "$fn""; done
``` 

This will calculate the offset for each read length (per sample), and create some images + data files in this directory: `$BASE_DIR/22276/analysis/output/plastid/p_offsets/`

Copy the plots from the cluster (run it locally on the laptop): 

```
scp -r USERNAME@odcf-cn34u03s10:BASE_DIR/22276/analysis/output/figures/p_offsets ~/analysis/22276/figures/
```

`USERNAME` has to be replaced with the real username (e.g. `e984a`), and `BASE_DIR` has to be replaced with the real BASE_DIR (e.g. `/omics/groups/OE0532/internal/from_snapshot/`)

The plot will look like that:

![](/pics/plastid.png) 

## 4. Manually re-format offset files

Now we need to extract the offset and eliminate non-relevant data from the plastid output.
 
Create data dir: 

```
mkdir -p $BASE_DIR/22276/analysis/output/ext_diricore/all_unique/p_offset
```

Copy data files:

```
for f in $(ls $BASE_DIR/22276/analysis/output/figures/p_offsets/all_unique/*p_offsets.txt); do echo "cp $f $BASE_DIR/22276/analysis/output/ext_diricore/all_unique/p_offset/"; done
```

Remove lines starting with `#`:

```
for f in $(ls $BASE_DIR/22276/analysis/output/ext_diricore/all_unique/p_offset/*); do echo "cat $f | grep -v '^#' | grep -v default > $f"1; echo "mv $f"1 $f; done
```

Now the offset file will look like that:

```
length  p_offset
25      7
26      10
27      9
28      10
29      11
30      12
31      13
32      14
33      5
34      15
35      13
```

So we have to remove the lines which don't look that good. In this case, the final result will look like that:

```
length  p_offset
28  10
29  11
30  12
31  13
32  14
```

How do we know which ones look good and which ones don't? Generally, the perfect lengths are around 28-30 and perfect offset is around 12. If we see that the offset is too low (e.g. 7 or 5) or too high (e.g. 20 or 50) - discard these rows. 

In this case the length 33 has offset 5 - this looks wierd, so I remove everything after 33. 

Also, the default value is 13, so if we see too many 13s - most likely something is wrong. 

Repeat this for all samples.

## 5. Get normalized counts 

```
bsub -q long -R "rusage[mem=20G]" python $BASE_DIR/software/counts/2_normalize_counts.py 22276 all_unique hg19
```

Output dir: `$BASE_DIR/22276/analysis/output/alignments/reads_per_gene/normalized/`

## 6. Get psites counts 

```
bsub -q long -R "rusage[mem=20G]" python $BASE_DIR/software/ext_diricore/5_psites_counts_3.py 22276 all_unique 20 hg19
```

Output dir1: `$BASE_DIR/22276/analysis/output/ext_diricore/all_unique/psites`
Output dir2: `$BASE_DIR/22276/analysis/output/ext_diricore/all_unique/psites_dotplot`

## 7. Plot results

The psite counts have been calculated with 3 types of normalization: CPM, TPM and RPKM. We can choose which normalization we use in the plots (tpm is default)


### Plot p-sites, a-sites and e-sites per sample 

TPM:

```
python $BASE_DIR/software/ext_diricore/5_psites_plot.py 22276 all_unique
```

or 

```
python $BASE_DIR/software/ext_diricore/5_psites_plot.py 22276 all_unique tpm
```

RPKM: 

```
python $BASE_DIR/software/ext_diricore/5_psites_plot.py 22276 all_unique rpkm
```

This will produce 2 types of plots - grouped by codon and grouped by amino acid

Grouped by amino acid:

![](/pics/ext_diricore_1.png)

Grouped by codon:

![](/pics/ext_diricore_2.png)

### Plot p-sites, a-sites and e-sites per contrast

TPM: 

```
python $BASE_DIR/software/ext_diricore/5_psites_plot_contrasts.py 22276 all_unique
```

RPKM: 

```
python $BASE_DIR/software/ext_diricore/5_psites_plot_contrasts.py 22276 all_unique rpkm
``` 

Grouped by amino acid:

![](/pics/ext_diricore_3.png)

Grouped by codon:

![](/pics/ext_diricore_4.png)


### Plot reads per frame

```
module load R/3.6.2 && module load gcc/7.2.0 && Rscript $BASE_DIR/software/ext_diricore/5_plot_reads_per_frame.r 22276 all_unique
```

¡[](/pics/ext_diricore_5.png)

### Web-site results

Upload to the web-site: `python $BASE_DIR/software/project_list.py psites 22276 all_unique --remote` 

See the results: http://b250-bioinfo:5000/psite_plot/22276

![](/pics/ext_diricore_6.png)
