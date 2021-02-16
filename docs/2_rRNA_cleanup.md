# rRNA cleanup

Before proceeding with regular alignment, we have to remove rRNA reads.

## 1. Create fasta from fastq

Create fasta from fastq `python /icgc/dkfzlsdf/analysis/OE0532/software/rRNA/faster/1_fasta_from_fastq.py 20910`

The output of this script will be the following:

```
bsub -q long  -R "rusage[mem=30G]" python /icgc/dkfzlsdf/analysis/OE0532/software/rRNA/faster/1_fasta_from_fastq.py 20910 S1_T2_late
bsub -q long  -R "rusage[mem=30G]" python /icgc/dkfzlsdf/analysis/OE0532/software/rRNA/faster/1_fasta_from_fastq.py 20910 S2_T4_late
bsub -q long  -R "rusage[mem=30G]" python /icgc/dkfzlsdf/analysis/OE0532/software/rRNA/faster/1_fasta_from_fastq.py 20910 S3_T5_late
bsub -q long  -R "rusage[mem=30G]" python /icgc/dkfzlsdf/analysis/OE0532/software/rRNA/faster/1_fasta_from_fastq.py 20910 S4_4T1_Ctrl
bsub -q long  -R "rusage[mem=30G]" python /icgc/dkfzlsdf/analysis/OE0532/software/rRNA/faster/1_fasta_from_fastq.py 20910 S5_T_Pool2
bsub -q long  -R "rusage[mem=30G]" python /icgc/dkfzlsdf/analysis/OE0532/software/rRNA/faster/1_fasta_from_fastq.py 20910 S6_Liver2
bsub -q long  -R "rusage[mem=30G]" python /icgc/dkfzlsdf/analysis/OE0532/software/rRNA/faster/1_fasta_from_fastq.py 20910 S7_Liver4
bsub -q long  -R "rusage[mem=30G]" python /icgc/dkfzlsdf/analysis/OE0532/software/rRNA/faster/1_fasta_from_fastq.py 20910 S8_Liver6
bsub -q long  -R "rusage[mem=30G]" python /icgc/dkfzlsdf/analysis/OE0532/software/rRNA/faster/1_fasta_from_fastq.py 20910 S9_Liver8
bsub -q long  -R "rusage[mem=30G]" python /icgc/dkfzlsdf/analysis/OE0532/software/rRNA/faster/1_fasta_from_fastq.py 20910 S10_T_Pool1
bsub -q long  -R "rusage[mem=30G]" python /icgc/dkfzlsdf/analysis/OE0532/software/rRNA/faster/1_fasta_from_fastq.py 20910 S11_Lung2
bsub -q long  -R "rusage[mem=30G]" python /icgc/dkfzlsdf/analysis/OE0532/software/rRNA/faster/1_fasta_from_fastq.py 20910 S12_Lung4
bsub -q long  -R "rusage[mem=30G]" python /icgc/dkfzlsdf/analysis/OE0532/software/rRNA/faster/1_fasta_from_fastq.py 20910 S13_Lung6
bsub -q long  -R "rusage[mem=30G]" python /icgc/dkfzlsdf/analysis/OE0532/software/rRNA/faster/1_fasta_from_fastq.py 20910 S14_Lung8
```

Just copy & paste this in the terminal. The jobs will be sumbitted. Each sample is proccessed in parallel, so the processing will be done faster. Once a job is finished (or failed), notification will be sent by email.

The script will output files here: `ls -lh /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/fastq_to_fasta`

## 2. Align with blat

Create an output dir for the next step: `mkdir -p /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna/blat_results`

Align with [blat](https://genome.ucsc.edu/goldenpath/help/blatSpec.html): 

```
for f in $(ls /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/fastq_to_fasta/*.fa); do fn=$(basename $f); fn=${fn%.fa}; echo "bsub -q long -R \"rusage[mem=50G]\" /icgc/dkfzlsdf/analysis/OE0532/software/diricore/programs/blat_for_linux -stepSize=5 -repMatch=4096 -minScore=0 -minIdentity=0 /icgc/dkfzlsdf/analysis/OE0532/static/mm9/rRNA_genes.2bit $f /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna/blat_results/${fn}.psl"; done
```

This step requires reference files. How to create these reference files, is described here: [Blat rRNA reference](docs/rrna_blat_reference.md)

This will output the following:

```
bsub -q long -R "rusage[mem=50G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore/programs/blat_for_linux -stepSize=5 -repMatch=4096 -minScore=0 -minIdentity=0 /icgc/dkfzlsdf/analysis/OE0532/static/mm9/rRNA_genes.2bit /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/fastq_to_fasta/S11_Lung2.fa /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna/blat_results/S11_Lung2.psl
bsub -q long -R "rusage[mem=50G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore/programs/blat_for_linux -stepSize=5 -repMatch=4096 -minScore=0 -minIdentity=0 /icgc/dkfzlsdf/analysis/OE0532/static/mm9/rRNA_genes.2bit /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/fastq_to_fasta/S12_Lung4.fa /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna/blat_results/S12_Lung4.psl
bsub -q long -R "rusage[mem=50G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore/programs/blat_for_linux -stepSize=5 -repMatch=4096 -minScore=0 -minIdentity=0 /icgc/dkfzlsdf/analysis/OE0532/static/mm9/rRNA_genes.2bit /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/fastq_to_fasta/S13_Lung6.fa /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna/blat_results/S13_Lung6.psl
bsub -q long -R "rusage[mem=50G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore/programs/blat_for_linux -stepSize=5 -repMatch=4096 -minScore=0 -minIdentity=0 /icgc/dkfzlsdf/analysis/OE0532/static/mm9/rRNA_genes.2bit /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/fastq_to_fasta/S14_Lung8.fa /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna/blat_results/S14_Lung8.psl
bsub -q long -R "rusage[mem=50G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore/programs/blat_for_linux -stepSize=5 -repMatch=4096 -minScore=0 -minIdentity=0 /icgc/dkfzlsdf/analysis/OE0532/static/mm9/rRNA_genes.2bit /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/fastq_to_fasta/S1_T2_late.fa /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna/blat_results/S1_T2_late.psl
bsub -q long -R "rusage[mem=50G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore/programs/blat_for_linux -stepSize=5 -repMatch=4096 -minScore=0 -minIdentity=0 /icgc/dkfzlsdf/analysis/OE0532/static/mm9/rRNA_genes.2bit /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/fastq_to_fasta/S2_T4_late.fa /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna/blat_results/S2_T4_late.psl
bsub -q long -R "rusage[mem=50G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore/programs/blat_for_linux -stepSize=5 -repMatch=4096 -minScore=0 -minIdentity=0 /icgc/dkfzlsdf/analysis/OE0532/static/mm9/rRNA_genes.2bit /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/fastq_to_fasta/S3_T5_late.fa /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna/blat_results/S3_T5_late.psl
bsub -q long -R "rusage[mem=50G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore/programs/blat_for_linux -stepSize=5 -repMatch=4096 -minScore=0 -minIdentity=0 /icgc/dkfzlsdf/analysis/OE0532/static/mm9/rRNA_genes.2bit /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/fastq_to_fasta/S4_4T1_Ctrl.fa /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna/blat_results/S4_4T1_Ctrl.psl
bsub -q long -R "rusage[mem=50G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore/programs/blat_for_linux -stepSize=5 -repMatch=4096 -minScore=0 -minIdentity=0 /icgc/dkfzlsdf/analysis/OE0532/static/mm9/rRNA_genes.2bit /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/fastq_to_fasta/S5_T_Pool2.fa /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna/blat_results/S5_T_Pool2.psl
bsub -q long -R "rusage[mem=50G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore/programs/blat_for_linux -stepSize=5 -repMatch=4096 -minScore=0 -minIdentity=0 /icgc/dkfzlsdf/analysis/OE0532/static/mm9/rRNA_genes.2bit /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/fastq_to_fasta/S6_Liver2.fa /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna/blat_results/S6_Liver2.psl
bsub -q long -R "rusage[mem=50G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore/programs/blat_for_linux -stepSize=5 -repMatch=4096 -minScore=0 -minIdentity=0 /icgc/dkfzlsdf/analysis/OE0532/static/mm9/rRNA_genes.2bit /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/fastq_to_fasta/S7_Liver4.fa /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna/blat_results/S7_Liver4.psl
bsub -q long -R "rusage[mem=50G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore/programs/blat_for_linux -stepSize=5 -repMatch=4096 -minScore=0 -minIdentity=0 /icgc/dkfzlsdf/analysis/OE0532/static/mm9/rRNA_genes.2bit /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/fastq_to_fasta/S8_Liver6.fa /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna/blat_results/S8_Liver6.psl
bsub -q long -R "rusage[mem=50G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore/programs/blat_for_linux -stepSize=5 -repMatch=4096 -minScore=0 -minIdentity=0 /icgc/dkfzlsdf/analysis/OE0532/static/mm9/rRNA_genes.2bit /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/fastq_to_fasta/S9_Liver8.fa /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna/blat_results/S9_Liver8.psl
```

Again copy & paste this in the terminal and wait for a notification email.

The output will be in that directory: `/icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna/blat_results`

## 3. rRNA cleanup

rRNA cleanup: `python /icgc/dkfzlsdf/analysis/OE0532/software/rRNA/faster/4_rRNA_cleanup.py 20910`

The script takes the `.psl` files (output of blat), extract sequences, and remove those sequences from the initial fastq files. 

The result will be written in this directory: `ls -lh /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/clean`

```
-rw-r--r--. 1 e984a B250 138M Feb 16 16:03 S1_T2_late.fastq.gz
-rw-r--r--. 1 e984a B250 144M Feb 16 16:02 S2_T4_late.fastq.gz
-rw-r--r--. 1 e984a B250 121M Feb 16 16:02 S3_T5_late.fastq.gz
-rw-r--r--. 1 e984a B250  15M Feb 16 15:55 S4_4T1_Ctrl.fastq.gz
-rw-r--r--. 1 e984a B250 214M Feb 16 16:04 S5_T_Pool2.fastq.gz
-rw-r--r--. 1 e984a B250  81M Feb 16 15:58 S6_Liver2.fastq.gz
-rw-r--r--. 1 e984a B250 121M Feb 16 16:00 S7_Liver4.fastq.gz
-rw-r--r--. 1 e984a B250 138M Feb 16 16:01 S8_Liver6.fastq.gz
-rw-r--r--. 1 e984a B250 115M Feb 16 16:00 S9_Liver8.fastq.gz
-rw-r--r--. 1 e984a B250  74M Feb 16 15:58 S10_T_Pool1.fastq.gz
-rw-r--r--. 1 e984a B250  39M Feb 16 15:56 S11_Lung2.fastq.gz
-rw-r--r--. 1 e984a B250  32M Feb 16 15:56 S12_Lung4.fastq.gz
-rw-r--r--. 1 e984a B250  17M Feb 16 15:55 S13_Lung6.fastq.gz
-rw-r--r--. 1 e984a B250  11M Feb 16 15:55 S14_Lung8.fastq.gz
```

These files will be used for the main alignment.

## Stats

Parse blat results: `bsub -q medium -R "rusage[mem=30G]" python /icgc/dkfzlsdf/analysis/OE0532/software/rRNA/faster/3_parse_blat_results.py 20910`
This script extract the information about the alignment from the `.psl` files (output of the step 2) and groups the reads by each gene, and by the position of 5'end  of the read in the gene sequence.

Output dir 1: `/icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna/reads_per_position`
Output dir 2: `/icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna/reads_per_gene`
Output file: `less /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna_stats.txt`

The stat file `less /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/output/rrna_stats.txt` contains the following information:
```
sample  rrna_reads
S1_T2_late	64126497
S2_T4_late	25360893
S3_T5_late	23805455
S4_4T1_Ctrl	228132
S5_T_Pool2	44563805
S6_Liver2	8833560
S7_Liver4	13482745
S8_Liver6	18043788
S9_Liver8	21851476
S10_T_Pool1	7021787
S11_Lung2	1711529
S12_Lung4	1003518
S13_Lung6	1064235
S14_Lung8	407138
```
This shows number of reads aligned to rRNA genes, per sample.

The stats can be uploadedd to the web-site: `python /icgc/dkfzlsdf/analysis/OE0532/software/project_list.py rrna_genes 20910 --remote`

## Reads per gene
The reads per gene stats are shown here: http://b250-bioinfo:5000/project_info/20910

This looks as following and shows the proportion of each rRNA gene:

![](/pics/rrna_genes.png)

## Reads per position

Reads per position can also be uploaded to the web-site: `python /icgc/dkfzlsdf/analysis/OE0532/software/project_list.py reads_per_position 20910 --remote`

The plots are here: http://b250-bioinfo:5000/reads_per_position/20910

![](/pics/reads_per_position.png)
