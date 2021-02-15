# Demultiplexing
Let's take a dataset 20910 as an example. This is a mouse dataset.

## Download the data
1. When dataset is ready -> the notification will be sent by email
2. Download the data: `/icgc/dkfzlsdf/analysis/OE0532/software/diricore/utils/get_data.sh 20910`
3. After that the dataset has been downloaded to: `/icgc/dkfzlsdf/analysis/OE0532/20910`
4. If the data needs to be downloaded to another location (or under another username) then open the script and modify it:  `/icgc/dkfzlsdf/analysis/OE0532/software/diricore/utils/get_data.sh`

## Sequencing information
Now let's see what kind of dataset we received.
1. Open ILSE: `https://ilseform.dkfz.de/ilse-wui/secure/mySubmissions.jsf` and click on the dataset

We get a table which looks more or less like that: 

![table](/pics/ilse_table.png)

2. From that we know our Run ID: `210212_VH00211_26_AAAHM3LM5`. 

So we know that our raw data is here: `/icgc/dkfzlsdf/analysis/OE0532/20910/210212_VH00211_26_AAAHM3LM5` 

3. We also can see the annotation for our `.fastq` files

`AS-580536-LR-55520_R1.fastq.gz` is `24`

`AS-580538-LR-55520_R1.fastq.gz` is `25`

`AS-580540-LR-55520_R1.fastq.gz` is `26`

The same info we can get from the `meta.tsv` file which is located here: `/icgc/dkfzlsdf/analysis/OE0532/20910/20910_meta.tsv`

## Create symlinks
Before running the script, we need to put the files in the correct directory. Which is this one: `/icgc/dkfzlsdf/analysis/OE0532/20910/analysis/input/merged/`. The script will look for `.fastq` files in that directory.

So, let's create symlinks.
1. Symlink for the subset 24:  `ln -s /icgc/dkfzlsdf/analysis/OE0532/20910/210212_VH00211_26_AAAHM3LM5/AS-580536-LR-55520/fastq/AS-580536-LR-55520_R1.fastq.gz /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/input/merged/24.fastq.gz`

2. Symlink for the subset 25: `ln -s /icgc/dkfzlsdf/analysis/OE0532/20910/210212_VH00211_26_AAAHM3LM5/AS-580538-LR-55520/fastq/AS-580538-LR-55520_R1.fastq.gz /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/input/merged/25.fastq.gz`

3. Symlink for the subset 26: `ln -s /icgc/dkfzlsdf/analysis/OE0532/20910/210212_VH00211_26_AAAHM3LM5/AS-580540-LR-55520/fastq/AS-580540-LR-55520_R1.fastq.gz /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/input/merged/26.fastq.gz` 

## Create annotation files (bc_files)
To demultiplex the data, we need to create annotation files (bc_files). These files contain information about sample name and the barcode (tab separated). This info should be received from the person, who prepared the library for sequencing. 

> **_NOTE:_** The name of the `bc_file` should end with the name of the `.fastq` file, so that the script knows which bc file is for which fastq file. E.g. for the subset 24 the fastq file is named `24.fastq.gz` and bc file is named `bc_file_24.txt`

1. bc_file for the subset 24: `vim /icgc/dkfzlsdf/analysis/OE0532/20910/analysis/input/metadata/bc_file_24.txt`

```
S5_T_Pool2    ATCGT
S6_Liver2    AGCTA
S7_Liver4    CGTAA
S8_Liver6    GATCA
S9_Liver8    GCATA
```

2. bc_file for the subset 25: `/icgc/dkfzlsdf/analysis/OE0532/20910/analysis/input/metadata/bc_file_25.txt`

```
S1_T2_late    AGCTA
S2_T4_late    CGTAA
S3_T5_late    CTAGA
S4_4T1_Ctrl    GATCA
```


3. bc_file for the subset 26: `/icgc/dkfzlsdf/analysis/OE0532/20910/analysis/input/metadata/bc_file_26.txt`

```
S10_T_Pool1    ATCGT
S11_Lung2    AGCTA
S12_Lung4    CGTAA
S13_Lung6    GATCA
S14_Lung8    GCATA
```

## Trim adapter and demultiplex
1. Trim and demultiplex the subset 24: `bsub -q long -R "rusage[mem=35G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore/trim_adapter_and_demultiplex.sh 20910 24`
2. Trim and demultiplex the subset 25: `bsub -q long -R "rusage[mem=35G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore/trim_adapter_and_demultiplex.sh 20910 25`
3. Trim and demultiplex the subset 26: `bsub -q long -R "rusage[mem=35G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore/trim_adapter_and_demultiplex.sh 20910 26`

This will allow to process all 3 subsets in parallel -> will save the time. 

> *_NOTE:_* However, it is also possible just to submit one job for the whole dataset: `bsub -q long -R "rusage[mem=35G]" /icgc/dkfzlsdf/analysis/OE0532/software/diricore/trim_adapter_and_demultiplex.sh 20910`. 

In this case all subsets will be processed one by one, which increased the processing time. 

> *_NOTE:_* The command `bsub` submits the job to the cluster. The job will be running for a few hours. Once it's done (or failed), the notification will be sent by email. 

If the job has failed, the output will be included in the email. Then you need to read it. This might help to figure out what exactly went wrong.
