# Analysis of tRNAs (mimseq)

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

> **_NOTE:_** check if usearch has already been installed: `ls -lh $BASE_DIR/software/bin/usearch`
> 
> If not, follow the instructions below

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

Install blastn: https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/

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

This version: http://research-pub.gene.com/gmap/src/gmap-gsnap-2019-02-26.tar.gz

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
module load samtools
module load bedtools
module load gcc/7.2.0
module load R
```

### 3. Run analysis

```
mimseq --species Hsap --cluster --cluster-id 0.95 --threads 15 --min-cov 2000 --max-mismatches 0.1 --control-condition S10_SUM159_2D -n hg19_test --out-dir $BASE_DIR/tRNAs/analysis/output/tRNAs/21012human --max-multi 4 --remap --remap-mismatches 0.075 $BASE_DIR/tRNAs/analysis/input/metadata/sample_data.21012human.txt --keep-temp
```
