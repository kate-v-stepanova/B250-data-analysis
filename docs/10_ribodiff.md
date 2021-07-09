# RiboDiff (Translational Efficiency)

> **_IMPORTANT:_** `$BASE_DIR` has to be specified in your `.bash_profile`. Details [here](docs/0_before_you_start.md)

> **_IMPORTANT 2:_** This analysis will work for the datasets which have replicates. 


This analysis is performed for coupled RNA-seq and RP datasets.

Let's take as an example RP dataset `21221` and RNA-seq dataset `21221_RNA`


## Highlight specific genes

1. create list of genes: `vim $BASE_DIR/21221_RNA/analysis/input/metadata/RPG_small.txt`

This will be the list:

```
RPSA
RPS2
RPS3
RPS3A
RPS4X
RPS4Y
RPS5
RPS6
RPS7
RPS8
RPS9
RPS10
RPS11
RPS12
RPS13
RPS14
RPS15
RPS15A
RPS16
RPS17
RPS18
RPS19
RPS20
RPS21
RPS23
RPS24
RPS25
RPS26
RPS27
RPS27A
RPS28
RPS29
RPS30
```

2. copy to RP: `cp $BASE_DIR/21221_RNA/analysis/input/metadata/RPG_small.txt $BASE_DIR/21221/analysis/input/metadata/`

3. highlight genes: `module load R/3.6.2 && module load gcc/7.2.0 && Rscript $BASE_DIR/software/RiboDiff/b250_scripts/highlight_genes.r 21221_RNA all_unique $BASE_DIR/21221_RNA/analysis/input/metadata/RPG_small.txt`
4. 
