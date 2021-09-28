# Analysis of tRNAs

## Installation

here is the explanation: https://github.com/nedialkova-lab/mim-tRNAseq/blob/master/docs/source/start.rst

### 1. Create a virtual environment

```
conda create -n mimseq python=3.7
conda activate mimseq
conda config --add channels conda-forge
conda install -c conda-forge mamba
mamba install -c bioconda mimseq
```

### 2. Install usearch

```
wget https://drive5.com/downloads/usearch10.0.240_i86linux32.gz
gunzip usearch10.0.240_i86linux32.gz
chmod +x usearch10.0.240_i86linux32
mv usearch10.0.240_i86linux32 usearch
export PATH=$PATH:full/path/to/usearch
```

### 3. Install cmalign

> **_NOTE:_** check if cmalign has already been installed: `ls -lh $BASE_DIR/software/bin/cmalign`
> 
> If not, follow the instructions below


Installation instructions: http://eddylab.org/infernal/Userguide.pdf, page 10

```
wget eddylab.org/infernal/infernal-1.1.4.tar.gz
tar xf infernal-1.1.4.tar.gz
cd infernal-1.1.4
./configure --prefix $BASE_DIR/software/bin/cmalign/
make
make install
```

### 4. Install blastn

> **_NOTE:_** check if blastn has already been installed: `ls -lh $BASE_DIR/software/bin/blastn`
> 
> If not, follow the instructions below

Install blastn: `https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/`

```
wget https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ncbi-blast-2.11.0+-x64-linux.tar.gz
tar -xf ncbi-blast-2.11.0+-x64-linux.tar.gz
cd ncbi-blast-2.11.0+-x64-linux
./configure --prefix=$BASE_DIR/software/bin/blastn
make 
```

This will take a couple of hours

### 5. Install gmap

> **_NOTE:_** check if gmap has already been installed: `ls -lh $BASE_DIR/software/bin/gmap`
> 
> If not, follow the instructions below

This version: `http://research-pub.gene.com/gmap/src/gmap-gsnap-2019-02-26.tar.gz`

```
cd $BASE_DIR/software/bin/
wget http://research-pub.gene.com/gmap/src/gmap-gsnap-2019-02-26.tar.gz
tar -xf gm*
cd $BASE_DIR/software/bin/gmap-2019-02-26/
./configure --prefix=$BASE_DIR/software/bin/gmap-2019-02-26
make install
ln -s $BASE_DIR/software/bin/gmap-2019-02-26/bin $BASE_DIR/software/bin/gmap
```


## Analysis

### 1. Prepare sample_data file

Sample data file: `vim $BASE_DIR/tRNAs/analysis/input/metadata/sample_data.21012human.txt`

The file contains 2 columns (tab-separated) - 1st is the path to `.fastq` files, 2nd ist the sample name

It looks as following:

```
/icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/demultiplexed/pool_25//dem_S10_SUM159_2D.fastq S10_SUM159_2D
/icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/demultiplexed/pool_25//dem_S11_SUM159_3D.fastq S11_SUM159_3D
/icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/demultiplexed/pool_25//dem_S12_SUM159_3D_Auf.fastq S12_SUM159_3D_Auf
/icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/demultiplexed/pool_25//dem_S13_468_2D.fastq    S13_468_2D
/icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/demultiplexed/pool_25//dem_S14_468_3D.fastq    S14_468_3D
/icgc/dkfzlsdf/analysis/OE0532/21012/analysis/output/demultiplexed/pool_25//dem_S15_468_3D_Auf.fastq    S15_468_3D_Auf
```

### 2. Setup correct paths for dependencies

```
export PATH=$PATH:$BASE_DIR/software/bin/cmalign
export PATH=$PATH:$BASE_DIR/software/bin/blastn
export PATH=$PATH:$BASE_DIR/software/bin/
export PATH=$PATH:$BASE_DIR/software/bin/gmap
module load gcc/7.2.0
module load R
```

### 3. Run analysis

```
mimseq --species Hsap --cluster --cluster-id 0.95 --threads 15 --min-cov 2000 --max-mismatches 0.1 --control-condition S10_SUM159_2D -n hg19_test --out-dir $BASE_DIR/tRNAs/analysis/output/tRNAs/21012human --max-multi 4 --remap --remap-mismatches 0.075 $BASE_DIR/tRNAs/analysis/input/metadata/sample_data.21012human.txt
```

### 4. Results


error: `FileNotFoundError: [Errno 2] No such file or directory: 'cmalign': 'cmalign'`

Install cmalign: `http://eddylab.org/infernal/Userguide.pdf`, page 10

```
wget eddylab.org/infernal/infernal-1.1.4.tar.gz
tar xf infernal-1.1.4.tar.gz
cd infernal-1.1.4
./configure --prefix /icgc/dkfzlsdf/analysis/OE0532/software/cmalign/
make
make install
```
Fix `/home/e984a/.conda/envs/mimseq/lib/python3.7/site-packages/mimseq/ssAlign.py`
vim `/home/e984a/.conda/envs/mimseq/lib/python3.7/site-packages/mimseq/ssAlign.py`
search for cmalign
replace `cmcommand = ['cmalign', '-o', stkname, '--nonbanded', '-g', cmfile, tRNAseqs]` with `cmcommand = ['/icgc/dkfzlsdf/analysis/OE0532/software/cmalign/bin/cmalign', '-o', stkname, '--nonbanded', '-g', cmfile, tRNAseqs]` (line 18)

2. `mimseq --species Hsap --cluster --cluster-id 0.95 --threads 15 --min-cov 2000 --max-mismatches 0.1 --control-condition S10_SUM159_2D -n hg19_test --out-dir /icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/tRNAs/21012human --max-multi 4 --remap --remap-mismatches 0.075 /icgc/dkfzlsdf/analysis/OE0532/tRNAs/sample_data.21012human.txt`

error: `message '/bin/sh: blastn: command not found'`
Install blastn: `https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/`

```
wget https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ncbi-blast-2.11.0+-x64-linux.tar.gz
tar -xf ncbi-blast-2.11.0+-x64-linux.tar.gz
cd ncbi-blast-2.11.0+-x64-linux
./configure --prefix=/icgc/dkfzlsdf/analysis/OE0532/software/blast
make 
```
This will take a couple of hours

3. `mimseq --species Hsap --cluster --cluster-id 0.95 --threads 15 --min-cov 2000 --max-mismatches 0.1 --control-condition S10_SUM159_2D -n hg19_test --out-dir /icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/tRNAs/21012human --max-multi 4 --remap --remap-mismatches 0.075 /icgc/dkfzlsdf/analysis/OE0532/tRNAs/sample_data.21012human.txt`

Error: ` message '/bin/sh: blastn: command not found'`

Fix python source: `vim ~/.conda/envs/mimseq/lib/python3.7/site-packages/Bio/Blast/Applications.py`

Line 558 - replace `def __init__(self, cmd="blastn", **kwargs):` with `def __init__(self, cmd="/icgc/dkfzlsdf/analysis/OE0532/software/blast/bin/blastn", **kwargs):`

4. `bsub -q long -R "rusage[mem=40G]" mimseq --species Hsap --cluster --cluster-id 0.95 --threads 100 --min-cov 2000 --max-mismatches 0.1 --control-condition S10_SUM159_2D -n hg19_test --out-dir /icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/tRNAs/21012human --max-multi 100 --remap --remap-mismatches 0.075 /icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/input/metadata/sample_data.21012human.txt`

## Faster dataset (less samples, less reads)
1. Create sample data: `vim $BASE_DIR/tRNAs/analysis/input/metadata/sample_data.21221.txt`

```
/icgc/dkfzlsdf/analysis/OE0532/21221/analysis/output/demultiplexed/FLP025//dem_S10_sh40_1.fastq S10_sh40_1
/icgc/dkfzlsdf/analysis/OE0532/21221/analysis/output/demultiplexed/FLP025//dem_S11_sh40_2.fastq S11_sh40_2
/icgc/dkfzlsdf/analysis/OE0532/21221/analysis/output/demultiplexed/FLP025//dem_S7_sh12_1.fastq  S13_sh12_1
/icgc/dkfzlsdf/analysis/OE0532/21221/analysis/output/demultiplexed/FLP025//dem_S8_sh12_2.fastq  S14_sh12_2
```

2. `bsub -q long -R "rusage[mem=40G]" mimseq --species Hsap --cluster --cluster-id 0.95 --threads 100 --min-cov 2000 --max-mismatches 0.1 --control-condition S10_sh40_1 -n hg19_test --out-dir $BASE_DIR/tRNAs/analysis/output/tRNAs/21221_25 --max-multi 100 --remap --remap-mismatches 0.075 $BASE_DIR/tRNAs/analysis/input/metadata/sample_data.21221.txt`

Error: 

```
File "/home/e984a/.conda/envs/mimseq/lib/python3.7/site-packages/mimseq/tRNAtools.py", line 429, in modsToSNPIndex
subprocess.check_call(cluster_cmd, stdout = subprocess.DEVNULL, stderr=subprocess.DEVNULL)
File "/home/e984a/.conda/envs/mimseq/lib/python3.7/subprocess.py", line 358, in check_call
retcode = call(*popenargs, **kwargs)
File "/home/e984a/.conda/envs/mimseq/lib/python3.7/subprocess.py", line 339, in call
with Popen(*popenargs, **kwargs) as p:
File "/home/e984a/.conda/envs/mimseq/lib/python3.7/subprocess.py", line 800, in __init__
restore_signals, start_new_session)
File "/home/e984a/.conda/envs/mimseq/lib/python3.7/subprocess.py", line 1551, in _execute_child
raise child_exception_type(errno_num, err_msg, err_filename)
FileNotFoundError: [Errno 2] No such file or directory: 'usearch': 'usearch'
```

Install usearch: 

```
cd /icgc/dkfzlsdf/analysis/OE0532/software
wget https://drive5.com/downloads/usearch10.0.240_i86linux32.gz
gzip -d usearch10.0.240_i86linux32.gz
mv usearch10.0.240_i86linux32 usearch
chmod +x usearch
```


To fix: `vim /home/e984a/.conda/envs/mimseq/lib/python3.7/site-packages/mimseq/tRNAtools.py`

replace in lines 427, 431 and 434 `usearch` with `/icgc/dkfzlsdf/analysis/OE0532/software/usearch`

E.g: `cluster_cmd = ["usearch", "-cluster_fast", temp_dir + anticodon + "_allseqs.fa", "-id", str(cluster_id), "-sizeout" ,"-centroids", temp_dir +      anticodon + "_centroids.fa", "-uc", temp_dir + anticodon + "_clusters.uc"]` 
replace with: `cluster_cmd = ["/icgc/dkfzlsdf/analysis/OE0532/software/usearch", "-cluster_fast", temp_dir + anticodon + "_allseqs.fa", "-id", str(cluster_id), "-sizeout" ,"-centroids", temp_dir +      anticodon + "_centroids.fa", "-uc", temp_dir + anticodon + "_clusters.uc"]`

Restart: `bsub -q long -R "rusage[mem=40G]" mimseq --species Hsap --cluster --cluster-id 0.95 --threads 40 --min-cov 2000 --max-mismatches 0.1 --control-condition S10_sh40_1 -n hg19_test --out-dir /icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/tRNAs/21221_25 --max-multi 40 --remap --remap-mismatches 0.075 /icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/input/metadata/sample_data.21221.txt`

Take GSE dataset: https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE152621

```
wget https://sra-download.ncbi.nlm.nih.gov/traces/sra59/SRR/011744/SRR12026342   
mv SRR12026342 Hsap_iPSC_rep1.rsa
wget https://sra-download.ncbi.nlm.nih.gov/traces/sra5/SRR/011744/SRR12026343    
mv SRR12026343 Hsap_iPSC_rep2.rsa
wget https://sra-download.ncbi.nlm.nih.gov/traces/sra53/SRR/011744/SRR12026344   
mv SRR12026344   Hsap_HEK293T_rep1.rsa
wget https://sra-download.ncbi.nlm.nih.gov/traces/sra46/SRR/011744/SRR12026345   
mv SRR12026345   Hsap_HEK293T_rep2.rsa
wget https://sra-download.ncbi.nlm.nih.gov/traces/sra79/SRR/011744/SRR12026346   
mv SRR12026346   Hsap_K562_rep1.rsa
wget https://sra-download.ncbi.nlm.nih.gov/traces/sra8/SRR/011744/SRR12026347    
mv SRR12026347    Hsap_K562_rep2.rsa
```

Convert sra to fastq

`for f in $(ls /icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/input/sra/*.sra); do fn=${f%.sra}.fastq; echo "bsub -q medium -R \"rusage[mem=30G]\" /icgc/dkfzlsdf/analysis/OE0532/software/sratoolkit.2.9.6-1-centos_linux64/bin/fastq-dump $f -O /icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/demultiplexed --gzip"; done`

Create sample_data: `vim /icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/input/metadata/sample_data.GSE.txt`

```
/icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/demultiplexed/Hsap_HEK293T_rep1.fastq.gz   HEK293T_R1
/icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/demultiplexed/Hsap_HEK293T_rep2.fastq.gz   HEK293T_R2
/icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/demultiplexed/Hsap_iPSC_rep1.fastq.gz  iPSC_R1
/icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/demultiplexed/Hsap_iPSC_rep2.fastq.gz  iPSC_R2
/icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/demultiplexed/Hsap_K562_rep1.fastq.gz  K562_R1
/icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/demultiplexed/Hsap_K562_rep2.fastq.gz  K562_R2
```


Run mimseq: `bsub -q verylong -R "rusage[mem=80G]" mimseq --species Hsap --cluster --cluster-id 0.95 --threads 40 --min-cov 2000 --max-mismatches 0.1 --control-condition HEK293T_R1 -n hg19_test --out-dir /icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/tRNAs/GSE --max-multi 40 --remap --remap-mismatches 0.075 /icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/input/metadata/sample_data.GSE.txt` 

### Install gmap
This version: `http://research-pub.gene.com/gmap/src/gmap-gsnap-2019-02-26.tar.gz`

```
cd /icgc/dkfzlsdf/analysis/OE0532/software/bin/
wget http://research-pub.gene.com/gmap/src/gmap-gsnap-2019-02-26.tar.gz
tar -xf gm*
cd /icgc/dkfzlsdf/analysis/OE0532/software/bin/gmap-2019-02-26/
./configure --prefix=/icgc/dkfzlsdf/analysis/OE0532/software/bin/gmap-2019-02-26
make install
```

replace path: `vim /home/e984a/.conda/envs/mimseq/lib/python3.7/site-packages/mimseq/tRNAtools.py`
replace `gmap` with `/icgc/dkfzlsdf/analysis/OE0532/software/bin/gmap-2019-02-26/bin/gmap`

Run mimseq: `bsub -q verylong -R "rusage[mem=80G]" mimseq --species Hsap --cluster --cluster-id 0.95 --threads 40 --min-cov 2000 --max-mismatches 0.1 --control-condition HEK293T_R1 -n hg19_test --out-dir /icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/tRNAs/GSE --max-multi 40 --remap --remap-mismatches 0.075 /icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/input/metadata/sample_data.GSE.txt` 

load modules:
```
module load samtools
module load bedtools
module load R/3.3.1
module load gcc/7.2.0
bsub -q verylong -R "rusage[mem=80G]" mimseq --species Hsap --cluster --cluster-id 0.95 --min-cov 2000 --max-mismatches 0.1 --control-condition HEK293T_R1 -n hg19_test --out-dir /icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/tRNAs/GSE /icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/input/metadata/sample_data.GSE.txt --keep-temp
```

bsub -q verylong -R "rusage[mem=80G]" mimseq --species Hsap --cluster --cluster-id 0.95 --min-cov 2000 --max-mismatches 0.1 --control-condition HEK293T_R1 -n hg19_test --out-dir /icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/tRNAs/GSE /icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/input/metadata/sample_data.GSE.txt

## Deseq2 -> run locally
1. Create dirs: `~/analysis/tRNAs/data/GSE/`
2. Copy files:

```
scp -r e984a@odcf-cn34u03s10:/icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/tRNAs/GSE/sample_data_cov.txt ~/analysis/tRNAs/data/GSE/

scp -r e984a@odcf-cn34u03s10:/icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/tRNAs/GSE/Anticodon_counts.txt ~/analysis/tRNAs/data/GSE/

scp -r e984a@odcf-cn34u03s10:/icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/tRNAs/GSE/Isodecoder_counts.txt ~/analysis/tRNAs/data/GSE/

scp -r e984a@odcf-cn34u03s10:/icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/tRNAs/GSE/*isoacceptorInfo.txt ~/analysis/tRNAs/data/GSE/

scp -r e984a@odcf-cn34u03s10:/icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/tRNAs/GSE/*clusterInfo.txt ~/analysis/tRNAs/data/GSE/

scp -r e984a@odcf-cn34u03s10:/icgc/dkfzlsdf/analysis/OE0532/tRNAs/analysis/output/tRNAs/GSE/*isodecoderInfo.txt ~/analysis/tRNAs/data/GSE/

```
