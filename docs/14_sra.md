
# SRA toolkit

Binary should be located here: `$BASE_DIR/software/bin/sratoolkit_fastq_dump`

## Software

Installation on the cluster has already been done. 

Download this version `compiled CentOS (May 7, 2019)`: https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/2.9.6-1/sratoolkit.2.9.6-1-centos_linux64.tar.gz

Installation guide: https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=software

## Create directory structure

Let's create a directory for a dataset with the name `SRA_dataset`

```
mkdir $BASE_DIR/SRA_dataset/analysis/input/sra
```

This is a location for the `.sra` files.

## Download dataset

Select sample:

![](/pics/sra_1.png)

Find a link to SRA:

![](/pics/sra_2.png)

Select run:

![](/pics/sra_3.png)

Data access tab:

![](/pics/sra_4.png)

Copy link for download:

![](/pics/sra_5.png)

Download `.sra` file:

```
cd $BASE_DIR/SRA_dataset/analysis/input/sra
wget https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos3/sra-pub-run-20/SRR10491341/SRR10491341.1
```

Rename files (from sra_id to sample name):

```
mv SRR10491341.1 WT_1.sra
```

## Convert SRA to fastq

Convert SRA to fastq: `$BASE_DIR/software/sra/sra_to_fastq.sh mito_riboseq`

`mito_riboseq` is a a project_id

the `.sra` files should be located in: `$BASE_DIR/mito_riboseq/analysis/input/sra/*.sra` 

and the output will be written to: `$BASE_DIR/mito_riboseq/analysis/output/demultiplexed`

The command will output the following:

```
bsub -q long -R "rusage[mem=20G]" /omics/groups/OE0532/internal/from_snapshot//software/bin/sratoolkit_fastq_dump /omics/groups/OE0532/internal/from_snapshot//mito_riboseq/analysis/input/sra/SHMT2_1.sra -O /omics/groups/OE0532/internal/from_snapshot//mito_riboseq/analysis/output/demultiplexed --gzip
bsub -q long -R "rusage[mem=20G]" /omics/groups/OE0532/internal/from_snapshot//software/bin/sratoolkit_fastq_dump /omics/groups/OE0532/internal/from_snapshot//mito_riboseq/analysis/input/sra/SHMT2_2.sra -O /omics/groups/OE0532/internal/from_snapshot//mito_riboseq/analysis/output/demultiplexed --gzip
bsub -q long -R "rusage[mem=20G]" /omics/groups/OE0532/internal/from_snapshot//software/bin/sratoolkit_fastq_dump /omics/groups/OE0532/internal/from_snapshot//mito_riboseq/analysis/input/sra/WT_1.sra -O /omics/groups/OE0532/internal/from_snapshot//mito_riboseq/analysis/output/demultiplexed --gzip
bsub -q long -R "rusage[mem=20G]" /omics/groups/OE0532/internal/from_snapshot//software/bin/sratoolkit_fastq_dump /omics/groups/OE0532/internal/from_snapshot//mito_riboseq/analysis/input/sra/WT_2.sra -O /omics/groups/OE0532/internal/from_snapshot//mito_riboseq/analysis/output/demultiplexed --gzip
```

Copy & paste to submit jobs to the cluster.

This will convert `.sra` to `.fastq.gz` and the files will be written here: `$BASE_DIR/SRA_dataset/analysis/output/demultiplexed`

## Trimming adapter

Usually these datasets have a different adapter sequence, e.g. `TCGTATGCCGTCTTCTGCTTGAA` (this information can usually be found in the description of the dataset)

This adapter can be trimmed as following:

```
$BASE_DIR/software/preprocessing/1_trim_any_adapter.sh GSE48933 TCGTATGCCGTCTTCTGCTTGAA 25
```

This will remove adapter from the reads and will output the results to the `trimmed` directory: `/omics/groups/OE0532/internal/from_snapshot//mito_riboseq/analysis/output/trimmed` 

Additionally, symlinks will be created in the directory: `/omics/groups/OE0532/internal/from_snapshot//mito_riboseq/analysis/input/fastq` 

From this step, the further preprocessing should be performed as usually (rRNA cleanup, alignment, etc)
